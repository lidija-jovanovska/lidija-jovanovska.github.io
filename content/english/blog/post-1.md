---
title: "Mastering Value Investing with Python: Implementing Benjamin Graham's Strategy"
meta_title: "Value Investing with Python: Implementing Benjamin Graham's Defensive Strategy"
description: "Discover how to implement Benjamin Graham's defensive investment strategy using Python and financial data. Learn the key criteria for value investing and how to screen stocks programmatically."
date: 2025-02-27
image: "/images/blog_post_banner.jpg"
categories: [ "Investing", "Python", "Financial Analysis", "Value Investing" ]
author: "Lidija Jovanovska"
tags: [ "python", "financial-modeling-prep", "value-investing", "stock-screening", "benjamin-graham" ]
draft: False
keywords: [ "value investing", "Benjamin Graham", "Python investing", "stock screening", "financial analysis", "defensive investment strategy", "investment criteria", "financial modeling prep", "S&P 500", "investment strategy" ]
---


### Introduction

Have you ever wondered why some investors consistently outperform the market while others struggle? The answer might lie in following time-tested principles rather than chasing the latest investment trends. What if you could use the same investment strategy that influenced Warren Buffett and countless other successful investors? In this post, I'll show you exactly how to implement Benjamin Graham's defensive investment approach using Python and financial data.

Two years ago, I knew almost nothing about investing. Coming from Macedonia, a post-socialist state perpetually transitioning to capitalism, I had the notion that investing was reserved for financial professionals or the exceptionally wealthy. My journey from complete novice to implementing Graham's strategy programmatically is what I want to share with you today.

