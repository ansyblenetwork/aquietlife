---
title: "Choosing expirations for your options trades"
---

There is a lot of bad advice on the internet for how to choose an expiration for your options contracts.

By "a lot," I mean a lot of people parroting one strategy: Shorting 45 days-to-expiration contracts, or what I'll call the "45 DTE strategy." In this post I would like to enumerate why this is a bad idea, and to develop a thesis for how one should actually think about choosing expiration times.



### Thesis-less, empirical trading

A thesis-less empirical trading strategy cannot work without thorough backtesting. This goes without saying.

But a thesis-less trading strategy also cannot be sustainable, because it is vulnerable to crowding. As soon as it becomes public, the strategy will begin easing toward market efficiency. As soon as it becomes popular, it will begin to lose money.

The 45 DTE strategy is empirical and popular, thanks to Tasty Trade. Does it have a thesis?

### Why the 45 DTE thesis is lacking

There are some arguments for the 45 DTE strategy thrown around on Reddit. They are nicely summarized by this [reddit comment](https://www.reddit.com/r/thetagang/comments/h9ym51/why_45_dte_why_do_you_take_profits_why_do_you/):

> 45 DTE is the sweet spot for theta decay, too close to expiration and you have gamma risk, too far out expirations have very little theta decay. 30-60 days is where theta decay starts to pick up dramatically.

The second point is easy to set aside. 30-60 days is when theta decay starts to pick up dramatically; but 0-1 days is when theta decay is maximal. So the 45 DTE strategy stands essentially on the first point about gamma risk.

Options greeks are a huge pet peeve for me, but that discussion is for another post. What is gamma risk? Essentially, it is just an unnecessary terminology to describe the risk associated with holding stock: Stocks can drop. In the case of options trading, you don't literally hold the stock, but closer to expiration, you adopt a larger portion of that risk.

But this is a tautology and a non-thesis. First of all, avoiding gamma risk captures none of the [OCD principles]({% post_url 2021-06-29-trading-theses %}):

1. It has nothing to do with opportunistic inefficiencies. 
2. It runs _counter_ to capital efficiency: If you trust your strategy, then you should be willing to trade it without holding back.
3. It has nothing to do with diversification.

Second, the thesis is not explanatory. Risk alone doesn't explain why a particular strategy should have better or worse returns. In fact, by conventional wisdom, strategies which adopt higher risk/variance should be rewarded with higher returns. This needs to be reconciled. More on this next.

### The case for trading 0-14 DTE

The goal of opportunism is to put as much of a differential between your useful knowledge, and the useful knowledge on the other side of the trade.

Expirations farther in the future are difficult to work with here because time quickly erodes the usefulness of any knowledge differential, except possibly for an extremely consistent stock about which you are particularly knowledgeable. But even if so, such long term information is unlikely to have a disproportionate influence on novice traders. Such a deep understanding also suggests you should invest rather than trade.

In contrast, news that is current and immediate has a much larger chance of inspiring exuberance or fear. Further, novice traders or speculators looking for immediate gratification are more likely to trade options near expiration.

The data from Tasty Trade, properly interpreted, actually supports this case for trading shorter expirations - assuming that you have time and energy to do a little homework. The reason that Tasty Trade's backtesting fails to perform well with shorter expirations is not that there is anything fundamentally wrong with short expirations or gamma risk. Rather, it is that their test strategy is agnostic to external information. This is evidence that the other side of the trade - options buyers - can leverage news and analysis to take advantage of a news-agnostic seller. But this goes both ways: Since the near-expiration segment of the options market is _not_ efficient, an informed options seller can also outperform!

This makes a lot of sense when we return to the 45 DTE discussion. Expirations farther out are bought by investors with deeper knowledge of the stock, while expirations closer to the present are traded by technical analysts. The 45 DTE expirations should not be viewed as optimal, so much as the closest to market efficient, where no one knows any better than anyone else. They should be traded only if your goal is to avoid using individual judgment. The general case for shorting options still suggests that outperformance is possible, but the effect is almost certainly less pronounced.


### On capital efficiency and diversification

With respect to capital efficiency, the analysis is straightforward: 0-14 DTE contracts have the greatest theta decay. Equivalently, they freeze your capital for the least amount of time per unit of premium.

That being said, I noted in my post on OCD that one way to diversify is to trade options at various expirations. This of course would include 45 DTE contracts. I am fine with this justification - but this is not what the 45 DTE proponents are saying.
