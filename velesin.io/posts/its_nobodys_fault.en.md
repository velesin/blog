# Whose fault it is?

When a project fails we often hear a question: "whose fault it is?".

I've heard it so many times. And every time I wonder how come people can't see how ridiculous this question is.

## Is it HIS fault?

Does anybody _seriously_ think that a failure of a 3 month, 10 people project can be a fault of a single person?

Let's think for a moment how he could cause such a failure:

Maybe he was a total slacker? [Personally, I don't believe it](LINK TO MY 4 TYPES OF PROGRAMMERS ARTICLE) but let's make such an assumption. How unproductive he could be? Assuming his productivity to be 0 is rather not realistic - it would mean that he did _absolutely nothing_ for the whole 3 months and nobody in his team noticed.

Let's make a more reasonable assumption. Maybe his productivity was 50%. That's still very bad (and still quite strange that nobody noticed for 3 months, but let's assume a little suspension of disbelief for a moment). However, in the case of a 10 person project it means only a 5% longer completion time. Not such big impact to make the project fail, right?

So maybe he wasn't lazy, but sloppy, and made an error that put the project at risk?

Is it possible for a single error to undermine the whole project? Unless you are in a space shuttle industry, I doubt it. And even if you are, then I bet you have a rigorous, multi-step QA process in place, so an error has to slip through _all_ of the steps - what's rather a failure of your QA _process_ than any single person.

OK, so maybe it wasn't a single error. Maybe he produced poor quality, defective work all the time, slowing down the project to a crawl?

And everybody has seen it and didn't react for 3 months, quietly letting the project fail?

Really, the only explanation I can think of how a single person could undermine a big, team project is that he was intentionally - and _very cleverly_ - sabotaging this project. But this isn't the most plausible theory, is it?

## Is it THEIR fault?

A project is a team endeavour. Business experts help developers understand requirements. Developers create code and review code of other developers. QA specialists help them all verify that what they build works correctly. A project can fail only when a chain of several failures of several people accumulates.

An "obvious" approach then is to assume it's the fault of all these people. Let's blame the whole team and press them to try harder next time. Problem "solved".

Let's consider why should this work? Why didn't they already tried as hard as they could _this_ time?

One reason may be that they had a "bad day". But as we already discussed, a single person's bad day doesn't have much impact on the project. And it's rather difficult to believe that all 10 people in a team had an epidemic of bad days throughout the whole 3 months.

So maybe the whole team was slacking?

It could indeed have a serious impact on a project. But considering [how uncommon the slackers are](LINK TO MY 4 TYPES OF PROGRAMMERS ARTICLE) this is very improbable. And even if you managed in some amazing way to build a 10 people team, consisting mostly of slackers (let's turn on our suspension of disbelief again), then you have a _really serious organizational problem_, that's definitely not a fault of this team.

## So WHAT failed?

Finally, we start to look in the right direction.

Project failures _always_ have roots in the organizational, systemic problems. They stem from poor communication, wrong methodologies, too much pressure, neglecting quality - or most often a combination of several such factors. If you want to prevent similar failures in the future, pointing a finger at people won't cut it.

If it seems to you that it's somebody's personal fault, that is a sure sign that you're focusing only at the symptoms, not the causes.

You need to look deeper.

A great way to do it is to conduct a Root Cause Analysis. If you've never done it, I recommend you to try the "5 Whys" method. It's simple and easy to grasp, but effective. I won't go into the details here, but in a nutshell you repeatedly ask "why" until you get to the very bottom of a problem (by anecdotal evidence it's typically five times, hence the name).

It's not important if you choose the "5 Whys", other style of Root Cause Analysis or some completely different approach to search for the true cause of a problem. What's the key is to focus on the facts and avoid judging or labeling people.

Take a look at an example of "5 Whys" done wrong:

```
The problem: A team deployed a buggy version of an app.

Why? Because one key feature was not integrated into the main code base.

Why? Because Tom forgot to do it.

Why? Because he was careless.

Conclusion: Tom should try harder next time.
```

Do you think such an analysis will help to prevent the problem from occurring again?

Maybe Tom will be more careful from now on. But what if he is very tired or has a really bad day? An what about other team members? Tom is not the only one responsible for integrating features. Maybe they participated in the analysis and took Tom's example to heart or maybe not. And what about the new hires, who don't even know that such a situation took place?

Compare it with the analysis done right:

```
[...]

Why? Because Tom forgot to do it.

Why? Because he had to do it "by hand".

Why? Because there is no way to list all features waiting for integration.

Why? Because there's no such filter in our project management tool.

Conclusion: Let's add a new custom filter. Even more - let's connect it to our Continuous Integration server, so it merges appropriate features to our main code base automatically.
```

Do you see the difference? Now the problem is solved for good. We've made it _impossible_ to miss a feature - and it doesn't depend on the unreliable human factors.

Search for objective, systemic causes not for the people to blame. When you change your mindset this way, you'll find much better solutions to your problems, create much healthier culture and keep the team motivation high.

---

What's your opinion on this topic? Please share below in the comments!
