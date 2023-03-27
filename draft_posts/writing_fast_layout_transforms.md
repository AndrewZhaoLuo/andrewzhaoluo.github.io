As part of my job, I've been recently learning a lot about GPU programming and CUDA in particular. 
In this post we are going to talk about an inocuous but fairly common deep learning operator: `layout_transform`/`transpose`
and a performant implementation on modern GPUs. As the operator itself has no computation but simply 
transforms memory, it's also useful for learning the basics of maximizing the memory bandwidth in GPUs. The 
first part of the post will discuss a sketch for a good implementation of the operator. The second part
will deal with ugly real world addenda and talk about fusion and how this can help hide
latency. The third part will be a more philosophical discussion on this operator and it's relationship with 
deep learning frameworks.

## Memory Access and Layouts
In most programming languages and deep learning frameworks, tensors are laid out in row-major order. What this means
is when view the tensor in actual memory, small deltas in indexing in the inner-most dimensions lead to smaller
jumps in address than outer dimensions. In other words, inner dimensions are more contiguous. We can use this to reason
about local memory accesses. Consider a canonical representation of matrix multiplication:

```
def matmul(A, B, C):
    # A: [M, K] @ B: [K, N] -> C: [M, N]
    # Calculate C = A @ B
    for i in range(M):
        for j in range(N):
            C[i, j] = 0
            for k in range(K):
                C[i, j] += A[i, k] * B[k, j]
```

The inner most loops are executed sequentially, so the `k` variable is what will influence local access patterns the most (assuming it is large enough) followed by the iteration represented by `j`.
 `k` is used  to access the inner dimension of `A` and the outer dimension of `B` meaning we will have good spatial locality in accessing
`A` but not `B`. 

Let's say we want better locality for C and B. One thing we can do in this case, is rearrange the axis. This will produce the same result with some slight changes:

```
def matmul(A, B, C):
    # A: [M, K] @ B: [K, N] -> C: [M, N]
    # Calculate C = A @ B
    for i in range(M):
        for j in range(N):
            C[i, j] = 0
    
    for k in range(K):
        for i in range(M):
            for j in range(N):
                C[i, j] += A[i, k] * B[k, j]
```

Note we had could no longer in-line our initialization of `C[i, j] = 0`. However, now `j` is our inner most loop
which accesses the inner dimension of `C` and `B`. Now we have two buffers with more local access!

All of this is to say important because we want to bring in numbers as quickly as possible to our computational units no matter
what compute platform you are using; in most hardware memory throughput severely lags behind instruction execution throughputs.
Accessing things more locally let's us make better use of caching in CPU, and though the code is written above for CPU, the analysis
can be easily extended to GPU as we talk above. 

## Layout Transform
Going back to the example above, if we want both `A` and `B` to have local accesses we have to make sure they share the inner most iterator. 
This is impossible as for `A` this is `A[i, *k*]` while for `B` this is `B[k, *j*]`. However, if the layout of `B` was transposed into `Bt` 
such that `Bt[i][j] = B[j][i]` than if we operate on `Bt`:

```
def matmul(A, Bt, C):
    # A: [M, K] @ Bt: [N, K] -> C: [M, N]
    # Calculate C = A @ B
    for i in range(M):
        for j in range(N):
            C[i, j] = 0
    
    for k in range(K):
        for i in range(M):
            for j in range(N):
                C[i, j] += A[i, k] * Bt[j, k]
```

Then we see our new code has locality for both `A` and `Bt`! So to make memory access patterns better, we have two tools in our toolbox:

- Rearranging loops
- Transforming memory layouts

Rearranging loops is quite a complex topic and I include discussion on blocking/tiling in this family of optimizations. Software like Halide and TVM contain transformations for certain loop based expressions which can modify the ordering and shape of loop iteration. This is a topic I do not fully understand and has a lot of depth and theory. 

Meanwhile transforming memory layouts makes a lot more sense (☞ ͡° ͜ʖ ͡°)☞? Convolution is often times the operator of interest which suffers from layout transform issues. In particular, while 

- TODO
- Talk about why NHWC is probably what you want for GPU
  - Reduction across channel in by channel out
  - NCHW is more popular...
- Why minimizing layout transform is needed
  - Easy ways to propagate this

## An Optimal Layout Transform on GPU
So now we have established that layout transformations should be minimized, we have also seen that sometimes they might be unavoidable in various scenarios; there is no perfect layout and sometimes it's worth moving back and forth between layouts. 

Let's then talk about implementing generalized layout transforms. In PyTorch this is analogous to "torch.permute", though keeping with the high level nature of the framework, it merely returns a view of the new tensor over the old one, not necessarily moving memory to be more local in the way you want.

To derive an efficient operator for layout transforms, let us focus on the simplest case first: transposing a 2D matrix. Let's say we have a matrix `A` with shape `[512, 1024]` and we want to output a matrix `B` of shape `[1024, 512]` such that for all valid `i, j` `A[i][j] = B[j][i]`. A naive implementation might be something like:

```
template <typename T, int rows, int cols, int loads_per_thread,
          int num_threads_per_block>
__global__ void CudaTransposeNaive(T *mat, T *matT) {
  int y_start = blockIdx.y * loads_per_thread;
  int x_start = blockIdx.x * num_threads_per_block + threadIdx.x;
  for (int i = 0; i < loads_per_thread; i++) {
    matT[x_start * rows + (y_start + i)] = mat[(y_start + i) * cols + x_start];
  }
}

// Specialization:
CudaTransposeNaive<float, 512 /*rows*/, 1024 /*cols*/, 1 /*loads_per_thread*/, 1024 /*num_threads_per_block*/>
```

I personally think it's a lot easier to read in a form like:
```
def cuda_transpose_naive(A, B):
    for r in range(512, bind='blockX'):
        for c in range(1024, bind='threadX'):
            B[c][r] = A[r][c]
```

I think this form is very easy to reason about local memory accesses. `A` and `B` are both in global memory, where to maximize throughput we need to have `coalesced memory accesses` (see link here). To do this, we must ideally have the most local accesses into a tensor be bound to threadX. We see from the above that `A` will have coalesced memory access but `B` cannot. Worse, we know we will always be stuck like this because we cannot 

## Hiding Latency
Of course it would be really cool if we could hide the data movement associated with layout transforms by intermixing it with heavy computation. For example, in Goto's famous examination of CPU matrix multiplications he noticably has "packing" operations why change submatricies which are not normally contiguous into contiguous entries. This is done within the actual kernel, and not done as a prologue to the main body of computation, allowing the fancy machinary and pipelining within the CPU to hide the data movement while doing computation, amortizing cost! Can this be done in general computation? Probably not, but there are probably some general subset of cases in the world of deep learning operators!