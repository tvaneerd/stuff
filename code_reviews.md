Thoughts on Code Reviews (and more)
========================

### 0. Do Them

**Do Code Reviews. Somehow. Any how.**

### This is really a diatribe on when to catch bugs

The worst place to catch bugs is in production.  The best is during code review - no, wait, that's not soon enough.  
The best time to catch bugs is before the code is written - design time.

Some of this seems to contradict "agile".  It need not.
Taking time to design something doesn't mean you need to design it beginning to end.
But at least get an idea of where you might be headed.

_Quickly iterating_ doesn't mean _skipping_.

```
$                                                                                                              $$$$$$
*---------------*------------*----------*---------*---------*--------*----------------------------------------------*
Design 1,2    Writing    Compiling     Tests*    Debug    Review*    QA                                    Production
```

Design 1 - feature design  
Design 2 - code design  
Tests* - could/should be between Design and Writing, and should be Unit and System  
Review* - can happen at multiple stages!

### Feature Design

Don't focus on features.  Focus on problems that need to be solved.
I've seen feature requests from users like "please add a short-cut key for Save-As, preferably one that auto-names with an incrementing number".
Obviously what they really want is a version control system.  Or a way to experiment within the app, maybe show multiple scenarios at once, compare them, etc.

Always think _What is the real problem behind the request?_

Don't be afraid to have a vision.  A vision can be quick, doesn't need to be designed in detail. (But does need enough thought to have some likelihood of feasibility.)
An example vision might be "The real user problem is they are constantly tranfering data between apps - let's make that seamless."
How is it going to be seamless. Well, in the end maybe it won't.  Maybe it it will just be less seams. Cut and paste? Import/export/ Plugins? Have an ideal, that may not be reached, but gives a direction to head.

An idea of direction helps.  You really don't want detail.  Because detail will be wrong.
But high level direction leaves lots of room for experimentation and agile.

> “A goal is not always meant to be reached, it often serves simply as something to aim at.” - Bruce Lee


### _Pre-code_ reviews

