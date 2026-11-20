# Deliverability

Whether cold email reaches the inbox, and the list cleaning that protects it. Bounces and complaints damage the sending domain for every later campaign, so this comes before copy. Checked October 2026; mailbox providers change their rules without much notice, so re-check the primary pages before quoting a threshold to a client.

## What the 2024-2025 sender rules cover

| Provider | Applies to | Threshold | Requires | Since |
|---|---|---|---|---|
| Gmail | Personal Gmail accounts only, not Google Workspace recipients | Close to 5,000 a day to personal Gmail from one primary domain; bulk status is then permanent | SPF and DKIM, DMARC at least `p=none` aligned with the From domain, TLS, valid forward and reverse DNS, one-click unsubscribe on marketing mail, spam rate under 0.3% | February 2024; one-click by June 2024; enforcement "ramping up" from November 2025 |
| Yahoo | Yahoo and AOL consumer mailboxes | Bulk senders | Announced jointly with Gmail and the same in substance (not re-checked against Yahoo's page for this note) | February 2024 |
| Microsoft | Outlook.com, Hotmail, Live and MSN, not Microsoft 365 tenants | 5,000 or more a day from one From domain | SPF and DKIM passing, a DMARC record at least `p=none`, aligned | 5 May 2025; failures get `550 5.7.515` |

Sources: [Google's sender FAQ](https://support.google.com/a/answer/14229414), [Microsoft's 5.7.515 page](https://support.microsoft.com/en-us/outlook/fix-ndr-error-550-5-7-515-in-outlook-com).

**How this bites on cold email.** B2B cold email mostly goes to company domains on Google Workspace or Microsoft 365, which these rules do not formally cover; in our own sample 7 of 9 checked B2B domains ran on Google and one on Microsoft ([field notes](field-notes.md)). So a cold sender is rarely a "bulk sender" in the rules' sense. What still applies:

- **The filters are the same.** Workspace and 365 inboxes run the providers' spam filtering on every sender. Unauthenticated mail is what gets rejected first, at any volume.
- **Gmail's spam-rate lines are the best public guide to tolerance:** keep user-reported spam under 0.1% and never reach 0.3% (Google). Above 0.3% Google stops offering mitigation until seven straight days below it. Cold email draws complaints at many times the rate of opted-in mail, so a list of people who never asked to hear from the user has little room.
- **Some recipients are on personal mailboxes.** A founder or freelancer at a gmail.com address is under the full rules, and is a personal address under data law too ([legal](legal.md)). Keep webmail off a B2B cold list.
- **One-click unsubscribe is required for marketing mail to Gmail.** For cold email an opt-out line or link is required by law anyway (CAN-SPAM, PECR); a list-unsubscribe header costs nothing and is what the filters look for.

## Setup that still holds

- **Send from a secondary domain**, never the main one, so a burned reputation does not take the company's own mail with it. SPF, DKIM and DMARC on it, aligned. Business domains replied at 5.2% against 2.5% for freemail senders (Hunter, 31 million emails, 2025 data).
- **Two or three mailboxes per domain, 20 to 50 new emails a day each.** Accounts sending 20 to 49 a day replied best, at 5.7% against a 4.5% average, and open rates were highest under 100 a day (Hunter). Instantly's 2026 report advises starting a new domain at 5 to 10 a day and ramping over four to six weeks, and found steady sending patterns replied 15% to 20% better than erratic ones (vendor data, no method published).
- **Platform caps are hard limits.** A paid Google Workspace user can send 2,000 messages a day to at most 2,000 unique external recipients; an account still on its trial is held at 500, until the domain has paid at least $100, which can take up to 75 days ([Google](https://knowledge.workspace.google.com/admin/gmail/gmail-sending-limits-in-google-workspace)). The safe cold volume is far below the cap.
- **Volume follows capacity, not the market.** The list size for a month is mailboxes × daily sends × working days, never `rows_available`.
- **Google Workspace senders replied at 5.9%, Microsoft 365 just over 4%** (Hunter). Correlational, but a reason not to pick 365 for a new sending setup without a reason.
- **Turn off open and click tracking.** Campaigns without open tracking replied at 7.4% against 4.4% (Hunter); Belkins dropped pixels in 2024 because they hurt deliverability. Apple's Mail Privacy Protection fills open counts with machine opens anyway.
- **Warm-up:** standard practice and harmless at low volume, but no independent study shows whether warm-up networks still help or are now recognised by the filters. Treat a gradual ramp of real sends as the part that matters.

## Bounces

- Keep a send's hard bounces under about 2% (Instantly's line) and aim under 1%. The average Hunter user bounced 3.6% in 2025, so the typical sender is already over it: verify every list.
- Watch bounces per batch. A catch-all batch that bounces more than the verified batch is the signal to stop sending catch-alls from that domain.
- A rising bounce rate on an old list is decay, not bad luck: people leave jobs. Re-verify anything older than about a month.

## Cleaning a list

What the old "clean my list" job did; the steps are mostly free and the verifier is 1 credit an address.

1. **Free, done by the host:** trim, lower-case, strip `mailto:` and punctuation; drop anything that is not an address; remove exact duplicates keeping the row with the most filled columns; flag the same person at the same domain under two addresses.
2. **Flag by the address alone:** role addresses, webmail domains and disposable domains (see [lead data](lead-data.md#verification)), because the verifier reports none of them. For cold outreach leave role addresses out by default.
3. **Verify the rest** with `leads_get_email_status`, a few at a time. A 1,000-row list with 900 unique addresses costs about 900 credits; say so first.
4. **Classify:** drop `invalid` and `mx_records: false`; flag `accept_all`, `unknown`, role and webmail; keep `valid`. Note the `mail_provider` so gateway domains can be batched apart.
5. **Deliver** the user's list with every original column untouched and these added: email_status, score, mail_provider, flags, action (keep, flag, drop), plus counts in, removed, kept, flagged, dropped and credits spent. Never send.

Verification is not permission: a valid address still needs a legal basis to receive cold email.

## Without the connector

Check the sending domain's SPF, DKIM and DMARC with any DNS lookup tool or the provider's admin console, read Google Postmaster Tools for the domain's spam rate where volume to Gmail is enough to report, and send a test to a Gmail and an Outlook.com seed address. A list can be normalised, deduped and flagged by hand; verification needs a verifier, so say plainly that an unverified list is a bounce risk.
