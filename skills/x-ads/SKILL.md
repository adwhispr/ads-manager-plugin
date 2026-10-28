---
name: x-ads
description: "Launch and manage X (Twitter) ads from chat: promoted posts, creative tests, keyword, interest and location targeting, conversion campaigns, budgets and performance. Use when the user wants to run ads on X or Twitter, promote a post, or manage X campaigns."
---

# X (Twitter) ads

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll set up your promoted post on X, created paused, you approve before anything spends."
4. After they connect, continue right where the request left off.

## Connect once

`connect_ad_account` platform x returns a browser link. X itself requires a payment method at ads.x.com and X Premium or a verified organization on the advertising handle; relay that when a launch is refused for it.

## Launching: `launch_campaign` platform x

- The ad is a promoted post built for the campaign: postText plus optional imageUrl (an https image the user owns or a Creative Library image by name) and landingUrl. posts for a creative test of 2 or more.
- objective: traffic, sales, leads (optimized to the account's conversion tag), awareness, engagement. dailyBudgetUsd required.
- Targeting: countries or locationIds from `ads_lookup` with what "targeting" (platform x, kind locations), keywords, interests as ids from the lookup (kind interests), audienceIds (kind audiences), ageMin / ageMax (X uses one age bucket), gender.
- Preview, explicit OK, then the same call with confirmToken. Created paused.

## Managing

`ads_lookup` with what "performance" (7-day windows). `manage_campaign` actions pause, resume (wakes the line items too), update.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before passing the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.
