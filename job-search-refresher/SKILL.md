---
name: job-search-refresher
description: Use when Kaelig asks to refresh his job-search pipeline, find new roles, run another sweep of the design systems job market, look for fresh listings, or names new companies to investigate. Also use when he says it's been a while since the last search. Do not use for single-role questions, role comparisons already on the list, or interview prep.
---

# Job Search Refresher

Run a fresh, deep sweep of the design systems and adjacent frontend job market for Kaelig. Return ONLY roles that have not been surfaced in the conversation before. When a candidate role appears at a company that's unfamiliar or shows red flags, pause and run the `company-evaluator` skill before including it.

## Context

Kaelig left Intuit on April 6, 2026 and is actively job hunting. He is a Staff/Principal-level frontend engineer, co-chair and co-founder of the W3C Design Tokens Community Group, based in Kirkland, WA. His ranked priorities:

1. **Design systems specifically** — tokens, components, tooling that hundreds of engineers depend on
2. **Remote or hybrid commutable from Kirkland** (Seattle/Bellevue/Redmond also work)
3. **Adjacent roles** — frontend platform, developer experience, design tooling

Cultural benchmark: **Shopify**. Aspirational targets: **1Password, Figma**. He benchmarks role evaluation against companies on his S/A/B tier list (Anthropic, Airbnb, Airtable, Netflix, etc.).

## Blocklist

Never include these companies, regardless of role match. Exclude them silently — no role card, no "investigated with no openings" mention.

- **Elon Musk-owned**: Tesla, SpaceX, X (formerly Twitter), xAI, Neuralink, The Boring Company
- **Sam Altman-owned or led**: OpenAI, Worldcoin, Tools for Humanity, Retro Biosciences
- **Defense / weapons**: Palantir, Anduril, Lockheed Martin, Raytheon, Northrop Grumman, General Dynamics, BAE Systems, L3Harris, Boeing Defense, Booz Allen Hamilton

If a company's status is ambiguous (parent/subsidiary relationships, ownership stakes you can't verify, defense divisions inside otherwise-civilian companies), default to excluding and surface the question for Kaelig to confirm.

## When this skill triggers

Trigger on any phrasing that implies "find me new roles" or "refresh my pipeline":

- "Find new roles" / "Refresh the list" / "Search again" / "Any new openings?"
- "What's new on the market?" / "Run another sweep" / "Update my pipeline"
- "Deep research the market" / "Look for fresh listings"
- Naming new companies to investigate as part of a search
- Saying "it's been a few days/a week" since the last search

Do NOT trigger when Kaelig is asking about a single specific role, asking to compare two roles already on the list, or doing application/interview prep work.

## Workflow

### Step 1: Build the dedupe list

Before searching, scan the conversation history and assemble a list of every role and every company already surfaced. Roles already shown — even if mentioned only briefly — are excluded from the output. Companies where a specific role was mentioned should still be searched if they may have OTHER relevant roles that haven't been surfaced.

If the conversation is long, search prior turns for previously surfaced roles using whatever conversation-search tool is available (e.g. `conversation_search` on Claude.ai). Use queries like "design systems engineer role" or "frontend engineer Staff".

State the dedupe list briefly in your thinking, e.g., "Already surfaced: Netflix Commerce DS, Discord Mana, Anthropic UI Platform, [...]. These are excluded from this search."

### Step 2: Launch deep research

Run deep web research — `launch_extended_search_task` on Claude.ai, or dispatch a research subagent on Claude Code (the `general-purpose` Agent with WebSearch and WebFetch). Build a prompt that:

