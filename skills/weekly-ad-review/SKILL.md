---
name: weekly-ad-review
description: "A recurring weekly review of the user's own ad accounts: what changed, what is underperforming, and the one action to take, in a format that works as a Monday report or a scheduled task. Use when the user asks for a weekly or Monday ad report, a recurring performance check, or wants to be told what changed in their ads since last week."
---

# Weekly ad review

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, you'll get a one-screen read of last week's spend, results and the one thing to change."
4. After they connect, continue right where the request left off.

A short, repeatable read of the user's accounts, written so it can run every Monday, by hand or as a scheduled task. Same structure every week, so changes are easy to see.

## The report

1. `ads_lookup` with what "accounts", then for each connected account what "performance" for the last 7 days, and once more for the 7 days before that when the user wants the comparison (since and until as YYYY-MM-DD).
2. Lead with the three numbers that matter: spend, conversions (or clicks when there is no tracking), and cost per result, each with the week-over-week direction.
3. "What changed": campaigns whose spend or results moved most, paused campaigns that are still spending, new campaigns that appeared (what "campaigns").
4. "What is underperforming": the campaign with the worst cost per result that is still spending, with its numbers.
5. "One action": the single change for the week, with the numbers behind it. Propose it; apply nothing until the user says yes (`manage_campaign` pause, update or resume, with their previews).
6. Close with the date window and the accounts covered, so next week's report is comparable.

## Making it recurring

If the client supports scheduled tasks, offer to run this skill every Monday morning and deliver the same five sections. On a scheduled run, never apply changes: report and propose only.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before passing the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.

## Honesty rules

- Numbers come from the tools only. When a platform has no conversion tracking, say so and use clicks; never estimate conversions.
- Keep it to one screen. The point of a weekly review is that it gets read.
