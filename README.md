# Paper trader (simulated GBP 100, no real money)

Runs 24/7 on UK + US shares/ETFs. Reads business news around the clock (Claude if ANTHROPIC_API_KEY
is set, otherwise a strict keyword fallback). News that arrives while a market is shut is queued and
acted on at the next open - unless the price has already run away (then it's dropped as "priced in").

Trading 212-style costs modelled:
- UK shares: ~0.1% half-spread each way + 0.5% stamp duty on buys (ETFs exempt)
- US shares: 0.15% FX fee each way (GBP<->USD) + small spread (wider in pre/after-hours)
- Fractional shares, no commission. US-listed ETFs are excluded (UK retail can't buy them).

Market windows (bot trades only when it has a fresh price):
- UK: Mon-Fri ~08:10-16:25 UK time (Trading 212 has no extended hours for UK shares)
- US: Mon-Fri 04:05-19:55 New York time (= ~09:05-00:55 UK). Trading 212 also has an overnight
  session, but Yahoo has no prices for it, so the bot sits that out.

Commands (run inside this folder):  python bot.py once | run | report | reset | demo
Settings: config.json.   Results: data\state.json, data\trades.csv, data\bot.log
`demo` uses made-up prices: a plumbing test only, not evidence the strategy works.

## Running it free in the cloud (GitHub Actions)
The file .github/workflows/paper-trader.yml runs one bot cycle every ~10 minutes on GitHub's servers and
saves the account back into the repo (data/state.json, data/trades.csv, data/report.txt).
Use a PUBLIC repo (private repos run out of free minutes). Add your Claude key as a repo secret named
ANTHROPIC_API_KEY (Settings > Secrets and variables > Actions). Open data/report.txt on GitHub for the
current profit/loss. GitHub pauses scheduled workflows in repos with no activity for ~60 days; if that
happens, re-enable it in the Actions tab. To restart from GBP100, delete the files inside data/ in the repo.
