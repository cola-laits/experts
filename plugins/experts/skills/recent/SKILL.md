---
name: recent
description: List College of Liberal Arts (COLA, UT Austin) faculty who have published recently, college-wide or in one department, from OpenAlex. Use when the user asks who in Liberal Arts or one of its departments has published recently, lately, this month or this semester, or asks for new or latest publications.
argument-hint: [department] [time window, e.g. "last 90 days"] [how many]
---

# Recent COLA publications

Request: $ARGUMENTS

Default count: **10** people (N below, unless the request asks for a different number).

Use the `cola-experts` connector's tools. If they aren't available, tell the user the COLA experts
connector isn't connected and stop; don't answer from general knowledge instead.
The server is reachable only from the UT campus network, so suggest this first: if you're off campus,
connect to the UT VPN, then start a new session. If that isn't it, suggest typing `/mcp` to check
`cola-experts`: if it's missing, remove the Experts plugin, install it again, and start a new session;
if it shows as failed while on campus or the VPN, the server may be down.
If the other `cola-experts` tools are there but `recent_publications` isn't, say the server doesn't offer it yet and stop.

## 1. Understand the request
Work out, from the wording (don't ask unless it's genuinely ambiguous):
- **Department**: if one is named, call `list_departments`, use its slug, and keep its faculty count for the summary line. Up to 3 departments, one call and one section each.
- **Window**: the start date the wording implies ("this month" → 30 days ago, "this semester" → the first day of the current semester, "this year" → January 1). No wording about time → leave `since` out; the server applies its own default and reports it as `since`.
- **How many** (N): the number the request asks for, otherwise the default count, at most 50.
- **Roles**: `["faculty"]` unless the user asks for researchers or graduate students.
- A topic ("who published on immigration"): this skill lists by date only. Say so and suggest `/experts:find <topic>`; don't filter titles by topic.
- One named person: suggest `/experts:profile <name>` instead.

## 2. Call `recent_publications`
Once, with `since`, `department`, `roles`, and `limit` N.
- **Widen once**: if the request gave no window and `people_count` is below N, call again with `since` = `window_start` and say the window was widened. The server holds the data already, so the second call spends nothing.
- `since_clamped: true` → say the server only holds works since `window_start`.
- Never call `get_publications` for the people listed: each call spends an OpenAlex budget shared by everyone. For one person's full list, point to `/experts:profile <name>`.
- On a tool error, report its message and stop; don't build a list from the other tools. For `OpenAlex unavailable …` or a timeout, suggest asking again in a minute. For `recent_publications is not ready …`, say the web team has to run the index build.

## 3. Drop namesakes
The server matches by name, so a different UT person with the same name can appear. Check every listed work:
- Drop a work when its `affiliation` names a UT unit that is not Liberal Arts, not one of the person's departments, and not named in their `title`, and its title and venue are plainly outside their field (an Asian Studies language instructor on an applied physics paper).
- Drop the person when no works remain, and say once under the list how many people were dropped as likely namesakes.
- When the affiliation names another unit but the work could be theirs, keep it and flag it.
- Ignore `match`: it is per person, so a `department` person can still carry a namesake's work, and most `name` people are correct.

## 4. Answer
Start with one line: "{people_count} COLA faculty matched to OpenAlex works since {since}; showing {number shown}."
With a department: "{people_count} of {department}'s {faculty count} faculty matched to OpenAlex works since {since}."

Then the people in the order returned, one block each, with the name alone as its heading:

```markdown
### 1. Name
Title · Department · [Profile](profile_url)

- **Recent works:** {number shown} of {works_in_window} since {since}, one sub-bullet each as [Title](doi_url, else openalex_url) (Mon YYYY, type, venue)
- **Contact:** email · [UT Experts](ut_experts.url) when listed
- **Flags:** see below (leave this line out if there are none)
```

Flags:
- "Affiliation names another unit: {affiliation}; verify" for a work kept in step 3 despite its affiliation.
- "Not in UT Experts" (confirm willingness first) when `ut_experts.listed` is false.
- "On leave" when `on_leave` is true for one of their `departments`.
- Researcher or graduate student, when the request widened `roles`.

End with: *OpenAlex data fetched {openalex_as_of}, covering works since {window_start}; COLA data as of {data_as_of}. The list is incomplete: it holds only works that record a UT Austin affiliation and an author name matching a COLA name, books and the humanities are under-reported, and the last few weeks lag. Absence from the list does not mean no publication, and matches are by name, so confirm authorship before contacting anyone.*

Rules:
- Use only facts in the response. Link every work to its `doi_url`, else `openalex_url`. Never invent publications, titles, or contact details.
- `people_count` 0 → say no works were matched since {since}, never that nobody published.
