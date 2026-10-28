---
name: meta-ads
description: "Launch and manage Meta (Facebook and Instagram) ads from chat: single images, carousels, videos, creative tests, retargeting, lookalike and website-visitor audiences, radius targeting, call and lead ads, plus budgets, pause, resume and in-place edits. Use when the user wants to run, launch, edit, or manage Facebook or Instagram ads, or asks to advertise on Meta."
---

# Meta ads

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll set up your Facebook and Instagram ad from your own creative, created paused, you approve before anything spends."
4. After they connect, continue right where the request left off.

## Connect once

`connect_ad_account` platform meta returns a browser link; on Facebook's dialog the user ticks the ad account and the Page. `ads_lookup` with what "accounts" shows the id (act_...).

## Launching with creatives: `launch_social_ad` platform meta

- cards IN ORDER, each by creativeName or creativeId from `ads_lookup` with what "creatives", or an imageUrl / videoUrl the user owns. 1 image = single-image ad, 2 to 10 = carousel, 1 video = video ad, mixed = creative test (Meta shifts budget to the winner).
- message (primary text), link, ctaType (LEARN_MORE default, SHOP_NOW, SIGN_UP, BOOK_NOW, CALL_NOW with phoneNumber in international format, WHATSAPP_MESSAGE with a wa.me link), pageName when the user has several Pages.
- objective: traffic (default), sales (conversionEvent when it is not Purchase), leads (leadFormId for an instant form built in Ads Manager, or the pixel's Lead event), awareness, engagement. Ask when the goal is unstated.
- Targeting: countries (ISO-2), or cityKeys / regionKeys from `ads_lookup` with what "targeting" (platform meta, kind locations), or radiusAddress plus radiusKm (1 to 80, no lookup). ageMin, ageMax, gender. interests as ids from the targeting lookup (kind interests), confirmed with the user. audienceIds for retargeting; retargetWebsiteVisitors true builds a pixel audience on the fly (retargetRetentionDays, retargetExcludePurchasers).
- Preview, explicit OK, then the same call with confirmToken. Created paused.

## Campaign shell without creative

`launch_campaign` platform meta builds the campaign and targeted ad set with no ad, for a user who will add the creative in Ads Manager.

## Audiences: `add_to_campaign` what "audience", platform meta

- kind website_visitors: a retargeting pool from the pixel (retentionDays, excludePurchasers).
- kind lookalike: seedAudienceId from the targeting lookup (kind audiences), ratioPct 1 to 10, country.

## Editing and managing

- `launch_social_ad` with editAdId changes an existing ad's message or headline in place (ids from `ads_lookup` with what "account_status"). Nothing new is created.
- `manage_campaign` action locations with radiusAddress / radiusKm, setCountries, setCityKeys or setRegionKeys replaces the ad set geography; advantageAudience "off" delivers only to the people it targets. Actions pause, resume, update as on every platform.
- What "account_status" shows ad review status and blockers.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before passing the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.
