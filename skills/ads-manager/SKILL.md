---
name: ads-manager
description: "Manage and optimize the user's own ad accounts from chat: read what is spending and converting, find the leak, and make the one change that matters, with a preview before anything applies. Use when the user says manage my ads, optimize my campaigns, take over my ad account, or asks what to change in their Google, Meta, TikTok, X, ChatGPT, Reddit or LinkedIn ads."
---

# Ads manager: manage and optimize the user's own accounts

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll read your real campaigns and tell you the one change that matters this week."
4. After they connect, continue right where the request left off.

This is the generalist workflow for "manage my ads". It reads first, proposes one change, and applies it only after the user says yes.

## The loop

1. `ads_lookup` with what "accounts". Nothing connected: `connect_ad_account` for the platform they run returns a browser link.
2. `ads_lookup` with what "campaigns" for what exists (name, status, budget, id), then what "performance" for real spend, impressions, clicks and conversions. Pass a campaignId to drill into one campaign; on Google Search that adds keyword-level diagnostics (Quality Score, bids below the first-page estimate, keywords rarely shown). What "account_status" shows ad-level status and review blockers on Meta.
3. Diagnose in plain language: what is spending, what is converting, and the single change that matters most this week. Typical leaks: a campaign spending with no conversions, a Search campaign barely spending because bids sit below the first-page estimate, budget concentrated on the wrong goal, a Meta ad set reaching strangers.
4. Propose ONE change with the numbers behind it. Then, only on the user's explicit yes, `manage_campaign`:
   - action pause to stop a leak (single step).
   - action update to move money (dailyBudgetUsd) or rename (newName). Two-step.
   - action locations to fix where it delivers: Google ids to add or remove, or a Meta ad set's radius, cities, regions or countries in place. Two-step.
   - action keywords (Google Search: pause, enable or re-bid a keyword) and add_negatives (exclude the searches that waste money). Two-step.
   - action resume to bring a paused winner back. Two-step, starts real spend.
5. Offer exactly one next step, tied to what you just did. Never a menu.

## When the fix is a new campaign

Hand off to the platform skill: google-ads, meta-ads, tiktok-ads, x-ads, chatgpt-ads, reddit-ads or linkedin-ads. Each builds the campaign paused with a preview first.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before passing the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.

## Honesty rules

- Every finding traces to a tool result. No benchmarks from memory.
- Never change a budget or resume anything the user did not explicitly approve.
- One concrete recommendation beats a list of ten.
