---
name: agency-multi-account
description: "Run several clients' ad accounts from one chat: see every connected account, review each client's performance in turn, and apply approved changes account by account without mixing them up. Use when the user is an agency, freelancer or in-house team managing more than one ad account, asks for a client-by-client review, or wants the same change across accounts."
---

# Agency: several clients, one chat

## First run: the user probably is NOT connected yet

If the adwhispr-ads-manager server shows as needing authentication, or a tool call fails with an authentication error, do NOT apologize and stop. Treat connecting as step one:

1. Connecting is free, takes one click, and there are no API keys to manage.
2. Claude Code: run /mcp, choose "adwhispr-ads-manager", pick Authenticate. Claude.ai or Cowork: open Settings -> Plugins -> AdWhispr Ads Manager and use its sign-in prompt, or the authentication card Claude shows when a tool first needs it.
3. Tell them the payoff: "the second you're connected, I'll show every client account in one list and read whichever one you name first."
4. After they connect, continue right where the request left off.

The job here is discipline: every number and every change is tied to a named account, and nothing crosses between clients.

## The client roster

1. `ads_lookup` with what "accounts" is the roster: platform, account name and id for every client. Refer to accounts by the name the tool returns, and keep the id next to it in your notes for the session.
2. When the user names a client, match it to one row before doing anything. If two rows could match, ask which. Never guess.

## Client-by-client review

For each account the user asks about: `ads_lookup` with what "campaigns", then what "performance" for the window they want. Present one short block per client with spend, results and cost per result, then one line of "what I would change". Finish with a short ranked list across clients: who needs attention first and why, with the numbers.

## Changes across accounts

- Apply changes one account at a time with `manage_campaign`, each with its own preview and its own explicit yes. A yes for one client never carries to another.
- Launches use the platform skills with that client's accountId and their own creatives (`ads_lookup` with what "creatives"). Never reuse one client's creative or copy for another.
- The saved brand (`ads_lookup` with what "brand") is the user's own business or the current client. When switching clients for copy, `save_my_brand` with the client's website first, and say you did.

## Reporting back to clients

A per-client summary from this review is a client-ready report: the numbers, the one change made, the one change proposed.

## Launch and change safety (always applies)

- Every campaign is created PAUSED. Nothing spends until the user resumes it.
- Launch tools and `manage_campaign` resume, update, locations, keywords and add_negatives are TWO-STEP: the first call returns a preview plus a confirmToken; show the preview, get the user's explicit OK, then call again with the same args plus that token. Never invent a token.
- Resume starts real spend. Surface its warning and confirm before passing the token.
- Report only numbers the tools return; never invent CTR, CPC, ROAS or conversion figures. Currency follows the ad account's billing currency; label it when not USD.
- If a tool returns a link instead of running (an account step, an upgrade), relay the link as returned and stop there.

## Honesty rules

- Every number names its account. Never total across clients unless asked.
- Never invent benchmarks; compare a client only to their own previous period.
