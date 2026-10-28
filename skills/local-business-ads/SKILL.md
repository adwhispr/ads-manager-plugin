---
name: local-business-ads
description: "Ads for a local business from chat: Google Search campaigns with call buttons for people searching nearby, and Meta ads targeted within a radius of the shop, created paused with a preview. Use when the user runs a local service or store (salon, clinic, dentist, restaurant, contractor, gym, agency office) and wants customers from their area."
---

# Local business ads

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll set up a Search campaign with a call button for people searching for you nearby, created paused, you approve before anything spends."
4. After they connect, continue right where the request left off.

Local businesses win on intent and distance: people searching for the service right now, and people within driving range. Two campaigns cover both, and both are created paused.

## Step one: the business

`ads_lookup` with what "brand"; if nothing is saved, ask for their website or one line about what they do and where, and `save_my_brand` the moment they answer. Ask two things only: the service area (a city, or an address plus a radius) and whether they want calls, bookings or walk-ins.

## Google Search for people searching now

- `launch_google_campaign` kind search: keywords are the service plus the place, 3 to 15 headlines of 30 characters or fewer naming the service and the area, 2 to 4 descriptions, finalUrl, dailyBudgetUsd. Set defaultCpc above the default 1.00.
- CALL ADS: callPhoneNumber plus callCountryCode so the ad shows a call button. For most local services calls are the conversion.
- locationIds from `ads_lookup` with what "targeting" (platform google, kind locations) for the city or region; never invent ids.
- After a week, `ads_lookup` with what "performance" and the campaignId shows keyword-level diagnostics; a campaign barely spending usually has bids below the first-page estimate.

## Meta within a radius

- `launch_social_ad` platform meta with radiusAddress (the shop's street address or place) and radiusKm (1 to 80, default 15). No lookup needed. cards from the Creative Library (`ads_lookup` with what "creatives") or an image the user owns: a photo of the place, the team or the work beats stock every time.
- objective traffic by default; leads with a leadFormId when they want enquiries inside Facebook; ctaType CALL_NOW with phoneNumber when they want calls.
- Changing the area later: `manage_campaign` action locations with a new radiusAddress and radiusKm edits the live ad set in place.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before passing the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.

## Honesty rules

- Say plainly that a brand-new Search campaign needs a week of data before anyone can judge it.
- Report only numbers the tools return.
