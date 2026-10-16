# Field notes

What Manifold has seen in its own runs of the `leads_*` tools. Small samples: read them as evidence of how the data and the tools behave, not as market rates. No address or name from these runs is kept here. Newest first.

## One ICP, end to end (15 October 2026)

US companies of 51 to 200 staff matching the keyword "SaaS", their marketing leaders, ten reveals and an independent re-check. 83 credits: one company search (4), one people search (4), ten reveals (60), fifteen verifications (15).

**The company search is loose.** `leads_search_companies` with `keywords: ["SaaS"]`, `employee_ranges: ["51,200"]` and `locations: ["United States"]` returned 40 rows of 1,724 available. Only 15 were "Software Development"; the rest included IT services, HR services, insurance, hospitality, oil and gas and higher education, each mentioning SaaS somewhere. One record carried its industry in French ("Développement de logiciels"). `headcount_growth[]` was filled on 6 of 40 rows.

**About a third of small accounts had a findable buyer.** `leads_search_people` with `titles: ["Marketing"]` and `seniority: ["executive"]` inside the 40 domains returned 18 people at 15 companies, everything there was (`rows_available` 18). One row was junk (a one-letter name, headline "test"). Two companies had two or three marketing leaders each.

**Person location is not company location.** 6 of the 18, at US-headquartered companies, lived in Canada (two), the UK, Israel, India and Hong Kong.

**Time in role.** 15 of 18 rows carried `title_start`. 3 of the 15 had started in the past 12 months, 2 in the past six. One headline still named the previous employer while `title` and `title_start` showed the new role from four months earlier.

**The waterfall found everyone.** `leads_get_email` by profile URL on ten marketing leaders: 10 hits, all `valid`, all `found_by` Findymail. `meta.providers_tried` read icypeas then findymail on every call: the first entry is the Icypeas person lookup that resolves the profile to a name and domain, not a finder that missed. `confidence` was null and `sources[]` empty on every hit. Address formats varied (first.last, first, first initial plus last, full first name plus last).

**A reveal's `valid` is the finder's verdict.** Re-checking the ten with `leads_get_email_status`: 8 `valid` (score 99), 1 `accept_all` (score 95, "risky"), 1 `unknown` with `mx_records`, `score` and `mail_provider` all null. Findymail returns only addresses it has verified, and the tool passes that on without a second check, so on a catch-all domain a reveal can say `valid` where the verifier says unproven.

**Most B2B mail here runs on Google.** Of the nine re-checks that answered, `mail_provider` was google for 7, microsoft for 1 and other for 1. These recipients are on Workspace, outside the formal reach of Gmail's bulk-sender rules ([deliverability](deliverability.md)).

**Probing a domain with a made-up address does not detect catch-all.** Five checks of an invented local part at five of the domains all came back `unknown` with every field null, including at the domain that the re-check had just called `accept_all`. Do not use invented addresses to test a domain; check a real address instead.
