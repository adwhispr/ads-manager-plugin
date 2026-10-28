---
name: reddit-ads
description: "Launch and manage Reddit ads from chat: promoted posts targeted by community, interest and location, with budgets, pause, resume and performance. Use when the user wants to run Reddit ads, advertise in subreddits, or manage Reddit campaigns."
---

# Reddit ads

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll set up your promoted post in the right subreddits, created paused, you approve before anything spends."
4. After they connect, continue right where the request left off.

## Connect once

`connect_ad_account` platform reddit returns a browser link. Reddit requires a pixel on every ad group: with none the launch stops and names the step in Events Manager.

## Launching: `launch_campaign` platform reddit

- The ad is a promoted post: postText (first line = headline, up to 300 characters), imageUrl (https or a Creative Library image by name) or a text post, landingUrl (image ads only). posts for a creative test. Ad copy cannot be edited after creation.
- objective: traffic, sales, leads, awareness (no engagement). dailyBudgetUsd lives on the ad group.
- Communities: `ads_lookup` with what "targeting" (platform reddit, kind interests) returns subreddits as r/name and interest categories; put them in interests. countries or locationIds from the lookup (kind locations). ageMin / ageMax are not applied on Reddit; say so.
- Preview, explicit OK, then the same call with confirmToken. Created paused.

## Managing

`ads_lookup` with what "performance". `manage_campaign` actions pause, resume (wakes the ad group and ads), update (with several ad groups in a campaign, the tool asks which; an ad group id works as campaignId).

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before sending the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.
