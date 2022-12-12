## Reflections After 3 Years of Software Engineering

I can't believe it's been 3 years since I started my first real job out of college!
It's a real milestone, so I thought it might be good to think back and reflect on 
some of the things I've learned in the meantime. 

---
#### The Importance of Working Quickly
I actually think I have an inherent bias toward working quickly, so in a sense
I've always followed this advice through my career! Of course, you can do right things
for the wrong reasons, and my past mindset of moving fast and breaking things was 
probably fed more by my impatience and laziness over anything else. After all,
If I could finish my project a bit earlier than expected, I could use the remaining 
time I had to fool around. 

Despite this mindset, working quickly on stuff I would assign would typically result
in me getting more work sooner, which in turn led me to touch larger parts of the 
codebase. Moving quickly opens up more learning, and can also help your coworkers 
trust you more as you successfully do more varied assignments. Even if you don't understand
everything you wrote perfectly, that's usually ok since you have the safety nets of testing
and your peers who review your code to help you not make egregious mistakes. In fact,
code review is perhaps the only effective way of learning and gaining tribal knowledge of 
the systems you are working on. Moving quickly therefore allows you to iterate and get 
feedback more quickly, again helping the whole learning feedback loop.

I first started committing to [Apache TVM](https://github.com/apache/tvm), I had to support
a new operator in the framework. This is a very complicated [process](https://tvm.apache.org/docs/dev/how_to/relay_add_op.html) which should probably be improved somehow in the future. Because it was so complex, I will admit I had no idea what any of the systems I was modifying was doing
perfectly, but I made enough assumptions to get the job done. In the review step, I learned the improtance of reading other ML framework's documentation to decide what variants of an operator to support. If I tried to learn each component of adding an operator to TVM, it would have taken like 6 months to get anything done, my coworkers would probably have lost faith in me, and it would have actually been the least efficient way to learn TVM! For example, TVM has it's own unique object system template which is used everywhere. Learning how to write compiler passes to manipulate higher level representations of code probably taught me more of the object system than it would if I jsut studied it when trying to add an operator. Even today, there are components, like tensor expressions, how TVM implements ABIs, and TVM's object system which I don't understand fully, but the knowledge I do have, I gained by moving quickly and seeing the parts of the systems from many different vantage points.

There are however tradeoffs to working quickly. Quickness is often synonymous with hacks and haphazard implementations. Ideally, for any jobs you have to balance all of the tradeoffs between
different implementations and styles of work. For example, it doesn't make sense to do excessive engineering for a one-off demo. That being said, even for big and hard problems, quick prototypes to test out ideas and assess variability have their place.

#### The Importance of Working Methodically 
I think for at least "easy" problems we can all agree on the improtance of working throuhg problems quickly. But as I get more experienced, I start running into much more complicated problems, where hacking and churning out prototypes really isn't the bottleneck in getting stuff done. I'm talking about stuff that requires a lot more knowledge and complexity. Things like thinking about how to increase the inference speed or accuracy of a model. Here because of their nature, how to tackle the problems is a bit more open ended and a lot of approaches sound reasonable. Personally, I think I've always struggled in situations like this since I often get overwhelmed by the amount of approaches I can take.

Here 

#### Make it Simple to do the "Right" Things
Speed 

#### Always Think About the Bigger Picture 

