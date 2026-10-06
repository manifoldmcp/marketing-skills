# List cleaning

A list the user already has, checked before it is sent to: every address verified, the ones that will bounce dropped, the risky ones flagged and the duplicates removed. Bounces damage the sender's domain for every later campaign, so this is the cheapest insurance in outbound. It ends in the user's list with a status and an action on every row.

## Inputs to settle first

- **The list**: a file or a paste, and which column holds the email.
- **Its use**: cold outreach, or a send to people who opted in. It decides how strict to be with role and webmail addresses.
- **Age**: when the addresses were collected. A list older than about three months has decayed; verify all of it.
- **Budget**: 1 credit per unique address after the free steps: a 1,000-row list with 900 unique addresses costs about 900 credits. Say so before starting; pass `max_credits` if the user gave a budget, and stop when the running total reaches it.

## Steps

1. **Normalise and dedupe.** Free, done by the host: trim, lower-case, strip `mailto:` and stray punctuation. Remove exact duplicates and keep the row with the most filled columns. Flag rows with the same name at the same domain under two addresses. Drop anything that is not an address at all (no `@`, spaces inside): the tool refuses it.
2. **Flag role addresses.** Free: `info@`, `sales@`, `support@`, `hello@`, `contact@`, `admin@`, `team@`, `office@`, `noreply@` and the like. For cold outreach, leave them out of the send by default and verify them only if the user keeps them: they reach a shared inbox, reply less and draw more complaints.
3. **Verify.** `leads_get_email_status` on each remaining address (1 credit each). Run a few at a time; on `ConcurrencyLimit`, wait `retry_after_s`. An address this account checked in the last 30 days comes back from the cache for free.
4. **Classify** each row from `status`, `result`, `disposable`, `webmail` and `mx_records`:
   - Drop: `status: "invalid"`, `result: "undeliverable"`, `disposable: true`, or `mx_records: false` (the domain takes no mail).
   - Flag: `status: "accept_all"` (the server takes anything, so the address is unproven), `status: "unknown"` (the server refused the check), `result: "risky"`, `webmail: true` on a B2B cold list (a personal address), and the role addresses from step 2.
   - Keep: `status: "valid"` with `result: "deliverable"`.
5. **Deliver** the list with every original column untouched and these added: email_status, result, score, flags (duplicate, role, accept_all, unknown, risky, webmail, disposable), and action (keep, flag, drop). Add a summary: rows in, duplicates removed, kept, flagged and dropped, and credits spent.

## Judgment

- Drop rather than keep when unsure. Keep the bounce rate of a send under about 2 percent; above that, mailbox providers start to route the sender's mail to spam.
- `accept_all` is common in B2B: many company mail servers accept every address. Send flagged catch-all rows last, in small batches, and stop if they bounce.
- `unknown` does not improve with a quick retry: the check is cached 30 days. Treat it like `accept_all`.
- Verification is not permission. A valid address still needs a legal basis to receive cold email; see the router's [personal data](../SKILL.md#personal-data) rules.
- An address that went `invalid` usually means the person left. If they matter, `leads_get_person` (3 credits) shows their current company, and `leads_get_email` (6 credits on a hit) finds the new address; that is [enrichment](enrichment.md), and it needs the user's go-ahead.
- Re-verify a cleaned list that waits more than about a month before the send.
