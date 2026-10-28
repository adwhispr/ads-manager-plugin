---
name: chatgpt-ads
description: "Launch and manage ChatGPT ads (OpenAI Ads) from chat: chat-card ads with context-hint targeting inside ChatGPT, more ad groups and ads in a campaign, budgets and performance. Use when the user wants to advertise on ChatGPT, run OpenAI ads, or asks whether ads inside ChatGPT are possible."
---

# ChatGPT ads

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll set up your chat-card ad inside ChatGPT, created paused, you approve before anything spends."
4. After they connect, continue right where the request left off.

## Connect once

`connect_ad_account` platform chatgpt returns a link to a page where the user pastes an Ads API key created in their own ChatGPT Ads Manager. Never ask for the key in chat.

## Launching: `launch_campaign` platform chatgpt

- The ad is a chat card: adTitle (3 to 50 characters), adBody (up to 100), imageUrl (direct https or a Creative Library image by name, at least 640x640), landingUrl.
- keywords are context hints: the customer intents the card should appear for.
- objective: traffic, sales or leads; every goal runs on clicks on a daily budget. locationIds from `ads_lookup` with what "targeting" (platform chatgpt, kind locations).
- Preview, explicit OK, then the same call with confirmToken. Created paused.

## More ad groups and ads

`add_to_campaign` what "ad_group", platform chatgpt, campaignId, name (one customer intent), keywords as its context hints, ads as 1 to 10 cards {title, body, imageUrl, landingUrl?}; adGroupId to add cards to an existing group. New groups and ads follow their siblings' status.

## Managing

`ads_lookup` with what "performance". `manage_campaign` actions pause, resume (wakes paused ad groups and ads, and notes review blockers), update.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before passing the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.
