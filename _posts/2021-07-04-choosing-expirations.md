---
title: "How to choose expirations for your options contracts"
---

There is a lot of bad advice on the internet for how to choose an expiration for your options contracts.

By "a lot," I mean a lot of people recommending one strategy: Shorting 45 days-to-expiration contracts, or what I'll call the "45 DTE strategy." In this post I would like to enumerate why this is a bad idea, and to develop a thesis for how one should actually think about the question of expiration length.



### Thesis-less, empirical trading

A thesis-less empirical trading strategy cannot work without thorough backtesting. This goes without saying.

But a thesis-less trading strategy also cannot be sustainable, because it is vulnerable to crowding. As soon as it becomes public, the strategy will begin easing toward market efficiency. As soon as it becomes popular, it will begin to lose money.

The 45 DTE strategy is empirical and popular, thanks to Tasty Works. Does it have a thesis?

### Why the 45 DTE thesis is lacking

There are some arguments for the 45 DTE strategy thrown around on Reddit. The main arguments are nicely summarized by this [reddit comment](https://www.reddit.com/r/thetagang/comments/h9ym51/why_45_dte_why_do_you_take_profits_why_do_you/):

> 45 DTE is the sweet spot for theta decay, too close to expiration and you have gamma risk, too far out expirations have very little theta decay. 30-60 days is where theta decay starts to pick up dramatically.

The second point is easy to set aside. 30-60 days is when theta decay starts to pick up dramatically; but 0-1 days is when theta decay is maximal. So the 45 DTE strategy stands essentially on the first point about gamma risk.

Options greeks are a huge pet peeve for me, but that discussion is for another post. What is gamma risk? Essentially, it is just an unnecessary terminology to describe the risk associated with holding stock: Stocks can drop. In the case of options trading, you don't literally hold the stock, but closer to expiration, you adopt a larger portion of that risk.

But this is a tautology and a non-thesis. Avoiding gamma risk captures none of the [OCD principles]({% post_url 2021-06-29-trading-theses %}):

1. It has nothing to do with opportunistic inefficiencies. 
2. It runs _counter_ to capital efficiency: If you trust your strategy, then you should be willing to trade it without holding back.
3. It has nothing to do with diversification.

The most charitable thing I can say about the 45 DTE thesis is that it is a means of risk management, and precisely, disaster prevention. But there is a much easier way to achieve this: Just put something like 40% of your portfolio in cash. Easier and more reliable.

### The case for trading 0-14 DTE

With respect to opportunism, the goal here is to put as much of a differential between the useful knowledge you have, and the useful knowledge on the other side of the trade.

Expirations far in the future are unlikely to be useful here because it is difficult to find information that 1) meaningfully affects a stock's price later but not earlier, and 2) is either not well known to, or disproportionally influential on, novice traders. 

In contrast, news that is current and immediate has a much larger chance of inspiring exuberance or fear. Further, novice traders or speculators looking for immediate gratification are more likely to trade options near expiration.

With respect to capital efficiency, 0-14 DTE contracts have the greatest theta decay. Equivalently, they freeze your capital for the least amount of time per unit of premium.

### On diversification

I noted in my post about OCD that one way of diversification is by trading options at different expirations. This of course would include 45 DTE contracts. I am fine with this justification - but this is not what the 45 DTE proponents are saying.
