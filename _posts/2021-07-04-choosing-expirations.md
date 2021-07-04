---
title: "Choosing expirations for your options trades"
---

There is a lot of bad advice on the internet for how to choose an expiration for your options contracts.

By "a lot," I mean a lot of people recommending one strategy: Shorting 45 days-to-expiration contracts, or what I'll call the "45 DTE strategy." In this post I would like to enumerate why this is a bad idea, and to develop a thesis for how one should actually think about choosing expiration times.



### Thesis-less, empirical trading

A thesis-less empirical trading strategy cannot work without thorough backtesting. This goes without saying.

But a thesis-less trading strategy also cannot be sustainable, because it is vulnerable to crowding. As soon as it becomes public, the strategy will begin easing toward market efficiency. As soon as it becomes popular, it will begin to lose money.

The 45 DTE strategy is empirical and popular, thanks to Tasty Trade. Does it have a thesis?

### Why the 45 DTE thesis is lacking

There are some arguments for the 45 DTE strategy thrown around on Reddit. The main arguments are nicely summarized by this [reddit comment](https://www.reddit.com/r/thetagang/comments/h9ym51/why_45_dte_why_do_you_take_profits_why_do_you/):

> 45 DTE is the sweet spot for theta decay, too close to expiration and you have gamma risk, too far out expirations have very little theta decay. 30-60 days is where theta decay starts to pick up dramatically.

The second point is easy to set aside. 30-60 days is when theta decay starts to pick up dramatically; but 0-1 days is when theta decay is maximal. So the 45 DTE strategy stands essentially on the first point about gamma risk.

Options greeks are a huge pet peeve for me, but that discussion is for another post. What is gamma risk? Essentially, it is just an unnecessary terminology to describe the risk associated with holding stock: Stocks can drop. In the case of options trading, you don't literally hold the stock, but closer to expiration, you adopt a larger portion of that risk.

But this is a tautology and a non-thesis. First of all, avoiding gamma risk captures none of the [OCD principles]({% post_url 2021-06-29-trading-theses %}):

1. It has nothing to do with opportunistic inefficiencies. 
2. It runs _counter_ to capital efficiency: If you trust your strategy, then you should be willing to trade it without holding back.
3. It has nothing to do with diversification.

Second, the thesis is not explanatory. Risk alone doesn't explain why a particular strategy should have better or worse returns (assuming the strategy does not make you go broke). More on this next.

### The case for trading 0-14 DTE

The goal of opportunism is to put as much of a differential between the useful knowledge you have, and the useful knowledge on the other side of the trade.

Expirations far in the future are difficult to work with here because it is rare to find information that 1) meaningfully affects a stock's price later but not earlier, and 2) is either not well known to, or disproportionally influential on, novice traders. 

In contrast, news that is current and immediate has a much larger chance of inspiring exuberance or fear. Further, novice traders or speculators looking for immediate gratification are more likely to trade options near expiration.

The data from Tasty Trade, properly interpreted, actually supports this case for trading shorter expirations - assuming that you have time and energy to do a little homework. The reason that Tasty Trade's backtesting failed to perform well with shorter expirations is that the test was agnostic to external information. This is evidence that the other side of the trade - an options buyer - is able to leverage news and analysis to take advantage of an agnostic seller. Interpreted another way, this is evidence that the near-expiration segment of the options market is not efficient: An informed investor can outperform!

Tying back to the previous discussion, it is not that gamma risk reduces expected returns, or equivalently, that holding stock is a losing strategy. Rather, Tasty Trade has discovered that informed stock trading on short time scales actually works.


### On capital efficiency and diversification

With respect to capital efficiency, 0-14 DTE contracts have the greatest theta decay. Equivalently, they freeze your capital for the least amount of time per unit of premium.

That being said, I noted in my post about OCD that one way to diversify is to trade options at various expirations. This of course would include 45 DTE contracts. I am fine with this justification - but this is not what the 45 DTE proponents are saying.
