# Finding app makers to contact

When the user sells to app developers or publishers (an SDK, a monetisation or analytics tool, an agency, a partnership, an acquisition) and wants a list of app makers in a category, with a way to reach them. Checked October 2026.

## Building the list

- **By name match:** `apps_search_apps` matches the query against app titles across a whole store's index, filtered by `category`, and returns the listing with `developer` and the contact fields on each row. It is the most expensive apps tool: 31 credits plus 31 per 100 rows (62 for the default 100, 33 for five). Tested in October 2026: it matches **any** word of the query, so "habit tracker" on Play returned period, transit, flight and running trackers (20,223 matches), and it returns the **biggest apps first** (by installs on Play, by rating count on the App Store), so the long tail sits deep in the pages. Search one distinctive title word ("habit" found 3,182 App Store apps, led by habit trackers, with a burger chain fifth), add `category`, and expect to filter the rows.
- **By chart:** `apps_get_top_charts` (1 credit per 100 rows) for the apps people actually download or pay for in a category, then `apps_get_app` (1 each) for the contact fields. Play chart rows name the `developer`; App Store chart rows do not. Cheaper per qualified row when the user wants the leaders, not everyone.
- **By competitor overlap:** `apps_get_competitors` on an app that is the user's ideal customer finds apps like it, then `apps_get_app` on each.
- **One developer's portfolio:** `more_by_developer[]` on `apps_get_app` lists their other apps.

## Qualifying

Filter before anyone writes a message:

- **Alive:** `updated_at` within the past six months. A large share of any store is abandoned apps.
- **Big enough:** `ratings_count` and, on Play, `installs` (an exact lifetime count) against a floor the user sets. Rating count is the best size proxy both stores share.
- **Fit:** `price` and `is_free` (paid against free with purchases), `category` and `genres[]`, `content_rating` (App Store only), and whatever the user's offer needs (a subscription app, a game, a kids' app).
- **Who is behind it:** `developer` and `developer_website` separate a studio from a solo developer; one studio with twenty apps is one account, not twenty leads. Group rows by `developer_id`.

## Contact data and the rules around it

- `developer_website`, `developer_email` and `developer_address` come from Play's developer contact section and are Play only: the vendor sends none of them for App Store apps, even where the App Store page links a website. For an iOS-only maker, open the listing page for the website link or use `developer_url` (the store's developer page), then find a contact by other means.
- **Play contact fields vary in what they hold.** A company listing can give a personal webmail address as its contact, and a solo developer's address can be just a country. Read each one before treating it as a company inbox.
- **Many of these are individuals.** A solo developer's email is personal data and the address can be a home address. The personal-data, lawful-basis and verification rules for using them belong to [outbound](../../outbound/SKILL.md); finding a person at a company and verifying an address belong there too. Never use a home address for outreach.
- The store contact address is often a support inbox. For a sale or partnership, the person to reach is usually a founder or a growth or product lead, found through the company, not the support address.
- **Never send or contact anyone on the user's behalf.** Deliver the list and draft the first line; the user sends.

## Cost of a typical list

A category search of 200 rows is 93 credits; qualifying from a chart of 100 and naming 50 of them is about 51. Price with `dry_run: true` first and pass `max_credits` when the user sets a budget.
