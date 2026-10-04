# Cheap proxies for scraping: how to cut your real cost per page without tanking your success rate

Nobody searches for "cheap proxies for scraping" because they enjoy proxy shopping. You're here because your scraper is getting blocked, your proxy bill is climbing faster than your data volume, or both — and you suspect you're overpaying for IPs you don't actually need.

Here's the thing that trips most people up: the number you see on a pricing page is a unit price, not a cost. $0.68 per GB and $3.00 per GB are only meaningfully different once you know how many bytes a page costs you and how often your requests come back with something other than a 200.

This article works through that arithmetic, then looks at where one budget residential network — 9Proxy — actually lands on the cheap end of the market, including its full current plan line-up and the limits you should know before paying.

## Free proxy lists are usually the most expensive setup you can pick

There's an entire industry of GitHub repos that publish free HTTP, SOCKS4 and SOCKS5 proxies. Aggregator tools pull 9,000–12,000 unique endpoints per run from about a dozen community-maintained sources, refreshed hourly or daily. On paper, that's infinite proxies for $0.

In practice, those lists are unvalidated. The publishers say it themselves: high failure rates, no country data, and you're expected to test every endpoint before trusting it. One analysis of free proxy performance puts a realistic conversion rate for a list you downloaded an hour ago somewhere between 0.3 and 0.4 — and calls that optimistic.

Run the numbers on a 50,000-page job at p = 0.35 and you're issuing roughly 143,000 requests. The painful part isn't the volume, it's that a dead proxy usually fails as a timeout, and timeouts hold a worker for seconds. So you're paying in wall-clock time and worker slots.

Then there's the overhead that never shows up in a request counter:

- Re-downloading and re-validating lists, because entries expire while you're mid-crawl
- Building health checks, retry logic and dead-proxy eviction — congratulations, you now maintain a small distributed system
- Re-running batches that silently completed with gaps, the failure mode you only discover in the data three weeks later

There's also a security dimension worth stating plainly, because it's underweighted in most "cheap proxy" listicles. A free open proxy is a machine that forwards your traffic. Some of them monetize that traffic by injecting ads, affiliate rewrites or mining scripts. Some are purpose-built observation posts. Reviewers who have studied these lists land on the same rule: never send anything through an unknown proxy that you'd mind a stranger keeping. No logins, no API keys, no session cookies, and nothing whose integrity you plan to trust later.

That doesn't make free lists useless. For learning how to configure a proxy in `requests` or `curl`, or testing that your own retry logic handles failure, they're fine — arguably ideal, since an unreliable pool is the perfect test fixture. For production scraping, they're a hobby.

## Cost per page beats price per GB every time

Here's the arithmetic that decides whether a proxy provider is genuinely cheap. Cost per page is:

**(price per GB × GB per page) ÷ success rate**

Take a 200 KB HTML response, which is a reasonable average for a product or listing page:

|  | Cheap pool | Mid-market pool |
| --- | --- | --- |
| Price per GB | $0.68 | $3.00 |
| Bandwidth per 1,000 pages | 0.2 GB | 0.2 GB |
| Bandwidth cost per 1,000 pages | ~$0.14 | ~$0.60 |
| Assumed success rate | 40% | 90% |
| Effective cost per 1,000 pages | ~$0.34 | ~$0.67 |

Two things worth noticing. First, at these page sizes bandwidth is almost free either way — a thousand pages costs cents, not dollars. Second, when a request is *blocked*, it usually transfers very little data. Your provider often can't bill you for much on a failure. That's why the cheaper pool still wins the pure-bandwidth comparison even with a bad success rate.

Which means the real cost of a cheap-but-flaky proxy isn't GB. It's engineering time, retries, and the credibility cost of a dataset with holes in it. The right question isn't "what's the lowest per-GB price" — it's "what's the lowest price at which my success rate stays above the threshold my pipeline needs."

## The three billing models, and which scraper each one fits

Budget residential providers mostly sell one of three structures. They're not interchangeable, and picking wrong is the most common way people overpay.

| Model | How you pay | Best fit | The catch |
| --- | --- | --- | --- |
| Rotating residential by GB | Per gigabyte, unlimited endpoints | High-rotation jobs, short requests, minimal data per page | Wasteful if each page is heavy or you hold sessions for minutes |
| Residential by IP with unlimited bandwidth | Per IP, flat | Long sessions, big payloads, bandwidth you can't predict | You're renting IPs, not requests — idle IPs are still spent |
| Bundles (IPs + GB together) | One payment for both | Mixed workloads: some sticky sessions, some rotation | You need to actually use both halves |

If your scraped pages are small and you rotate every request, GB billing is almost always the cheaper structure. If you're pulling full-page renders, downloading files, or holding a single identity for an hour, per-IP with unlimited bandwidth wins — sometimes by a lot, because a bandwidth-heavy job on per-GB pricing is how people end up spending $400 in a weekend.

