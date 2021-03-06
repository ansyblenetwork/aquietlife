---
title: "Options Principles III: Understanding options strategies through collateral"
---

I like fundamental concepts (generally - not only in the sense of financial lingo). In particular, one of my pet peeves is when options traders talk about complex options strategies, often to flex a supposedly deep understanding of options.

To my mind, there is only one thing one needs to study to deeply understand options trading: How to collateralize a short contract. There are roughly three mechanisms to understand, and every options strategy is built on one or more of these mechanisms.

My claim is that it is almost always sufficient, and often better, to understand the underlying collateralization mechanisms, rather than the complex strategy itself. In particular, thinking in terms of these mechanisms brings one to think directly in terms of [capital efficiency and diversification]({% post_url 2021-06-29-trading-theses %}), which complex strategies obfuscate.



### The mechanisms

There are three ways to collateralize a short options contract:

1. cash (or stock),
2. a long contract,
3. mutually exclusive cash collateral.

Due to habit and the need to communicate, it is still convenient to refer to these in terms of the corresponding options strategies:

1. cash secured put (or covered call),
2. spreads,
3. iron condors. 

In fact, one can think of (1) as a degenerate version of (2), and (2) a degenerate version of (3). So if one wanted to be reductive, options trading could be described entirely by the phrase "mutually exclusive cash collateral," and all strategies could be viewed as combinations of (possibly degnerate) iron condors.

### Cash and stock collateral (secured puts and calls)

To short a put contract, you need to be prepared to buy the underlying stock. Your broker will freeze the amount of capital required.

To short a call contract, you need to be prepared to sell the underlying stock. Your broker will require you to hold, and freeze, the amount of stock required. (I think of this mechanism as somewhat of an anomaly. Precisely, it does not interact with the other mechanisms of collateralization.)

### Protective long contracts (spreads)

You can reduce the amount of capital required to collateralize a short put by pairing it with a long put for the same underlying. There are two degrees of freedom to consider when deciding which put to buy:

First, there is time. A contract with the same strike but with an equal or later expiration perfectly collateralizes the short contract. Note that in an efficient market, such a pair of contracts will cost you premium. To earn more premium (precisely, to buy less premium), the expiration of your defensive put should be chosen closer to the expiration of your short put.

Second, there is the strike. A contract with the same expiration but a lower strike will collateralize some of your short contract; the difference in strikes must still be held in cash as collateral. To earn more premium, the strike should be chosen farther from the strike of your short put.

These two degrees of freedom may be mixed arbitrarily. Any long put with a later expiration and lower strike, will reduce the amount of cash collateral required for your short put.

Similarly, any long call with a later expiration and higher strike, can be used to collateralize a short call. 

Here is a chart which depicts the properties of a put credit spread depending on the expiration and strike of the protective put:

| (Short put here)     | Few days to expiration |  ... | Many days to expiration |
| ----------- | ----------- | ----------- | ----------- |
| __High strike__   ||| low (negative) premium, no collateral    |
| __...__   | 
| __Low/zero strike (secured put)__   |max premium, max collateral |


### Mutually exclusive cash collateral (iron condors)

Cash collateral held for an out-of-the-money (OTM) call spread can simultaneously be used for an OTM put spread, as long as the short legs of both have the same expiration. The idea is that call and put spreads cannot be in-the-money (ITM) at the same time, so the collateral will only ever be needed to exercise one of the spreads, not both.


### Relation to OCD

Spreads correspond to capital efficiency. One way to think about this is that part of the collateral held for a naked put is not truly at risk, and accordingly (assuming a reasonably efficient options market) not earning any premium. For instance, Google simply is not going to drop below $500 anymore. The $500 held as part of a cash secured put is not really at risk, and also not really earning any premium.

The protective contract in a spread releases that collateral so that you can do something with it - precisely, to put it at risk, and thus earn premium.

Iron condors are evidently another excellent means of capital efficiency, since they essentially use the same capital twice. Beyond this, they are also a form of perfect diversification: Half of an iron condor tends to increase in value exactly when the other half tends to decrease. They are, in my opinion, severely underused, perhaps because of fees, but also perhaps because of the complex way they are typically presented.

The right way to think about these strategies when trading is in terms of their relation to [OCD]({% post_url 2021-06-29-trading-theses %}):

1. Open cash secured puts _opportunistically_.
2. Spread your puts for _capital efficiency_.
3. Open a condor to _diversify_.
