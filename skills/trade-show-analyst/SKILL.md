---
name: trade-show-analyst
description: Plan which trade shows to attend and which exhibiting companies to meet, starting from the website of the user's product or company. Uses the DataOrigin tools to rank trade shows by sector, country and date, shortlist the exhibitors that fit the ideal customer, name the person to approach and link each result in the DataOrigin app. Use it when the user asks which trade shows to attend, who to meet at a trade show, or how to prepare a fair.
---

# Trade show analyst

Use this skill to turn a company website into a ranked list of trade shows, a shortlist of exhibiting companies for one of them, and the people to approach. Work with the DataOrigin tools. Do not use outside knowledge to fill gaps in the data.

## Rules that always apply

- Reading and searching are free. Only `unlock_event` and `unlock_company_contacts` spend credits.
- Before any call that spends credits, call it without `confirm`. The answer is the price and nothing is charged. Tell the user the price and the balance (`get_credits`). Send `confirm=true` only after the user says yes to that price.
- One approval covers what the user saw. For several companies, list them with the cost of each one and the total, and ask once. Ask again when the list or the price changes.
- Never unlock to look around. Use `search_events`, `get_event` and the preview described below first.
- Never guess. If a name, email, phone or job title is not in a tool answer, do not write it.
- The user's explicit instructions override this skill.

## What the tools cannot do

- There is no filter for "distributor", "buyer type" or any other business model. Use industry, category, size, country and text, and say so when the user asks for a type the filters do not have.
- Companies can be found by name across DataOrigin with `search_companies` and `query`, without a trade show. Comparing the participants of several trade shows needs one `search_companies` call per trade show. There is no single call that compares them.
- The assistant cannot write or change the ideal customer profile. It can read it. If the user wants it saved, tell them to do it in the app settings at https://dataorigin.es/app/settings. `generate_app_url` has no page for it.
- The assistant cannot buy credits, change the plan or delete data. The user does this in the app.

## Step 1. Know who the user sells to

1. Call `get_target_profile`. It returns target countries, industries, company sizes, buyer roles and reference companies, if the user saved them.
2. If the user gave a website, read it. Note what the company sells, to whom, in which countries and in which industries. If you cannot open web pages, ask the user to paste a short description instead.
3. If there is a saved profile, use it as the base and use the website to refine it. If both disagree, show the difference and ask which one to follow.
4. If there is no saved profile, state the profile you inferred from the website in four lines (what they sell, buyer industries, buyer countries, company size and buyer roles). Ask the user to correct it before you search. Do not continue with a guess the user has not seen.

## Step 2. Find and rank trade shows

1. Call `get_event_filters` when you need the exact sector keys, region names or attendee ranges. Turn the buyer industries into sector keys.
2. Call `search_events` with the sector, the countries or regions and a date window. Ask the user for the window if they did not give one. Use `sort_by=upcoming` for the next trade shows.
3. Page through the results with `offset_next` until you have enough candidates. The first page is not always a fair sample. Read at least two pages before you rank, and say how many trade shows you looked at.
4. Rank the candidates and keep five to ten. Use only numbers from the tools. Useful signals are
   - sector match with the profile
   - country and distance to the buyer countries
   - dates, and whether there is time to prepare
   - the number of exhibiting companies, attending companies and speakers registered in DataOrigin. This is what DataOrigin has on record, not the real total of the trade show. Say so when you show the numbers
   - for the top candidates, the number of companies that match the profile (see step 3, preview)
5. Show a table with name, city and country, dates, exhibitors loaded, fit with the profile, and one reason of one sentence. If a number is missing, write "not loaded". Do not estimate it.
6. Say which trade shows the account can already open (`has_access`) and the price in credits of the others (`price`).

## Step 3. Shortlist exhibiting companies for one trade show

Ask the user to pick one trade show, or pick the first one and say so.

1. Check access with `get_event`. If `has_access` is true, go to point 3.
2. If `has_access` is false, do not unlock yet. Try `search_companies` with `event_id`, `preview=true`, `type=["Exhibitor"]` and the filters from the profile, so the counts are of exhibiting companies that fit and not of the whole trade show. When the account has the preview, this gives the counts, a few companies in full and the rest locked. Use the counts to tell the user how many companies fit. Say that the other companies are locked, give the unlock price, and ask whether to unlock. If the preview answers with an error, say that the list is locked and give the price from `search_events`. Do not name companies you cannot see.
3. Call `search_companies` with `event_id`, `type=["Exhibitor"]` and the filters from the profile. Add `country_business`, `industry_business`, `categories_business` and `employees_business` as needed. Use `get_company_filters` with the `event_id` to get valid values.
4. If the user asks for attending companies or speakers, repeat with `type=["Attendee"]` or `type=["Speaker"]`.
5. Pick the companies that fit best. For each one, call `get_company` with the `company_id` and the `event_id` to read what it does, its size, its website and its stand.
6. Write one line per company with the reason it fits, using facts from the answer (industry, size, country, what it does). Say "does not fit" for clear misses and "unclear" when the data is thin. Keep the shortlist to ten companies unless the user asks for more.

## Step 4. The person to approach

Read the `get_company` answer and keep three cases apart.

1. People are listed. Choose the person whose role matches the buyer roles of the profile and give the name and role. Add the LinkedIn address, email and phone when they are present. A name that is visible stays visible. Show it even when the contact details are missing.
2. The answer lists `contacts` under `hidden`. This means no email, phone or contact point is visible to this account for that company. Read the entry.
   - It has `how_to_unlock`. The company is locked for this account. Say that no contact details are visible. Call `unlock_company_contacts` without `confirm` to get the price of unlocking the company and tell the user that unlocking shows what DataOrigin has, which can be nothing for some companies. Never promise a contact.
   - It has no `how_to_unlock`. The account already has access and DataOrigin has no contact on record. Say so and do not offer to unlock.
3. The answer lists another block under `hidden` with the reason that it is not included in the plan. Say that the plan does not include it. Unlocking contacts does not change that.

Other rules for this step.

- If a company has no people in the data, say so. Suggest the company website or the stand as the way in. Do not invent a role or a name.
- Contacts of a trade show the account has unlocked are already visible. Check there before you offer to unlock a company.
- Unlock only the companies the user approved.

## Step 5. Links and saving

- For each trade show and company you recommend, call `generate_app_url` with `normalized_event_name` (and `normalized_company_name` for a company) and put the link next to it.
- Save to bookmarks (`add_bookmark`) or save a filter (`save_filter`) only when the user asks.

## Output

End with a short summary the user can act on.

- The ranked trade shows with the reason for each.
- For the chosen trade show, the shortlist with the reason and the person to approach.
- The links.
- Credits. What was spent, what the user can still unlock and at what price. If nothing was spent, say so.
- Open points, for example a locked list, a missing profile or a window of dates the user did not give.
