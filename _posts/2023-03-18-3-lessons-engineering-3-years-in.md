# My Top 3 Lessons After 3 Years of Software Engineering
Awww yeah, I'm now getting older and wiser 😎. Given the seniority of my new position, I thought it wise to reflect upon the top 3 lessons I've developed over the past 3 years of my career. Hopefully this helps a youngin' out there with something or another.

## Iteration is Everything
The world is constantly changing so we should be constantly changing and updating our internal models on how things work. Having accurate information on the world and how it works is how you execute accurately, quickly, and consistently. "Iteration" is probably the best word for naming this process and I think it's the most important thing about executing well as a software engineer.

I was originally going to title this "Working Fast is Everything" but I realized that there was a bit more nuance to what I was trying to say. Doing this process (aka "iteration") *quickly* is important, but to truly iterate involves making a thoughtful change in response to a new experience. Good changes require high quality reflection. This in return requires time and energy. We may think about working super quickly by putting in tons of hours, but that will quickly cause burn-out and lower the quality of both your work and your reflection. 

Here we have a bit of an optimization problem. We can work super quickly and put little thought into reflection leading to many fast cycles of poor iteration. We can work also work super slowly, putting a lot of thought into book learning before trying new things leading to few slow cycles of good iteration. In practice, the balance between speed and quality of iteration is somewhere in the middle and figuring out the right balance is itself a task of iteration. Your mileage may vary but personally, but alternating between fast and slow cycles is often times a good idea. At the start of a new project a few fast cycles is good to get a sense of some basic context. It turns out a lot of documentation sucks, so having this basic context is needed to access book knowledge to it's fullest. After moving fast, a few slower cycles on reading through documents and references becomes worth it. Afterwards, a few fast cycles are good to play and test your knowledge. Then you might do a few more slow cycles to test your new knowledge... and fast and slow cycles alternate again and again.

## Respect Authorial Intent
I think the attitude I've developed toward approaching existing code is trying to understand authorial intent. It's often the first thought of a junior software engineer to rewrite entire systems from scratch, mistaking their lack of understanding for poor design. No matter how much we document some pieces of code, at the end of the day, some things are just complex and have a lot of decision making wrapped up in them. Engineering is the art of tradeoffs, and understanding design choices and trade-offs are one of the most interesting aspects of engineering. Not only does it raise "the bar" for a good job on your end when you realize how well designed some things are (e.g. [rice cookers](https://www.youtube.com/watch?v=RSTNhvDGbYI)), but it helps build a little bit of empathy and understanding for the entire engineering process. For example, if you pay attention, you notice some decisions are made very deliberately while others are afterthoughts. For example, maybe a piece of performance critical code uses the correct data structures, but was written in Python since that was the only language the person who wrote it knew. If you try to "improve" code without understanding why it was written in the first place, you might show yourself to have less thought than the original engineer. Meanwhile, if you identify the afterthoughts and tradeoffs, you now know obvious areas of improvement. Rather than throw perfectly good knowledge away, why not try to stand on the shoulder of giants and improve upon their existing design?

## The Problem is Inconsistency 
For all popular conception likes to paint software engineers as anti-social people who would rather hang out with electronics than people, most problems you encounter in computer science are related to dealing with humans:

- Building the right piece of software people want to use 
- Making the codebase readable and easy to contribute to
- Making it hard to do dumb/incorrect things
- Making it easy for external uses to interact with your software (e.g. APIs) 
- Spreading tribal knowledge which may be integral to understanding a system

Anyone who builds good software is thinking about people somewhere down the line whether that be the users or future engineers responsible for the system in the future. The best software engineers I have met can take complicated ideas and make them approachable to any idiot (e.g. myself) and thinks about how *people* will interact with their code. The conception that engineers enjoy machines more than humans is inaccurate. Instead, I think the typical software engineer likes *consistency* and dislikes *inconsistency* more so than anything else. The stereotype of anti-social engineers makes sense in this context since people are capricious and hypocritical. With almost every experience in life, we build up mental models using the best available information we have at the time. We work best in new situations when we are able to connect a scenario to more fundamental truths established by our models, and it can be jarring when these models are broken. One simple idea is having consistent and informational naming within a codebase. For example, if a method is called `do_foo()` it better do "foo" when it's called:

```
def foo():
    # do foo
    ...
```

If later we refactor business logic so we only want to conditionally do foo and otherwise do nothing within a system, it would be really really bad to push condition checking and dispatch into `do_foo()` without changing the name to something like `conditionally_do_foo()`: 

```
# Bad 
def do_foo(condition):
    if not condition:
        return
    
    # do foo
    ...

# Maybe better
def conditionally_do_foo(condition):
    if not condition:
        return
    
    # do foo
    ...
```

It's probably best however to move conditional checking and dispatch to outside of the method than inside:

```
# The best probably
def do_foo():
    # do foo
    ...

def business_logic_blah(...):
    if condition:
        do_foo()
    else:
        # whatever you need here
        ...
```

Other people will have a lot better time reasoning about your code.

I think that captures the essence of the lesson, make whatever you are doing easy to reason about to other people! Be clear, be consistent, and try to fit your ideas within common mental models of the time.
