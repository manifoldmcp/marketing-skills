---
name: clean-email-list
description: When the user wants an email list they already have verified and cleaned before a send. Normalises and dedupes the addresses, flags role, webmail and disposable ones, verifies every address that is left, drops the ones that will bounce, flags catch-all and unknown ones, and hands back the same list with a status and an action on every row. Also use when the user mentions verify these emails, clean my list, check for bounces, a high bounce rate, remove invalid emails, a catch-all or accept-all check, email verification, or scrubbing a list before a campaign. Filling empty email, title or company cells goes to enrich-lead-list, a new list of people by job title to build-lead-list, and past contacts who changed jobs to track-job-changes. Sending and CRM writes are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Clean email list

A list the user already has, checked before it is sent to: every address verified, the ones that will bounce dropped, the risky ones flagged and the duplicates removed. Bounces damage the sender's domain for every later campaign, so this is the cheapest insurance in outbound. It ends in the user's own list with every original column untouched, a status and an action on every row, and a summary.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `leads_get_email_status` (hosts often add a prefix, for example `mcp__manifold__leads_get_email_status`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it this skill cannot verify an address; only the free steps (normalise, dedupe, flag by the address) can run.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the ICP, B2B or consumer, the countries the list covers) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **The list**: a file or a paste, and which column holds the email.
- **Its use**: cold outreach, or a send to people who opted in. It decides how strict to be with role and webmail addresses.
- **Age**: when the addresses were collected. A list older than about three months has decayed; verify all of it.
- **Budget**: 1 credit per unique address after the free steps. A 1,000-row list with 900 unique addresses costs about 900 credits. Say it before the first paid call and pass `max_credits` if the user gave a budget, stopping when the running total reaches it.

## Steps

1. **Read the file.** The host reads it and finds the email column. Nothing paid happens yet.
2. **Normalise and dedupe.** Free, done by the host: trim, lower-case, strip `mailto:` and stray punctuation. Remove exact duplicates and keep the row with the most filled columns. Flag rows with the same name at the same domain under two addresses. Drop anything that is not an address at all (no `@`, spaces inside): the tool refuses it.
3. **Flag by the address alone.** Free, done by the host, because the verifier reports none of these:
   - Role addresses: `info@`, `sales@`, `support@`, `hello@`, `contact@`, `admin@`, `team@`, `office@`, `noreply@` and the like. For cold outreach, leave them out of the send by default and verify them only if the user keeps them: they reach a shared inbox, reply less and draw more complaints.
   - Webmail domains (gmail.com, outlook.com, hotmail.com, yahoo.com, icloud.com and the like): a personal address, flagged on a B2B cold list.
   - Disposable domains (mailinator.com, guerrillamail.com, 10minutemail.com and the like): drop them without verifying.
4. **Verify.** `leads_get_email_status` on each remaining address (1 credit each). Run a few at a time; on `ConcurrencyLimit`, wait `retry_after_s` and send fewer at once. An address this account checked in the last 30 days comes back from the cache for free.
5. **Classify** each row from `status` and `mx_records`, with the flags from step 3:
   - Drop: `status: "invalid"`, or `mx_records: false` (the domain takes no mail).
   - Flag: `status: "accept_all"` (the server takes anything, so the address is unproven), `status: "unknown"` (the server refused the check), and the role and webmail addresses from step 3.
   - Keep: `status: "valid"`.
6. **Deliver** the list with every original column and its order untouched and these added at the end: email_status, score, flags (duplicate, role, accept_all, unknown, webmail, disposable), and action (keep, flag, drop). Add a summary: rows in, duplicates removed, kept, flagged and dropped, and credits spent. If the host can write files, it saves a copy; the original stays as it was. Do not send.

## Judgment

- Drop rather than keep when unsure. As a rule of thumb for cold email in 2026, keep a send's bounce rate under about 1 percent and never let it pass 2: above that, mailbox providers start to route the sender's mail to spam.
- `accept_all` is common in B2B: many company mail servers accept every address. Send flagged catch-all rows last, in small batches, and stop if they bounce.
- `unknown` does not improve with a quick retry: the check is cached 30 days. Treat it like `accept_all`.
- An address that went `invalid` usually means the person left. If they matter, finding their current company and new address is [enrich-lead-list](../enrich-lead-list/SKILL.md), and it needs the user's go-ahead.
- Re-verify a cleaned list that waits more than about a month before the send.
- The user's data is the record. Never change a cell they typed; the status and action go in new columns.
- Verification is not permission. A valid address still needs a legal basis to receive cold email; see the [personal data](../create-outbound-plan/references/lead-data.md#personal-data) rules.
- Never send, sequence or write to a CRM; see the [handoff rules](../create-outbound-plan/references/lead-data.md#handoff).

## Related skills

- Empty email, title and company cells filled in the same sheet: [enrich-lead-list](../enrich-lead-list/SKILL.md).
- A new list of people by job title at companies that fit: [build-lead-list](../build-lead-list/SKILL.md).
- Past champions and buyers in the list who moved to a new company: [track-job-changes](../track-job-changes/SKILL.md).
- An opening line for each row once the list is clean: [write-first-lines](../write-first-lines/SKILL.md).
