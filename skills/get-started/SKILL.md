---
name: get-started
description: "Introduce AdWhispr Ads Manager and get a new user to their first result in two minutes: their own ad account read, the leak found, the one change proposed. Use when the user just installed this plugin, asks what it is or can do, says get started, set up, or help, or when no ad account is connected yet."
---

# Get started with AdWhispr Ads Manager

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll read your real campaigns and tell you the one change that matters this week."
4. After they connect, continue right where the request left off.

AdWhispr Ads Manager lets Claude launch, manage and optimize the user's own ad accounts on Meta (Facebook and Instagram), Google Ads, TikTok, X, ChatGPT Ads, Reddit and LinkedIn: real spend, clicks and conversions, wasted-spend diagnosis, pause and resume, budget and targeting changes with a preview first, and complete campaigns created paused from the user's own creatives.

## The two-minute first run

1. Make sure the connection is live (see the first-run section above).
2. `ads_lookup` with what "brand". If nothing is saved, ask for the user's website or one line about what they sell and `save_my_brand` the moment they answer. Never re-ask once saved.
3. `ads_lookup` with what "accounts". Nothing connected: `connect_ad_account` for the platform they run returns a browser link (free; never ask for keys or sign-in details in chat). Connected: `ads_lookup` with what "performance" on that account and deliver the first win as a plain-language read of what is spending, what is converting, and the single change that matters most this week.
4. Offer exactly one next step, matched to what they just saw: pause the leak, move the budget, or launch something new with the platform skill. Never a menu.

## Three things to try (when the user asks what this can do)

- "What's wasting money in my ad account?": a real spend review with the fix ranked first.
- "Launch a Google Search campaign for my store at 20 a day": created paused, preview first, nothing spends without an explicit resume.
- "Pause everything that has not converted this month": the list, then one approval per campaign.

## Account facts (quote accurately, never oversell)

- Ad accounts connect once via `connect_ad_account`, on the platform's own consent screen. AdWhispr never sees the sign-in.
- Every campaign is created paused with a preview-and-confirm step.
- Connecting changes nothing in the ad account until the user asks for a change and approves its preview.