## Where 9Proxy sits on the cheap end

9Proxy is a residential proxy network — not datacenter, not mobile. Its advertised footprint is 20 million-plus residential IPs across 90+ countries, served by 8,000+ servers, with a claimed 99.95% uptime. It supports HTTP/HTTPS and SOCKS5, and sells two distinct products plus bundles.

The two products behave very differently, so this matters more than the price list:

**Residential by IPs** — you buy a fixed quantity of IPs and get unlimited bandwidth on each. IPs you haven't used don't expire, but an individual IP's natural lifespan runs from a few hours up to roughly 24 hours. Setup for this model goes through the 9Proxy desktop app, which does local port forwarding and optionally proxy authentication.

**Residential by GB** — you buy traffic and generate unlimited endpoints. Rotating or sticky sessions, targeting down to country, state, city, ZIP and ISP, authentication by username/password or IP whitelisting, and everything runs from the dashboard with no app install. Traffic is valid for 180 days, or unlimited on Enterprise plans.

One timing note: on June 1, 2026, 9Proxy raised prices on its IP-based and bundle packages — the first adjustment in its history — while explicitly keeping GB-based pricing unchanged. The tables below reflect the post-adjustment IP and bundle figures.

The "from $0.015 per IP / $0.68 per GB" numbers you'll see in ads are the top volume tiers. Here's the actual ladder.

### Residential proxies by IP (unlimited bandwidth)

| Package | Price per IP | Total | Buy |
| --- | --- | --- | --- |
| 100 IPs | $0.24 | $24 | Start with 100 IPs |
| 500 IPs | $0.144 | $72 | Get the 500 IP package |
| 1,000 + 500 bonus IPs | $0.084 | $126 | Buy 1,500 IPs with the bonus package |
| 2,500 IPs | $0.084 | $210 | Compare the 2,500 IP package |
| 5,000 IPs | $0.072 | $360 | View the 5,000 IP plan |
| 15,000 IPs | $0.048 | $720 | Check the 15,000 IP tier |
| 25,000 IPs | $0.035 | $863 | See 25,000 IP pricing |
| 50,000 IPs | $0.029 | $1,438 | View the 50,000 IP package |

For teams buying at industrial scale, the Business tiers drop the per-IP cost further:

| Business package | Price per IP | Total | Buy |
| --- | --- | --- | --- |
| 100,000 IPs | $0.023 | $2,300 | Get the 100,000 IP package |
| 200,000 IPs | $0.021 | $4,140 | Check 200,000 IP pricing |
| 500,000 IPs | $0.018 | $8,625 | View the 500,000 IP tier |

### Residential proxies by GB

| Package | Price per GB | Total | Validity | Buy |
| --- | --- | --- | --- | --- |
| 5 GB | $3.00 | $15 | 180 days | Start with 5 GB |
| 50 + 5 GB | $2.10 | $105 | 180 days | Get 55 GB of traffic |
| 100 GB | $1.50 | $150 | 180 days | Buy the 100 GB package |
| 200 GB | $1.00 | $200 | 180 days | Compare the 200 GB package |
| 1,000 GB | $0.80 | $800 | 180 days | View the 1,000 GB plan |
| 2,000 GB | $0.75 | $1,500 | 180 days | See 2,000 GB pricing |

A 10,000 GB tier is reported to bring the rate down to $0.68 per GB — that's where the headline number comes from. Enterprise GB packages trade the 180-day clock for unlimited validity plus team features: one owner, up to five members, shared non-expiring bandwidth, per-member traffic controls and activity logs.

### Bundle packages (IPs + traffic)

| Bundle | What's included | Total | Buy |
| --- | --- | --- | --- |
| Starter | 100 IPs + 5 GB | $30 | Get the Starter bundle |
| Popular | 1,500 IPs + 50 GB | $180 | Buy the Popular bundle |
| Pro | 5,000 IPs + 500 GB | $500+ | View the Pro bundle |

## What you're actually giving up at this price

No budget provider is free of compromise, and it's worth naming 9Proxy's before you commit.

The pool is advertised at 20M+ IPs. That's plenty for most scraping workloads, but it's a third of what the enterprise-tier providers advertise, and smaller pools mean each IP gets reused more often, which accumulates reputation faster. Location depth is concentrated in the US and Western Europe — published per-country counts show the largest pools in the US, Canada, France, the UK and Germany, with thinner coverage in emerging markets.

Session model is the bigger practical constraint. On IP-based plans you don't get natural rotation: an IP lives a few hours to about a day, and you need the 9Proxy app plus its port-forwarding setup in the loop. If your stack is headless and containerized, GB-based plans fit more naturally, because they authenticate with username/password or a whitelisted IP and generate endpoints straight from the dashboard.

