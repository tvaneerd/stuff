Thoughts on Code Reviews (and more)
========================

0. Do Them

Do Code Reviews. Somehow. Any how.

The worst place to catch bugs is in production.  The best is before the code is written - design time.

Some of this seems to contradict "agile".  It need not.
Taking time to design something doesn't mean you need to design it beginning to end.
But at least get an idea of where you might be headed.
_Quickly iterating_ these stages, doesn't mean _skip_ them.

```
$                                                                                                                                $$$$$$
*----------------*--------------*-----------*----------*---------*---------*--------------------------------------------------------*
Design 1,2     Writing       Compiling     Tests*      Debug     Review*    QA                                                  Production
```

Design 1 - feature design  
Design 2 - code design  
Tests* - could/should be between Design and Writing, and should be Unit and System  
Review* - can happen at multiple stages!

Feature Design

Don't focus on features.  Focus on problems that need to be solved.
I've seen feature requests from users like "please add a short-cut key for Save-As, preferably one that auto-names with an incrementing number".
Obviously what they really want is a version control system.  Or a way to experiment within the app, maybe show multiple scenarios at once, compare them, etc.
What is the real problem behind the request?

Don't be afraid to have a vision.  A vision can be quick, doesn't need to be designed in detail. (But does need enough thought to have an idea of feasibility.)
A vision can be "The real user problem is they are constantly tranfering data between apps - let's make that seamless."
How is it going to be seamless. Well, in the end maybe it won't.  Maybe it it will just be less seams. Cut and paste? Import/export/ Plugins?

An idea of direction helps.  You really don't want detail.  Because detail will be wrong.
But high level direction leaves lots of room for experimentation and agile.



_Pre-code_ reviews

I am amazed how often I hear "You probably should have written this like X instead of like Y". :-(
Once, at Adobe, a developer spent about a month implementing an important and complicated feature.
We decided to do a code review - we hadn't (yet) instituted a process for code reviews, we were still feeling our way around.
The lead architect noted (correctly) that the feature really needed to be threaded,
otherwise the main thread could (in some reasonable situations) stall badly.
(This was many years ago, when threading was still fairly new.)
The conclusion of the code review was, yes, lots of rewrite was necessary, as it needed to be threaded - even at the cost of missing a ship date.
(As you can imagine, adding threads after-the-fact isn't always straightforward, particularly with code mixing processing with UI feedback.)

So what was the lesson to be learned?

Code reviews work?  An issue, which would have caused real problems for users, was found early.  We saved time and money!

Yes, true.

However, bigger lesson - we wasted a month (or a good portion thereof) because no one (or no senior dev) asked the developer, before they started, a simple question
"any idea how you think this is going to work"?

_Pre-code Reviews_ ie code-design reviews, are the best code reviews.

Sure you don't always know how the code is going to go, sometimes you just need to dive in and start coding.
But if so, discuss it with someone as you go.  Pairwise programming is another way to do this.

_The very best programmers I've ever worked with_, those rare devs who didn't need to discuss their code because honestly it was just always naturally done the right way,
were the ones that nonetheless _discussed their code the most_ - before they were done writing it. They questioned their choices. They sought feedback, sounding boards.

For senior devs or whomever the team follows: talk about code, at the class level, function level.  Make it a habit for your team to talk about code.

Not features, not deadlines.  Code.  Developers love code.  That's what makes them developers.  The most introverted dev will still light up discussing code.

When someone says "I'm working on feature X" ask what classes they are working on, or what cpp files they are touching.
Some devs get scared by these questions (particularly when they come from senior devs) - it can sound like prying, watching over their shoulders.
Feel free to imply a pretense of "I'm just trying to avoid merge conflicts".
Simpler, if you know that area of code at all, say "Does that involve the FooManager class" - you can probably guess a class nearby.
That makes it easy for them to say "yeah, I probably need to add a bar() method and etc..".
Better yet, guess wrong, and then they can feel some mastery of the problem "No, FooManager is probably fine, I just need to adapt Bar to plug into it".
(P.S. if you have lots of "Manager" classes, like my current codebase, I feel sorry for you.)

Speaking code will result in:
- more devs know more of the codebase
- devs know each other better, work together better
- flaws are found earlier
- less code is written - less wheels reinvented
- new code fits together better

You probably can't/shouldn't dive into a code discussion during the standup, but you can maybe get a quick question in.
You can definitely lead by example - mention code in your updates.
Instead of "I'm almost done feature X",
simply "I'm splitting ProjectorConfig into ProjectorConfig and ChannelConfig, so it is easier to have multiple projectors per channel".
If you can tie code into user-facing features, both devs and management will be happy.
Also, you have a better chance of someone saying "Yes!" (because they've also wanted that code to happen)
and someone saying "Keep an eye on GeometryConfig" (because they've looked into this before) and saving you time. "Oh, I hadn't thought about that - Let's talk after standup". TADA!


Catching errors at "Writing time"

This is done via guidelines and best practices.  No, not styles guides.  I really don't care where the brackets go.
I do care about having constructors that match assignment.  About avoiding raw pointers. (And "a shared pointer is as good as a global variable" - Sean Parent. So that leaves, typically, unique_ptr.)

With some guidelines and best practices, you can avoid problems before you write them.
(You can also write some checkers to catch it slightly after it is written.)

Catching errors at compile time



