---
name: wasted-spend-audit
description: "Audit the user's connected ad account for wasted spend: what is running, what is quietly burning budget, and the fixes ranked by money at stake. Use when the user asks what is wasting money, wants an account audit, a PPC audit, or asks why their ad spend is not converting."
---

# Wasted-spend audit

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll find what is quietly burning budget in your account and rank the fixes by what they are worth."
4. After they connect, continue right where the request left off.

## The audit

1. `ads_lookup` with what "accounts", then what "campaigns" on the account to audit: every campaign with status, daily budget and id.
2. `ads_lookup` with what "performance" for the last 30 days (since and until as YYYY-MM-DD). Then once more per campaign that is spending, with its campaignId, for the drill-down: Google Search returns keyword-level diagnostics.
3. Rank the waste by money at stake, largest first:
   - Spend with zero conversions (or zero clicks where there is no tracking).
   - Google Search keywords with a low Quality Score or rarely shown because the bid is below the first-page estimate: budget that cannot even enter the auction.
   - Search terms that do not match the offer: `manage_campaign` action add_negatives is the fix.
   - A Meta ad set reaching people outside the market (wrong country, no radius) or a campaign duplicated under two names.
   - Paused campaigns that are still reporting spend (delivery lag), so the user knows the number is real.
4. For each item: the number, the cause in one sentence, and the fix as a tool call you can make: `manage_campaign` pause, update, locations, keywords or add_negatives, each with its preview.
5. Apply nothing until the user picks a fix and approves its preview. Then exactly one next step.

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
