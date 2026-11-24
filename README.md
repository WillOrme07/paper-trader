# Paper trader (simulated GBP 100, no real money)

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