For me, everything clicked once I started earning as a data scientist. I became increasingly curious about how to allocate 
my finances and maximize my financial situation. Rather than rely on generic advice, I decided to educate myself independently.
I searched for the book that virtually all great investors reference — like tracking down the root of a tree by following its branches. 
If Warren Buffett, Charlie Munger, and Peter Lynch represent the branches of modern investing wisdom, Benjamin Graham is 
undoubtedly the root. His investing approach is comprehensively captured in his seminal work: [The Intelligent Investor](https://en.wikipedia.org/wiki/The_Intelligent_Investor).
It took me considerable time to finish this book, and I'll admit that initially I understood very little. Gradually, 
it opened up an entirely new world, encouraging me to explore increasingly broader concepts. You don't simply "read" 
this book once and move on—it functions more like a guide that reveals new insights each time you return to it. 
It has helped me understand the foundations of investing and shown me which paths I might follow in my own financial journey.



### Understanding Benjamin Graham's Philosophy

Benjamin Graham, often referred to as the "Father of Value Investing," was a renowned economist, investor, and author
whose principles continue to shape modern finance. His investing approach emphasized the importance of intrinsic value,
advocating for purchasing securities at prices significantly below their intrinsic worth to minimize risk and maximize
potential returns. Graham's disciplined, research-driven methodology focused on a company's financial health, earnings
stability, and margin of safety. 
What's remarkable is how relevant his principles remain today, even in our high-speed digital markets dominated by algorithms and day traders.

### Defensive vs. Enterprising Investing

Graham outlined two distinct investment strategies in The Intelligent Investor: one for the defensive investor
and another for the enterprising (or aggressive) investor. These strategies reflect different levels of risk tolerance,
time commitment, and skill. Here's his take on the differences between these two strategies:

> It has been an old and sound principle that those who cannot afford to take risks should be content with a relatively
> low return on their invested funds. From this there has developed the general notion that the rate of return which the
> investor should aim for is more or less proportionate to the degree of risk he is ready to run. Our view is different.
> The rate of return sought should be dependent, rather, on the amount of intelligent effort the investor is willing and
> able to bring to bear on his task. The minimum return goes to our passive investor, who wants both safety and freedom
> from concern. The maximum return would be realized by the alert and enterprising investor who exercises maximum
> intelligence and skill.

#### Defensive investor strategy

The defensive investor seeks a passive, low-risk approach focused on capital preservation and steady returns. This
strategy prioritizes simplicity, diversification, and a long-term perspective. Key principles include:

- Stock Selection: Invest in large, financially stable companies with a history of consistent dividends and earnings.
  Avoid speculative or high-risk stocks.
- Diversification: Hold a well-diversified portfolio of 10–30 stocks to reduce risk.
- Asset Allocation: Maintain a balanced allocation between stocks and bonds, typically 50/50, but adjust slightly based
  on market conditions or individual circumstances.
- Minimal Activity: Avoid frequent trading, market timing, or speculative decisions. Instead, focus on a buy-and-hold
  approach with periodic rebalancing.

#### Enterprising Investor Strategy

The enterprising investor takes a more active, hands-on approach, aiming to outperform the market by identifying
undervalued opportunities. This strategy requires diligence, financial knowledge, and a greater tolerance for risk. Key
principles include:

- Stock Selection: Focus on undervalued, overlooked, or mispriced stocks, often identified through detailed financial
analysis.
- Special Situations: Exploit opportunities in special scenarios, such as bankruptcies, mergers, or spinoffs, where
inefficiencies can lead to significant gains.
- Market Timing: While not speculating, the enterprising investor is more willing to adjust portfolio allocation based on
market conditions.
- Concentrated Investments: Unlike the defensive investor, the enterprising investor may hold fewer stocks to concentrate
on high-potential opportunities.


### Implementing the Strategy with Python

The inspiration for this blog post was the fact that I wanted to screen stocks based on Graham's simple heuristics for identifying
valuable companies potentially trading below their fair value. I knew I could use some of the websites like yahoo finance
for some of the criteria, but not for all of them - implemented exactly as Graham defined them in the Intelligent Investor.

After some research and deliberation, I decided to implement them myself - using my expertise in programming and data,
with the help of financial data obtained via the financialmodelingprep API and pandas and plotly for stats and visualisation. 

Not only will this help us find potential investment opportunities, but building this tool ourselves gives us complete 
control over the criteria and thresholds - something no pre-built screener can offer.

For this example, we will use data from the S&P 500 list of the most valuable companies in the US market. However, the code
would work also for any other companies which have the data needed to compute the indicators via the financialmodelingprep API.

The code used for this walkthrough is available on [Github](https://github.com/lidija-jovanovska/investing).

Financial modeling prep offers different access plans which you can review on their [website](https://site.financialmodelingprep.com/developer/docs/pricing).
In short, you can get an access token and use the **Basic** plan for free. However, you won't be able to compute criteria
3, 4 and 5, as they require historical data for more than the past five years. This analysis was done using the Starter plan.

### The 7 Key Criteria Explained: A case study on the S&P 500

Let's now go through each of the criteria. We will discuss the logic behind each of them, the implementation and the
results when evaluating it on the S&P500 list.

#### Adequate size of the enterprise


The idea here is to exclude small companies which are potentially less likely to withstand market fluctuations. Although there may
be great opportunity in such enterprises, the skill and time investment required is contrary to the defensive investor approach.
Jason Zweig notes interestingly in his commentary in the book that you can nowadays diversify your portfolio, and thereby decrease the risk of owning 
small companies, by buying a well-diversified ETF.

The threshold was set to $2 billion in 2003. Adjusting for inflation by using an [inflation calculator](https://www.nerdwallet.com/calculator/inflation-calculator),
this would amount to $3.42 billion in 2024. We can see that all companies satisfy the filter since these are in fact the S&P 500 companies - the biggest companies in the US.
This means that investing in any of these companies would satisfy the first criteria. 
However, if we want to use the same criteria for other indexes (say, for emerging markets), this criteria would be useful to determine which companies are of adequate size. 


{{<plotly json="/plotly/10_largest_companies.json" height="400px">}}


#### Strong financial condition


What is a good heuristic that would help us discern between companies in a strong financial condition and ones that are not?
Graham uses the [current ratio](https://www.investopedia.com/terms/c/currentratio.asp), which represents the ratio of the company's assets to its liabilities.
It measures the company's liquidity, i.e., whether it can pay its short-term debts using its current assets. He recommends a
current ratio of 2-to-1, so that a company has twice more assets than liabilities, which should sustain them through hard times.
This is a more restrictive criterion and according to some sources it can be better to compare the current ratio of the company
to the industry average. Personally, I find this criterion fascinating because it focuses on what a company actually owns versus what it owes - 
a fundamental principle often overlooked in today's growth-obsessed market. When I first applied this filter, I was 
surprised by how many seemingly "stable" S&P 500 companies didn't make the cut. Applying this filter leaves us with the following 115 companies:


{{<plotly json="/plotly/current_ratio.json" height="400px">}}


#### Earnings stability


Some earnings for the common stock in each of the past ten years. 
Note that it is possible that some companies went public less than 10 years ago, so they are automatically excluded from this list. 
Depending on when you run this code, you may end up with 11 years - where the average EPS per year for the 11th year will be calculated with less than four quarters. 
We are left with 354 companies, so it's a valid test to eliminate chronic losers, but not so restrictive to limit the choices to a very small sample.
Some general advice though, If you like the business, go deeper into the data - the indicators may be misleading. For example, Amazon is not on the list just because it has a negative EPS for one quarter in 2014.


{{<plotly json="/plotly/positive_eps.json" height="400px">}}


#### Dividend record


Uninterrupted dividend payments for at least the past 20 years. 
This ensures that owning the stock provides some tangible return of investment to you,
as a result of having some earnings which is already a good sign of a financially stable company. The span of 20 years is also 
vast enough to cover several economic cycles. Think about it - a company that's managed to pay dividends consistently 
through recessions, market crashes, and technological disruptions demonstrates remarkable business resilience.
Applying this criterion leave us with 251 companies - roughly half of the S&P 500 list.

#### Earnings growth


A minimum increase of at least one third in per-share earnings in the past ten years using three-year averages at the 
beginning and end.

So, this means that we're expecting at least 33% cumulative growth in earnings from 2014 to 2024. This equates to 3%
average annual increase which is pretty conservative and leaves us with 300 companies. For the purposes of a better visual experience, 
the chart filters out the companies EOG Resources and APA Corporation which have a growth of 200,000% over 10 years - an astounding rate.


{{<plotly json="/plotly/eps_growth.json" height="400px">}}

#### Moderate price/earnings ratio


Current price should not be more than 15 times average earnings of the past three years.
This touches on the point of identifying undervalued stocks. Graham thought that a P/E ratio greater than 15 might mean 
that the company is overvalued, i.e., that the market has high growth expectations which, if not met, will hurt the price.
Using the average earnings of the last three years provides a smooth representation of the company's recent earnings.
This criterion is also very industry-specific. For example, in the tech sector, the average P/E ratio is much higher than 
other more conservative sectors, like financial for example. Currently, only 110 companies satisfy this criterion.

{{<plotly json="/plotly/pe_ratio.json" height="400px">}}


#### Moderate price to book ratio


Current price should not be more than 1.5 times the book value last reported.
The book value (assets minus liabilities) represents the net worth of a company. A P/B ratio under 1.5 means that 
the price is not too detached from the actual asset value. Nowadays, very often the value of a company comes from 
intangible assets (i.e., franchises, brand names, trademarks). These intangible assets are not accounted for in the book
value calculation, and as a result many companies have a P/B ratio much higher than what Graham suggested. Feel free to
adjust the threshold based on your risk tolerance and intrinsic knowledge. Currently, only 117 companies satisfy this 
criterion.

{{<plotly json="/plotly/pb_ratio.json" height="400px">}}



{{< notice "info" >}}
I want to be clear that I'm sharing my personal journey and implementation of Graham's strategy, not offering formal investment advice. I'm not a licensed financial advisor. Any investment decisions you make should be based on your own research and possibly consultation with qualified professionals. Remember that all investments carry risk, and what worked in the past isn't guaranteed to work in the future. You're responsible for your own financial choices, and neither I nor this blog can be held liable for any investment outcomes.
{{< /notice >}}


### Conclusion

Finally, we combine all criteria in a single dataframe where we can filter based on any combination of the criteria.
We can see that only three companies satisfy all the criteria laid out by Graham. While this may seem limiting, 
it actually shows the disciplined selectivity that Graham advocated.
However, as mentioned, we can adjust some of the criteria thresholds based on our risk tolerance and the sector in which
we plan to invest. Or, we can only use a subset of the criteria which make sense to us. For example, you may not be 
interested in uninterrupted dividend payments for 20 years since you'd rather see the business reinvest the profits in
its own operations, and so the fourth criteria (link to tag) may not interest you. What I really like about these criteria
is that they are mainly based on the company's financial success instead of relying on market fluctuations. Of course,
the company is a part of the economy, the same way any living being is a part of an ecosystem, so they cannot be entirely
separated. However, as we can see for example by looking at the fifth criterion, if a company has shown earnings growth
 for a long period it is reasonable to expect that it is a safe bet for the defensive investor. 

 {{<plotly json="/plotly/all_criteria.json" height="600px">}}

Ultimately, the goal of this blog post was to show you that with some knowledge in investing and programming you can 
relatively easily create your own stock screener and build your investing strategy based on the perennial wisdom of
Benjamin Graham. However, all of us have different goals and ideas, and this opens up the path for you to get creative
and modify some of the criteria to your own needs. The beauty of Graham's approach isn't just in its historical success — 
it's in how it combines disciplined analysis with psychological protection against market volatility. 
By implementing these criteria programmatically, you gain both efficiency and emotional distance from the daily market noise.

I've shared the complete code on [GitHub](https://github.com/lidija-jovanovska/investing), so you can immediately start screening
 for your own Graham-approved stocks. If you adjust the criteria as I've suggested, I'd love to hear about your results in the comments.

Remember, as Graham himself would say:
> Those who emphasize protection are always especially concerned with the price of the issue at the time of study.
> Their main effort is to assure themselves of a substantial margin of indicated present value above the market price - 
> which margin could absorb unfavorable developments in the future. Generally speaking, therefore it is not necessary for them
> to be enthusiastic over the company's long-run prospects as it is to be reasonably confident that the enterprise will get along.
 
 
What criteria will you prioritize in your own investment strategy?