There are protocol gaps too. Reviewers note no UDP support, which rules it out for anything UDP-based, and no private (dedicated) proxy offering. Datacenter proxies aren't part of the current plan line-up either.

On measured performance, one 2026 budget-provider comparison puts 9Proxy at 95%+ success on lightly protected targets and 85–92% on moderately protected ones, noting that these budget pools hit their ceiling on Tier 3 targets. It also lists the effective residential cost at roughly $1.30–$2 per GB at standard volumes — which is the honest number to plan around if you're buying mid-tier rather than at 1,000 GB.

Treat all of that as the price of entry at this end of the market, not as a flaw specific to one vendor. Every provider selling residential at under $2/GB is making similar trade-offs somewhere.

## Wiring it into a scraper without wasting traffic

The GB-based flow is the one most scrapers want. You generate an endpoint, choose rotating or sticky, and paste credentials into your client:

python
import requests

PROXY = "http://USERNAME:PASSWORD@ENDPOINT:PORT"

session = requests.Session()
session.proxies = {"http": PROXY, "https": PROXY}

r = session.get("https://example.com/product/123", timeout=20)
print(r.status_code, len(r.content))


Two decisions inside that snippet matter more than the code.

**Rotating vs sticky.** Use rotating for broad crawls where every request can come from a fresh IP — the default for price monitoring, SERP checks and category-level sweeps. Switch to sticky when a target requires continuity: login flows, multi-step forms, carts, or anything that fingerprints you mid-session. Sticky sessions hold one IP for a configurable window, which is exactly what you want and exactly what burns traffic if you leave it on by default.

**Where you spend your bytes.** Images, fonts, video and analytics calls are usually 80%+ of a page's weight and 0% of the data you need. Blocking resource types in Playwright, or using `--compressed` and `Accept-Encoding` headers in lighter clients, is the single fastest way to make a cheap proxy plan go further.

If your workload is bandwidth-heavy rather than rotation-heavy — full-page renders, large file fetches, long authenticated sessions — the per-IP model with unlimited bandwidth is the structurally correct choice, and the 100-IP tier at $24 is a legitimate way to test it.

## Six ways to lower the bill further

1. **Reuse what's already on your account.** 9Proxy maintains a "Today List" of proxies that came back online within 24 hours, reusable at no extra charge — reviewers consistently flag this as unusual, and it's real money during testing phases where sessions die mid-run.
2. **Know the failure window.** Multiple third-party reviews describe a 60-second credit-back policy: if a proxy fails within the first minute of activation, the balance returns to your account. It doesn't cover every failure mode, but it's not standard practice in this industry either.
3. **Don't buy a tier above your burn rate.** GB traffic stays valid for 180 days, so a 200 GB package at $1.00/GB is a better first step than 1,000 GB at $0.80/GB if you're unsure about volume. You're not racing a monthly clock.
4. **Use the referral code to shave the first purchase.** 9Proxy's own affiliate documentation states referral codes give the referred user a 5% discount at checkout.
5. **Fix your retry logic before you buy more IPs.** Retrying a 403 four times with the same identity wastes traffic and gets you flagged harder. Rotate on the block, not on the timer.
6. **Watch for periodic giveaways.** 9Proxy has run share-code campaigns handing out 1 GB of traffic per code, redeemable in the dashboard under Share Code → Use Code. One code per account, limited quantity, so it's luck rather than strategy — but free testing traffic is free testing traffic.

## FAQ

**Is $0.68 per GB the real price?**
Only at the 10,000 GB tier. Entry pricing is $3.00 per GB for 5 GB, dropping to $1.00 per GB at 200 GB. Plan for $1.00–$1.50 per GB unless you're buying four figures of traffic.

**Do I need to install anything?**
Only for IP-based plans, which route through the 9Proxy desktop app with local port forwarding. GB-based plans work entirely from the browser dashboard.

**Can I target a specific city?**
Yes, on GB-based plans — targeting runs down to country, state, city, ZIP and ISP.

**What happens when a proxy dies immediately after I activate it?**
Third-party write-ups describe a 60-second credit-back window, after which you can pull a replacement from the Today List.

**Residential or datacenter for scraping?**
Datacenter if your targets don't run serious bot detection — it's cheaper and faster. Residential the moment you hit blocks, because the IP looks like a home connection in the city you selected. 9Proxy's current line-up is residential only.

**What's the cheapest sane starting point?**
$15 for 5 GB if you rotate a lot and pages are small. $24 for 100 IPs if you need stable identities and heavy transfer.

The honest summary: if your bottleneck is bandwidth, per-IP with unlimited traffic is the cheaper structure. If your bottleneck is IP freshness and rotation, per-GB is. Picking the model that matches your crawl is worth more than chasing a few cents off the per-GB rate — and the provider with the lowest headline price is frequently the one that costs you the most.
