---
name: tiktok-ads
description: "Launch and manage TikTok ads from chat: video ads from the user's own videos, creative tests, interest and location targeting, pixel-based sales campaigns, budgets and performance. Use when the user wants to run TikTok ads, launch a TikTok campaign, or manage TikTok campaigns."
---

# TikTok ads

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll set up your TikTok campaign from your own video, created paused, you approve before anything spends."
4. After they connect, continue right where the request left off.

## Connect once

`connect_ad_account` platform tiktok returns a browser link to TikTok's consent screen. `ads_lookup` with what "accounts" shows the advertiser id.

## Launching: `launch_social_ad` platform tiktok

- cards: 1 video by creativeName or creativeId from `ads_lookup` with what "creatives" (pass tiktokAccountId to also see the videos already in the TikTok asset library), or a videoUrl the user owns; 2 to 5 videos = creative test. Vertical video, 5 seconds or longer.
- message is the caption: 100 characters or fewer, no emoji. link, ctaType (LEARN_MORE default, SHOP_NOW, SIGN_UP, DOWNLOAD_NOW).
- dailyBudgetUsd: TikTok's minimum is 20 in the account currency.
- objective: traffic (default), sales (pixelId from `ads_lookup` with what "targeting", platform tiktok, kind pixels; optimizationEvent usually auto-picked), awareness, video_views.
- Targeting: locationIds from the targeting lookup (platform tiktok, kind locations), ageMin, ageMax, gender, interests as ids from the lookup (kind interests), audienceIds.
- Preview, explicit OK, then the same call with confirmToken. Created paused.

## Managing

`ads_lookup` with what "performance" (platform-native numbers). `manage_campaign` actions pause, resume, update as on every platform.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before passing the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.
