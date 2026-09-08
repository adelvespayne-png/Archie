# Show HN post — ready to paste

> ## ⛔ BLOCKED — you cannot post this yet (checked 2026-09-08)
>
> HN is **temporarily restricting Show HNs** because of an influx of posts
> from brand-new accounts. Their notice: *"You're welcome on HN! Take some
> time to get to know the community, become a good contributor, and then it
> will be fine to post an occasional Show HN."*
>
> This is not a rejection of the project — it is an account-standing gate,
> and it applies to everyone new. Trying to post around it (a plain link
> submission from a 0-karma account, a second account) gets the domain
> flagged and burns the one good shot.
>
> **Unblock condition — all three, then post:**
> 1. The HN account is **2+ weeks old**.
> 2. **50+ karma** from ordinary comments (shown next to your username).
> 3. Comments were on other people's threads, not about Trendkept.
>
> Roughly 3–5 thoughtful comments a day for two weeks. Re-check whether the
> restriction has lifted at https://news.ycombinator.com/showhn.html before
> each attempt. Everything below stays ready to go.

**When:** Tuesday–Thursday, around 14:00–15:00 UTC (peak HN traffic).
**Where:** https://news.ycombinator.com/submit
**Golden rule:** clear your day. The submission is 10% of the work; being
present, humble and technical in the comments is the other 90%.
**Honesty rule:** every claim below was verified against the code and the log
on **2026-09-08** (day 39). Before posting, re-check the two numbers marked ⚠️.

> **IMPORTANT — how HN's form actually works.** The submit page gives you a
> **url** box *or* a **text** box, not both. Typing in one greys out the other.
> So: submit with the **URL**, then immediately post the text below as your own
> **first comment** on the thread. That is standard Show HN practice and is
> what almost every Show HN you've read has done.

---

## Title (A recommended)

A) `Show HN: Trendkept – a dependency-free backtester that refuses to peek at the future`

B) `Show HN: I built a tool to test whether I should change my trading rules. It said no.`

## URL (goes in the url box)

https://github.com/adelvespayne-png/Trendkept

## Text (post this as your OWN first comment, straight after submitting)

I built Trendkept because almost every retail backtest I saw looked too good
to be true — usually through look-ahead bias — and because the standard advice
("write your rules down and follow them") has no tooling that actually
enforces it.

It turns a written trend-following ruleset into code you can backtest and
paper-trade. Things HN might find interesting:

**Every signal is causal.** A value at bar i uses only bars ≤ i. Swing pivots
need confirmation bars before they exist, so the backtest can only act on
information the market had actually revealed. Backtests that peek look
brilliant and trade terribly.

**Zero dependencies, stdlib only, Python 3.9+.** The CSV loader copes with
Yahoo/Stooq/broker exports and scales whole OHLC bars by the adjustment
factor, so a 2:1 split never reads as a 50% "lower low".

**The stop is enforced by the broker, not willpower.** Entries go in as
entry+stop pairs. The autopilot that runs the paper account has no live mode
at all — by construction, not by flag. The manual commands that can touch a
live account are gated behind `--confirm --live --i-understand-live`.

**It's been flying a paper account in public since 13 July.** A GitHub Action
runs the full pass every trading day, commits what it did to a CSV in the
repo, and opens an issue when it acts. ⚠️ 39 trading days in, the account is
down 6.97% — every loss capped in advance except one that gapped through its
stop and cost 1.57× the planned risk. That's in the log too, because a stop is
a trigger for a market order, not a guarantee of a price.

**The part I'd most like feedback on:** I got twitchy during the drawdown and
wanted to change the rules, so I built a lab that compares rule variants over
8 years and 50 instruments with an out-of-sample split — tune on the early
years, validate on years the tuning never saw, and a variant only counts if it
beats the baseline on *both* slices for *both* expectancy and return.

Its first run printed a suspicious result: positive return, profit factor
1.48, and negative expectancy. That combination is impossible, and it was my
bug — I was dividing R-multiples by the *trailed* stop instead of the entry
stop, which corrupts exactly the trades that ran furthest. Fixed it, added a
regression test asserting a profitable run can't report negative average R,
re-ran, and the verdict came back: **nothing beat the baseline on both slices.
Change nothing.**

MIT licensed. The honest caveat is in the README: backtests use idealised
fills and are an optimistic ceiling, not a promise. I'd love feedback on the
causality boundary and on the out-of-sample criterion — particularly whether
requiring both slices is too strict.

---

## Numbers to re-check on the morning ⚠️

The two live numbers are the **day count** and the **drawdown**. Both come
from the newest row of the log:

```
python3 -c "
import csv; rows=list(csv.reader(open('business/paper_log.csv')))[1:]
print('day', rows[-1][1], '| date', rows[-1][0]); print(rows[-1][10][:160])"
```

As of 2026-09-08: day 39, equity 93,027.02, −6.97% since 13 July.

## Comment playbook

- **Answer every technical question fast, with file/line references.**
- **"Trend following doesn't work"** → don't argue returns. The tool's claim
  is narrower: *if* you trade rules, it makes you follow them, and the
  backtest is honest about what those rules did historically.
- **"You're down 7%, why would I use this?"** → agree, and say the honest
  thing: fewer than a dozen closed trades can't establish an edge either way.
  To distinguish a +0.2R edge from zero at 95% confidence you need ~250
  trades. That's years. So the paper account demonstrates execution and
  discipline, not profitability — and it's public precisely so nobody has to
  take my word.
- **Someone finds a bug** → thank them, fix it same-day, reply with the
  commit link. Nothing plays better on HN.
- **Never mention Pro or pricing unless asked.** If asked: "planning a paid
  hosted version later; the core stays MIT."
- **Newsletter link goes in your HN profile, not the post.**

## Do not

- Claim or imply any profit.
- Post performance screenshots.
- Ask anyone to upvote. HN detects it and it kills the post.
- Argue with anyone. Concede good points immediately — it reads far better
  than defending, and the good-faith critics are usually right.
