---
title: "Incrementalism in options trading"
---

For the past few weeks I have been working on a [Robinhood API applet]({% post_url 2021-07-23-robinhood-api %}) tailored to my personal approach toward trading options (which more or less has been fully presented in my posts up to now).

The process of refining the applet has gotten me thinking about trading strategies and how to best execute them - but also to ask the existential question: Will a given strategy even work?


### The applet

I realize that most readers (wisely) are not going to enter their Robinhood credentials into the applet of a random stranger on the internet, so let me describe what my applet does:

1. It pulls the entered account's options positions.
2. It calculates the rate at which each position depreciates, assuming no value will remain at expiration.
3. It recommends the positions with the lowest rate of depreciation for closure.
4. It also finds contracts which are not paired into a strategy (a spread or condor), and recommends them for completion.

This management allows the account owner to open a much more diverse set of positions without having to remember and track all of them. This helps remove all sorts of bad trading habits:

1. Diversification reduces the chance of a major negative event.
2. More positions means less emotional attachment to any one position. 
3. Closing positions with low depreciation rate means greater capital efficiency.
4. Every position is marked with its rate of return, so risky trades cannot be entered blindly.
5. All rate computations are agnostic to potential biases, like position size and underlying stock price. 

### Incrementalism

This reminded me of a freakonomics episode I overheard on the radio in grad school, about the British cycling team, and how they won some substantial number of gold medals in recent competitions after many years of not winning.

> Physics and cycling go hand in hand. It’s a sport that lends itself nicely to physics, data collection, measurement, power and speed. And so, we could collect lots of data and analyze performance and we could feed that back to riders. And then we could work with them on small, very small, minor tweaks, minor changes that probably felt relatively insignificant at the time, but over time, would stick.
> 
> Some of it could be the position of the bike, the position of the head. We fight against the wind in cycling all the time. It’s the biggest thing that slows us down. And just literally dropping the head between the shoulders, dropping it down just a centimeter will improve the aerodynamics and for the same power, you’ll go a little bit further. And the more you can think about holding that position and being cognizant of that position whilst you’re riding at your limit, it makes a difference. 
> 
> We’d look at hand-washing, for example, an area where we’d go to the Olympic Games and we’d be in great form and then we’d be terrified of the riders getting ill or catching a bug. So we started to think about, Wow, how are you going to optimize or reduce the chances of us getting an illness within the team in the Olympic Village for example, and then for that to run through the team and create havoc. So we got a surgeon in who showed everybody how to wash their hands properly. We had people who cleaned all the handles, cleaned the lifts buttons, we obviously encouraged people not to shake hands and be very mindful of this and use hand gels all the time.
> 
> Is that going win us a medal? Well, no, it’s not. But is it going to contribute to it? Yeah, potentially.

Algorithmic trading, or "advanced trading" (in the sense of [advanced chess](https://en.wikipedia.org/wiki/Advanced_chess)), is a bit like this. It helps ensure optimal strategy on deterministic factors, akin to preventing blunders in chess. These gains are incremental in the sense that most sane traders probably already approximately achieve them - but exact realizations provides the edge that may be the difference between profitability and loss. 

Algorithmic trading is also just a lot more enjoyable. It helps automate many calculations, which frees a lot of mental capacity which the trader can use to focus exclusively on the decisions that really matter.

### Success?

This brings us to the last comment about winning medals. What decisions do really matter? What wins medals?

I honestly thought, before building my applet, that it would basically guarantee profitable trades on average.

This might not be too far from the truth. It makes transparent how one might maintain profitable trading, or even beat the major indicies. But the possibility that this rosy picture deviates even a little from the truth, given so much effort, requires some careful deliberation. The problem with options trading is that 

1. it is a zero-sum game,
2. losses are large, and
3. losses are rare.

The last of these, a bit counterintuitively, is perhaps the most dangerous feature. It is hard to see whether a trading strategy is destined to be profitable in the long run, because losses are statistical anomalies in the short run. How does one know that one's strategy is worthwhile against opportunity costs - namely, passive investing?

I think the unsatisfying answer is inevitable: One cannot know. But the great thing about thesis-driven, incrementally-automated trading is that it ratchets. A strategy or applet which systematically surfaces relevant, timely, information, and reduces the workload of the trader, cannot be a bad thing.
