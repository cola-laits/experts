# experts

A Claude Code plugin marketplace named `cola-experts` (repo: `cola-laits/experts`).

Find College of Liberal Arts faculty experts for media requests and research highlights.

## Moving from the earlier location

If you added this marketplace before it moved to `cola-laits/experts`, remove the old copy first:

1. **Settings** → **Customize** → **Plugins** → **+ Add** → **Manage marketplaces**.
2. Next to **cola-experts**, click **⋯** → **Remove**, then **Done**. This also removes the
   Experts plugin.
3. Follow the Install steps below.

## Install

In the Claude desktop app, open **Settings** → **Customize** → **Plugins**:

1. **+ Add** → **Add marketplace**, enter `cola-laits/experts`, and click **Sync**.
2. On the **Discover** tab, next to **Experts**, click **Add to this computer**.
3. Start a new session in the **Code** tab.

## Update

1. **Settings** → **Customize** → **Plugins** → **+ Add** → **Manage marketplaces**.
2. Next to **cola-experts**, click **⋯** → **Check for updates**, then **Done**.
3. Back in **Plugins**, click **Experts** → **Update**.
4. Start a new session in the **Code** tab. Sessions already open keep the old version.

If it still acts like the old version, click **⋯** next to Experts → **Remove**, then install it
again from **Discover**.

## Use

In a Code session, type one of these commands followed by your request:

- `/experts:find` finds people who can speak to a topic. Give it a topic or paste a reporter's
  request, e.g. `/experts:find voter turnout in Texas`.
  - It lists 5 people unless you ask for a different number (up to 10).
  - It searches faculty only unless you ask for researchers or graduate students too.
  - Say whether it's for a reporter or for a story you want to write; for a story it flags
    people the college has already featured.
  - You can narrow it, e.g. "only in Sociology" or "someone who has studied this recently".
  - Each person comes with why they fit, the evidence, contact details, recent publications,
    news mentions, and flags such as "on leave" or "not listed in UT Experts; confirm
    willingness first".
- `/experts:profile` shows everything known about one person, so you can check them before a
  referral. Give it a name or an EID, e.g. `/experts:profile Jane Doe`. If several people share
  the name, it asks which one you mean.
- `/experts:recent` lists who has published lately, newest first.
  - With nothing after it, it covers the whole college. You can name up to 3 departments,
    e.g. `/experts:recent History`.
  - It lists 10 people unless you ask for a different number (up to 50).
  - It can look back about six months at most; you can ask for a shorter period, e.g. "this
    month".
  - The list is matched by name from an outside publication database, so it can miss work,
    especially books and humanities. Treat it as a starting point, not a complete record.

You can also just ask without a command, e.g. "Who in Liberal Arts could talk to a reporter
about voter turnout?"

Always check the details before contacting someone. Each answer links to its sources and says
how current the data is.

The tools only answer on the UT campus network. If a skill says the campus network is required,
connect to the UT VPN and ask again; you don't need a new session. If a skill says the connector
isn't connected, remove the Experts plugin, install it again, and start a new session; if that
doesn't fix it, the server may be down.
