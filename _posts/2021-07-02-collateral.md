---
title: "Collateral: The right way to think about trading options"
---

I like fundamental ideas (generally - not only in the sense of financial lingo). In particular, one of my pet peeves is when options traders talk about complex options strategies, often to flex a supposedly deep understanding of options.

To my mind, there is only one thing one needs to study to deeply understand options trading: How to collateralize a short contract. There are roughly three mechanisms to understand, and every options strategy is built on one or more of these mechanisms. 

My claim is that it is almost always sufficient, and often better, to understand the underlying collateralization mechanisms, rather than the complex strategy itself. In particular, thinking in terms of these mechanisms brings one to think directly in terms of [capital efficiency and diversification]({% post_url 2021-06-29-trading-theses %}), which complex strategies obfuscate.



There are three ways to collateralize a short options contract:

1. cash (or stock),
2. a long contract,
3. mutually exclusive cash collateral.

Because of convention, it is still convenient to refer to these in terms of the corresponding options strategies:

1. cash secured put (or covered call),
2. spreads,
3. iron condors. 

### Cash and stock collateral (secured puts and calls)

To short a put contract, you need to be prepared to buy the underlying stock. Your broker will freeze the amount of capital required.

To short a call contract, you need to be prepared to sell the underlying stock. Your broker will require you to hold, and freeze, the amount of stock required. (I think of this mechanism as somewhat of an anomaly. Precisely, it does not interact with the other mechanisms of collateralization.)

### Protective long contracts (spreads)

You can reduce the amount of capital required to collateralize a short put by pairing it with a long put for the same underlying. There are two degrees of freedom to consider when deciding which put to buy:

First, there is time. A contract with the same strike but with an equal or later expiration perfectly collateralizes the short contract. Note that such a pair of contracts will cost you premium. To earn more premium (precisely, to buy less premium), the expiration of your defensive put should be chosen closer to the expiration of your short put.

Second, there is the strike. A contract with the same expiration but a lower strike will collateralize some of your short contract; the difference in strikes must still be held in cash as collateral. To earn more premium, the strike should be chosen farther from the strike of your short put.

These two degrees of freedom may be mixed arbitrarily. Any long put with a later expiration and lower strike, will reduce the amount of cash collateral required for your short put.

Similarly, any long call with a later expiration and higher strike, can be used to collateralize a short call. 

Here is a chart which depicts the properties of a put credit spread depending on the expiration and strike of the protective put:

| (Short put here)     | Few days to expiration |  ... | Many days to expiration |
| ----------- | ----------- | ----------- | ----------- |
| __High strike__   ||| low (negative) premium, no collateral    |
| __...__   | 
| __Low strike (naked put)__   |max premium, max collateral |


### Mutually exclusive cash collateral (iron condors)

Cash collateral held for an out-of-the-money (OTM) call spread can simultaneously be used for an OTM put spread (with the same expiration). The idea is that call and put spreads cannot be in-the-money (ITM) at the same time, so the collateral will only ever be needed to exercise one of the spreads, not both.


### Relation to OCD

Spreads correspond to capital efficiency. One way to think about this is that part of the collateral held for a naked put is not truly at risk, and accordingly (assuming a reasonably efficient options market) not earning any premium. For instance, Google simply is not going to drop below $500 anymore. The $500 held as part of a naked put is not at risk, and also not earning any premium.

The protective contract in a spread releases that collateral so that you can do something with it - precisely, to put it at risk, and thus earn premium.

Iron condors are a form of perfect diversification, and in my opinion, underused because of it. Half of an iron condor increases in value exactly when the other half decreases.

The right way to think about these strategies when trading is in terms of their relation to [OCD]({% post_url 2021-06-29-trading-theses %}):

1. Look to open cash secured puts Opportunistically,
2. Spread your puts for Capital efficiency,
3. open a condor to Diversify.
