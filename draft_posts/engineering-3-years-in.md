## Reflections After 3 Years of Software Engineering

I can't believe it's been 3 years since I started my first real job out of college!
It's a real milestone, so I thought it might be good to think back and reflect on 
some of the things I've learned in the meantime. 

---
#### The Importance of Working Quickly
I actually think I have an inherent bias toward working quickly, so in a sense
I've always followed this advice through my career! Of course, you can do right things
for the wrong reasons, and my past mindset of moving fast and breaking things was 
probably fed more by my impatience over anything else. Luckily, even if I 
was doing the right things for the wrong reason, I was still doing the right thing 
and gaining the benefits.

Working quickly on stuff I would assign would typically result
in me getting more work sooner, which in turn led me to touch larger parts of the 
codebase. Moving quickly opens up more learning, and can also help your coworkers 
trust you more as you successfully do more varied assignments. Even if you don't understand
everything you wrote perfectly, that's usually ok since you have the safety nets of testing
and your peers who review your code to help you not make egregious mistakes. In fact,
code review is perhaps the only effective way of learning and gaining tribal knowledge of 
the systems you are working on. Moving quickly therefore allows you to iterate and get 
feedback more quickly, again helping the whole learning feedback loop.

I first started contributing to [Apache TVM](https://github.com/apache/tvm), I had to support
a new operator in the framework. This is a very complicated [process](https://tvm.apache.org/docs/dev/how_to/relay_add_op.html) unfortunately. Because it was so complex, I will admit I had no idea what I was doing perfectly, but I made enough assumptions to get the job done. During the review process, I was referred to several similar PRs, which helped me focus on which part of the codebase would be most useful to understand. If I tried to learn each component of adding an operator to TVM to completion, it would have taken like 6 months to get anything done, my coworkers would probably have lost faith in me, and it would have actually been an inefficient way to learn TVM. Some stuff just makes more sense in different contexts. For example, TVM has it's own object system implemented as C++ templates which add features like automatic reference counting and Python support. Learning how to write compiler passes to manipulate higher level representations of code probably taught me more of the object system than it would if I jsut studied it when trying to add an operator since you have to not only use the data stored in objects, but create objects too. I still don't understand large parts of TVM, but the deep knowledge I do have I gained by seeing parts of the systems from many different vantage points.

There are however tradeoffs to working quickly. Quickness is often synonymous with hacks and haphazard implementations. Ideally, for any jobs you have to balance all of the tradeoffs between
different implementations and styles of work. For example, it doesn't make sense to do excessive engineering for a one-off demo. That being said, even for big and hard problems, quick prototypes to test out ideas and assess variability have their place in my opinion. I say this with greater confidence since short term forks seem to be common among my knowledgeable coworkers.

#### The Importance of Working Methodically 
I think for at least "easy" problems we can all agree on the improtance of working throuhg problems quickly. But as I get more experienced, I start running into much more complicated problems, where hacking and churning out prototypes really isn't the bottleneck in getting stuff done. I'm talking about stuff that requires a lot more knowledge and complexity. Things like thinking about how to increase the inference speed or accuracy of a model. Here because of their nature, how to tackle the problems is a bit more open ended and a lot of approaches sound reasonable. Personally, I think I've always struggled in situations like this since I often get overwhelmed by the amount of approaches I can take.

Here I've found simply being organized is super important. When working on a complicated problem, there are a lot of different angles to attack something. Most recently, I've been interested in doing performance work, seeing how we can speed up the inference latency of various machine learning models we have on hand. In the past, in the name of working quickly, I would probably quickly hop from one trying one idea to another idea. 

#### Make it Simple to do the "Right" Things
Speed 

#### Always Think About the Bigger Picture 

