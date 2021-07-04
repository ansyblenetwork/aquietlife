---
title: "Choosing strikes for your options trades"
---

Choosing an appropriate strike is a much harder task than choosing an appropriate expiration date.

One way to see this is that the [OCD principles]({% post_url 2021-06-29-trading-theses %}) begin to conflict. Here, the desire for capital efficiency would suggest one should sell strikes at the current market price of the underlying stock. (The usual terminology here is at-the-money, or ATM.) This is the strike at which premium is maximized per unit of collateral. On the other hand, ATM contracts have the highest liquidity, and accordingly are most likely to be market efficient. There is much less opportunism to be found here.


My soft recommendation is to focus more on opportunism than capital efficiency. After all, in the absence of a promising opportunistic trade, there is nothing on which to be efficient. As a result, one should start with strikes deep out of the money (OTM), with low liquidity and the possibility of high inefficiency.

I have some remarks on quantiative recommendations.

### Rate of return

What quantity should one use to objectively choose a strike? The answer is simple: rate of return. It is the quantity that is easiest to compute and understand, and at the end of all things, it is the only quantity that matters.

However, this still doesn't help you decide which rate of return is best. There isn't really a clean answer. As I noted in the introduction, opportunism should swamp almost any rule of thumb.

There is soft piece of wisdom, however, that I like to keep in mind. It comes from [an interview with Charlie Munger](https://www.youtube.com/watch?v=53vXIbsaBgw), which was about investing, but one phrase sticks.

Here is the relevant anecdote:

> Let me give you an interesting example. The investment world involves an enormous amount of high IQ people trying to be more skillful than normal. You can hardly imagine another activity that gets so much attention. And weird things have happened. Years ago one of our local investment counseling shops, a very big one, they were looking for a way to get an advantage over other investment counseling shops. And they reasoned as follows. We’ve got all these brilliant young people from Wharton and Harvard and so forth and they work so hard trying to understand business and market trends and everything else. And if we just ask each one of our most brilliant men for their single best idea then created a formula with this collection of best ideas, we would outperform averages by a big amount... So they tried it out, and of course it failed utterly.
>
> The interesting thing about this situation is that this is a very intelligent group of people that’s come from all over the world. You’ve got a lot of bright people from China where people tend to average out a little smarter. And the issue is very simple. It’s a simple question. Why did that plausible idea fail? Just think about it for a minute. You’ve all been to fancy educational institutions. I’ll bet you there’s hardly one in the audience who knows why that thing failed. That’s a pretty ridiculous demonstration I’m making. How could you not know that?
>
> Now at a place like Berkshire Hathaway or even the Daily Journal, we’ve done better than average. And now there’s a question, why has that happened? Why has that happened? And the answer is pretty simple. __We tried to do less.__
>
> __We never had the illusion we could just hire a bunch of bright young people and they would know more than anybody about canned soup and aerospace and utilities and so on and so on and so on. We never had that dream.__


### Not using delta

I've [noted before](% post_url 2021-07-04-choosing-expirations %}) that I do not like discussing options Greeks. Here is an example of why.

I just watched a Tasty Trade video discussing the "perfect delta" to trade at. They roughly agreed with my point that the answer is not clear, and their explanations were actually pretty similar to mine, at least in spirit. However, as part of their discussion, they kept saying something to the effect of:

> Our research shows that 0.5 delta contracts are the most capital efficient.

I have a huge gripe with that sentence, and here's why: A 0.5 delta contract is synonymous with an ATM contract. So the conclusion of their research was that an ATM contract is most premium-efficient. I'm sorry, but no shit.

This is just one instance demonstrating the harm of options Greeks: They mostly serve to obfuscate, sometimes to real detriment.