I am amazed how often I hear "You probably should have written this like X instead of like Y". :-(

Once, at Adobe, a developer spent about a month implementing an important and complicated feature.
We decided to do a code review - we hadn't (yet) instituted a process for code reviews, we were still feeling our way around.
The lead architect noted (correctly) that the feature really needed to be threaded,
otherwise the main thread could (in some reasonable situations) stall badly.
(This was many years ago, when threading was less common.)

The conclusion of the code review was, yes, lots of rewrite was necessary, as it needed to be threaded - even at the cost of missing a ship date.
(As you can imagine, adding threads after-the-fact isn't always straightforward, particularly with code that mixed processing with UI feedback.)

So what was the lesson to be learned?

Code reviews work?  An issue, which would have caused real problems for users, was found early.  We saved time and money!

Yes, true.

However, bigger lesson - we wasted a month (or a good portion thereof) because no one (or no senior dev) asked the developer, before they started, a simple question:

> "any idea how you think this is going to work"?

_Pre-code Reviews_ ie code-design reviews, are the best code reviews.

Sure you don't always know how the code is going to go, sometimes you just need to dive in and start coding.
But if so, discuss it with someone as you go.  Pairwise programming is another way to do this.

_The very best programmers I've ever worked with_, those rare devs who didn't need to discuss their code because honestly it was just always naturally done the right way,
were the ones that nonetheless _discussed their code the most_ - before they were done writing it. They questioned their choices. They sought feedback, sounding boards.

#### Make a habit of talking about code

For senior devs or whomever the team follows: talk about code, at the class level, function level.  Make it a habit for your team to talk about code.

Not features, not deadlines.  Code.  Developers love code.  That's what makes them developers.  The most introverted dev will still light up discussing code.

When someone says "I'm working on feature X" (ie during standup) ask what classes they are working on, or what cpp files they are touching.
Some devs get scared by these questions (particularly when they come from senior devs) - it can sound like prying, watching over their shoulders.
Feel free to imply a pretense of "I'm just trying to avoid merge conflicts".
Simpler, if you know that area of code at all, ask "Does that involve the FooManager class?" - you can probably guess a class nearby.
That makes it easy for them to say "yeah, I probably need to add a bar() method and etc..".
Better yet, guess wrong, and then they can feel some mastery of the problem "No, FooManager is probably fine, I just need to adapt Bar to plug into it".
(P.S. if you have lots of "Manager" classes, like my current codebase, I feel sorry for you.)

Speaking code will result in:
- more devs know more of the codebase
- devs know each other better, work together better
- flaws are found earlier
- less code is written - less wheels reinvented
- new code fits together better
- devs become comfortable talking about code
- devs make less mistakes because they feel more open to ask questions/advice

You probably can't/shouldn't dive into a code discussion during the standup, but you can maybe get a quick question in.
You can definitely lead by example - mention code in your updates.
Instead of "I'm almost done feature X",
simply "I'm splitting ProjectorConfig into ProjectorConfig and ChannelConfig, so it is easier to have multiple projectors per channel".
If you can tie code into user-facing features, both devs and management will be happy.
Also, you have a better chance of someone saying "Yes!" (because they've also wanted that code to happen)
and someone saying "Keep an eye on GeometryConfig" (because they've looked into this before) and saving you time. "Oh, I hadn't thought about that - Let's talk after standup". TADA!


### Catching errors at "Writing time"

ie catch bugs as the code is being written. This is done via guidelines and best practices.  No, not styles guides.  I really don't care where the brackets go.
I do care about having constructors that match assignment.  About avoiding raw pointers. (And "a shared pointer is as good as a global variable" - Sean Parent. So that leaves, typically, unique_ptr.)

With some guidelines and best practices, you can avoid problems before you write them. Or maybe they will copy/paste from a better starting example, etc.
(You can also write some checkers to catch it slightly after it is written.)

### Catching errors at compile time

Everyone's favourite time to catch bugs.

Good APIs are easy to use correctly, and hard to use incorrectly. ie bad uses don't compile. Good classes and functions cause you to "fall into the pit of success".

And "API" means every class and function that you write.

### Catching errors in Tests

Yeah, do that.  Test early - attempting to call your code helps give clarity to your code.  Writing unit tests helps keep your code small and independent. If the code isn't independent, you will find it harder to write tests (because you will need to set up more objects and state to test the code you really want to test).  So if you are diligent about writing tests, you will naturally write more independent code.

And of course tests can catch bugs.


## Code Review

Finally, we get to the topic at hand.

Why was all that other stuff mentioned first?  Because most of that stuff shouldn't happen during a code review - it has, hopefully, already been done.  And the earlier it was in this doc, the more likely it should have been done already.

A dev rightly feels defeated if told to do major rewriting on something they just wrote. So feature and architectural issues were hopefully caught before the code review, and shouldn't be part of the review.

Some guidelines and best practices can be caught - and taught - during code review. (And discovered.  Either by seeing a good technique, or by seeing a mistake and figuring out ways to avoid it in the future.)

API improvements are often found during code reviews.  Everything is obvious when you write it.  You need someone else to read it to really know whether it is clear or not.

So, things to keep in mind when doing a code review:

### 0. Note the good, not just the bad

> Good code goes unnoticed.

It is an inherent property of good code that it isn't noticed.  It just works. It just makes sense.  So you read it and skip right past it.
Try to get in the habit of noticing when that happens, and point it out to the author.

If you notice a file in a code review that has no review comments, maybe it is because the code is good.  Not great.  Just reasonable.  Don't be afraid to leave a positive comment in there.

### Style is mostly a waste of time

I just don't bother commenting on style, unless it really hinders readability.  And not just _your_ readability because it isn't your style.  I only comment when I think it harms _anyone's_ ability to read it.

If you do want to "police" style, do it with automated tools, not by littering a code review with comments on every bracket.  At best, leave a single comment on the whole review (or one per file) about not following the style guide.

### Code authors should respond with code

Almost any comment left by a reviewer, even just "I don't understand what is going on here", or particularly "I don't understand what is going on here", should be answered by updated code, not a review-comment in the review tool, explaining the code.  Take that explanation and paste it into the code, if you need to. Or, more likely, find a way to make the code more understandable.


