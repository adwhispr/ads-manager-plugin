---
name: linkedin-ads
description: "Launch and manage LinkedIn ads from chat: Sponsored Content targeted by job title, seniority, function, industry and company size, with budgets, pause, resume and performance. Use when the user wants to run LinkedIn ads, reach a B2B audience by job title or company, or manage LinkedIn campaigns."
---

# LinkedIn ads

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll set up your LinkedIn campaign targeted by job title and company, created paused, you approve before anything spends."
4. After they connect, continue right where the request left off.

## Connect once

`connect_ad_account` platform linkedin returns a browser link (OAuth on LinkedIn's own consent screen). The ad account must be linked to a company Page the user can post for, with a payment method in Campaign Manager.

## Launching: `launch_campaign` platform linkedin

- The ad: postText (up to 600 characters, lead with the point, about the first 150 show before "see more"), imageUrl (a JPG or PNG the user owns, or a Creative Library image by name from `ads_lookup` with what "creatives"), optional adTitle (the headline under the image). posts for a creative test. Write the copy fresh and show it before launching.
- dailyBudgetUsd required (LinkedIn usually asks for at least 10 in the account currency). objective: traffic, sales, leads (website conversions, not Lead Gen Forms), awareness, engagement. Ask when unstated.
- Targeting is LinkedIn's strength: job titles, seniorities, functions, industries, company sizes, skills and interests go in interests as PLAIN WORDS the way LinkedIn names them, each {"id": "Marketing Manager"}; place names in locationIds, for example "London Area, United Kingdom". The preview names every match LinkedIn made; let the user correct a wrong one before confirming. Titles cannot be combined with seniorities or functions; companies not with industries or sizes. LinkedIn will not run an audience under 300 members. ageMin / ageMax map to LinkedIn's buckets; keywords and audienceIds do not apply.
- Audience Network and Audience Expansion stay off, so the budget goes to the audience the user picked.
- Preview, explicit OK, then the same call with confirmToken. Created paused.

## Managing

`ads_lookup` with what "performance" (platform linkedin), down to the ad. `manage_campaign` actions pause, resume, update. The words of a LinkedIn ad cannot be edited after creation; a change means a new ad. Sales and leads campaigns may land as a draft with one step left in Campaign Manager (attaching the conversion); relay that plainly.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before passing the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.