1. States Kaelig's profile and priorities (copy from Context section above)
2. **Lists every previously-surfaced role explicitly** with instructions to exclude them
3. Tells the research agent today's date and that listings must be live
4. Directs the agent to specific search territories (rotate these between searches so the same companies aren't re-checked every time):

   **Standard sweep (every refresh):**
   - LinkedIn jobs filtered to "design systems engineer," "frontend platform engineer," "design engineer" — last 14 days, Remote US or Seattle/Kirkland/Bellevue
   - Greenhouse, Lever, Ashby boards — design systems engineering roles posted in last 30 days
   - designsystems.jobs and jobs.intodesignsystems.com — new US remote engineering roles
   - Aspirational targets: 1Password and Figma careers pages

   **Rotating deep-dive (pick 1–2 each refresh):**
   - Dev tooling Series B+: Vercel, Storybook/Chromatic, Sourcegraph, LaunchDarkly, Retool, Builder.io, Plasmic, Knapsack, Specify, Bit, Nx, Cursor, Replit, Codeium, Sanity, Contentful, Stytch, Clerk
   - AI companies: Anthropic, Perplexity, Harvey, LiveKit, Magic, Poolside, Cursor, Cognition
   - Pacific Northwest commutable: Microsoft (Redmond), T-Mobile (Bellevue), Smartsheet (Bellevue), Snowflake (Bellevue), Robinhood (Bellevue), Snap (Bellevue), Zillow, Redfin, Expedia, Costco, Starbucks tech
   - S/A tier list companies: Anthropic, Airbnb, Airtable, HRT, Netflix, Figma, Dropbox, NVIDIA, Uber, Pinterest, Roblox, Robinhood, Jane Street, Instacart, Brex, Meta, Plaid, DE Shaw, Google, Coupang, Databricks
   - Established design system teams worth re-checking: Shopify Polaris, Atlassian ADS, Adobe Spectrum, Microsoft Fluent, Salesforce Lightning, IBM Carbon, GitLab Pajamas, Twilio Paste

5. Asks the agent to flag any company it's unfamiliar with or that shows risk signals (recent layoffs, exec churn, strategic pivots, sub-3.0 Glassdoor) so they can be deferred for company-evaluator deep-dive.

### Step 3: Triage results

For each role the research returns:

- **Confirmed open + well-known company** → include in the artifact
- **Confirmed open + unfamiliar or risky company** → STOP and run the `company-evaluator` skill on that company (using deep research) before deciding whether to include it. Use the same deep-research approach as Step 2, with the company-evaluator skill's report structure as the prompt. Only include the role if the evaluator returns ★★★ or higher.
- **Stale/filled** → exclude
- **Already surfaced** → exclude (this is a check on the dedupe step)

Companies that always count as "well-known + safe to include without evaluation": Netflix, Anthropic, Figma, Shopify, GitHub, Microsoft, Google, Meta, Apple, Airbnb, Stripe, Discord, Block, Databricks, Pinterest, Uber, LinkedIn, Adobe, Atlassian, Salesforce, 1Password, Vercel, Sourcegraph, Linear, Notion, Asana, Instacart, Ramp, Plaid, DoorDash, Snap, Robinhood, Brex, Datadog, Snowflake, HubSpot, Eventbrite, Zendesk, Smartsheet, Carta.

Anything outside that list — especially Series A/B startups, recently-rebranded companies, or companies with public layoff history — gets the company-evaluator treatment first.

### Step 4: Deliver as a markdown artifact

Title the artifact: `New Design Systems Roles for Kaelig — [Date]`

Structure:

```
# New Design Systems Roles for Kaelig — [Month Day, Year]

[1-paragraph opener: how many new roles found, what companies, the standout, and any noteworthy patterns. State explicitly that previously-surfaced roles have been excluded.]

## Tier 1: Apply this week
[Roles that meet Staff/Principal level, design systems focus, and remote-or-Kirkland-commutable. Each role gets the role card format below.]

## Tier 2: Strong adjacent roles
[Frontend platform, DX, design tooling, or roles with one significant caveat (level mismatch, location, etc.).]

## Tier 3: Monitor or speculative
[Roles below level, niche, or where outreach makes sense without a posted listing.]

## Companies investigated via company-evaluator
[For any company that triggered a deep evaluation, include a 2-sentence summary and the fit rating. Link to the full evaluator report if it was created as a separate artifact.]

## Companies searched with no new openings
[A single short paragraph naming companies checked that had nothing new. Helps Kaelig see the search was exhaustive without padding the artifact.]
```

Role card format (use for every role in Tier 1, 2, 3):

```
### [Number]. [Company] — [Exact Job Title]

| Detail | Info |
|--------|------|
| **URL** | [direct application link] |
| **Level** | [Staff / Senior / Principal / etc.] |
| **Location** | [city + remote policy] |
| **Comp** | [base range + equity if known, otherwise "not listed"] |
| **Match** | ★★★★★ / ★★★★ / ★★★ / etc. |

[2–4 sentence assessment: what the role actually involves, how it maps to design systems / tokens / Kaelig's profile, what makes it interesting or risky. Be direct, no filler.]

**Red flags:** [explicit list, or "None significant"]
```

### Step 5: Close the loop

End the artifact with one short paragraph naming the top 1–3 roles to apply to first and why. No filler, no recap of what's already in the artifact.

## Writing style

- Write without bullshit. Josh Bernoff principles. No "it's worth noting that," no "in today's market," no "comprehensive."
- Direct statements. Say what's true and what's not.
- Use contractions.
- Bold names and titles on first mention only.
- Short paragraphs (2–4 sentences).
- If a data point can't be confirmed, say so. Don't pad with guesses.
- Don't soften red flags.

## What NOT to do

- Don't re-surface roles already shown in the conversation. The whole point is freshness.
- Don't include roles you couldn't verify as currently live. Stale aggregator listings are worse than nothing.
- Don't skip the company-evaluator step for unfamiliar companies. Kaelig has been burned by chasing roles at companies with hidden instability.
- Don't recommend roles below Senior level unless they're at an aspirational company AND there's a clear path to negotiate the title up.
- Don't include companies on the **Blocklist** (above). It's a hard filter — exclude silently, no role card, no mention.
- Don't include companies that have been deprioritized in past conversations (e.g., Cruise).
- Don't pad the search by re-checking companies that yielded nothing in the previous 1–2 refreshes. Rotate territories.
