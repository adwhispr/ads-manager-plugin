---
name: budget-reallocation
description: "Move ad budget from losers to winners with evidence and explicit approval. Use when the user asks where to shift budget, wants to scale what works, cut what does not, or change a campaign's daily budget."
---

# Budget reallocation

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll show which campaigns earn their budget and propose the exact move, you approve before anything changes."
4. After they connect, continue right where the request left off.

## The move

1. `ads_lookup` with what "campaigns" then what "performance" for the window the user cares about (default the last 30 days). Build one table: campaign, daily budget, spend, results, cost per result.
2. Winners: the lowest cost per result with enough volume to trust. Losers: the highest cost per result still spending, or spend with nothing to show. Say which is which and why in one line each.
3. Propose the move as numbers: "cut X from 40 to 20 a day, raise Y from 20 to 40". Keep the total the same unless the user asked to spend more or less. Raise a winner by no more than about half at a time; platforms relearn after big jumps.
4. On the user's explicit yes, `manage_campaign` action update with dailyBudgetUsd, one campaign at a time, each with its own preview and confirmToken. A pause (`manage_campaign` action pause) is the right call for a loser with nothing to show.
5. Tell the user when to look again (a week is usually the minimum) and offer that as the one next step.

Reddit: the daily budget lives on the ad group; with several ad groups in a campaign the tool asks which, and it accepts an ad group id as campaignId.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before sending the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.

## Honesty rules

- Every finding traces to a tool result. No benchmarks from memory.
- Never change a budget or resume anything the user did not explicitly approve.
- One concrete recommendation beats a list of ten.
