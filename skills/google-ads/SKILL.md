---
name: google-ads
description: "Launch and manage Google Ads from chat: Search campaigns with keywords and call buttons, YouTube video campaigns, Shopping campaigns, ad groups, keyword and negative keyword changes, budgets and performance. Use when the user wants to run Google ads, launch a Search, YouTube or Shopping campaign, advertise on Google, or manage Google campaigns."
---

# Google Ads

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll set up a Search campaign with your keywords and copy, created paused, you approve before anything spends."
4. After they connect, continue right where the request left off.

## Connect once

`connect_ad_account` platform google returns a browser link; the user needs a regular Google Ads account (not only a manager account). `ads_lookup` with what "accounts" then shows the id (digits).

## Launching: `launch_google_campaign`

- kind "search": keywords (one tight theme), 3 to 15 headlines of 30 characters or fewer, 2 to 4 descriptions of 90 or fewer, finalUrl, dailyBudgetUsd, matchType (default PHRASE). Set defaultCpc from what the keywords cost; the default 1.00 is usually too low to win auctions. Add callPhoneNumber plus callCountryCode for a click-to-call button.
- kind "youtube": videos, 1 to 5 by Creative Library name or id (`ads_lookup` with what "creatives") or YouTube links the user owns, at least 5 seconds long; plus headlines and descriptions. Runs as in-stream, in-feed and Shorts.
- kind "shopping": the account's linked Merchant Center; merchantId only when several are linked (the preview lists them). The preview says how many products can show.
- Geography: locationIds from `ads_lookup` with what "targeting" (platform google, kind locations, query the place). Default United States. Never invent ids.
- Write the copy fresh for the user's own product; show headlines and descriptions before launching. Preview, explicit OK, then the same call with confirmToken. Created paused.

## More ad groups

`add_to_campaign` with what "ad_group", platform google, campaignId, name (the theme), keywords as {text, matchType?, bid?}, headlines, descriptions, finalUrl, defaultCpc. One intent per ad group.

## Managing

- `ads_lookup` with what "performance" and a campaignId gives keyword-level diagnostics: Quality Score, bids below the first-page estimate, keywords rarely shown.
- `manage_campaign` action keywords: pause, enable or set_bid per keyword (adGroup when the text sits in several). Action add_negatives: exclude searches that waste money, each {text, matchType?}. Action locations with addLocationIds / removeLocationIds (read the current list first). Actions pause, resume, update as on every platform.
- A brand-new Search campaign needs about a week of data before anyone can judge it; say so.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before sending the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.
