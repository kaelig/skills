---
name: company-evaluator
description: Produce a structured employer evaluation report for a specific company, covering design leadership, C-suite, financials, red flags, engineering culture, design talks, podcast appearances, and CEO/CTO mission statements. Use this skill whenever the user asks to "evaluate," "research," "look into," or "give me a breakdown of" a company as a potential employer — even if the phrasing is casual like "what do you think about working at X" or "is X worth applying to." Also trigger when asked to compare companies or assess employer fit. This skill produces a consistent, repeatable report format every time.
---

# Company Evaluator

Produce a comprehensive employer evaluation report for a Staff/Principal-level frontend engineer and design systems specialist evaluating a company as a potential employer. The report must follow the exact structure below every time — no reordering, no skipping sections, no improvising the format.

## Context

The person using this skill is a Staff/Principal IC with 10+ years of experience in design systems (Shopify Polaris, Salesforce Lightning, Intuit IDS), co-founder of the W3C Design Tokens Community Group, based in Kirkland, WA. They care about:

1. Whether the company has a real design systems function (not a side project)
2. Whether design leadership has organizational weight (VP/CDO vs. buried under product)
3. Remote/hybrid flexibility from the Pacific Northwest
4. Staff+ IC career ceiling (not just junior/mid growth paths)
5. Organizational stability — layoffs, exec churn, strategic pivots
6. Compensation and equity upside relative to stage and trajectory

## Research approach

Do not write the report from memory. Search aggressively for every section. Use multiple queries per section to triangulate. Prioritize primary sources:

- **Leadership/org**: LinkedIn, The Org, RocketReach, company about pages
- **Financials**: Crunchbase, PitchBook, Tracxn, Sacra, SEC filings (if public), press coverage of funding rounds
- **Engineering culture**: Company engineering blog, GitHub org, Key Values profile, Greenhouse/Lever job postings (they reveal tech stack and team structure), Glassdoor/Blind
- **Talks/podcasts**: YouTube search (`"[company name]" design system`), podcast directories, conference speaker pages
- **Red flags**: Blind, Glassdoor reviews, layoff trackers (layoffs.fyi), news coverage of restructurings

When a data point can't be confirmed, say so explicitly. Never fill gaps with plausible-sounding guesses.

## Report structure

Use this exact structure. Every report must have all 9 sections in this order. If a section yields nothing, write "Nothing found" with a note on what was searched.

---

### Opening verdict

A single paragraph — 3 to 5 sentences max. State the company's core business, its current trajectory (growing? contracting? pivoting?), and the bottom-line assessment for a Staff/Principal design systems engineer. Include the strongest signal for and the strongest signal against. This paragraph should let someone decide in 30 seconds whether to keep reading.

---

### 1. Design leadership

Who leads design? Title, name, background, tenure, reporting line. Where does design sit in the org — under product, engineering, or its own function? Is there a VP of Design, CDO, or Head of Design? What's their track record — did they come from a company known for design quality?

Specifically look for: whether the company has a named design system, whether there's a dedicated design systems team, and what level the most senior IC on that team holds.

Include LinkedIn URLs where found.

---

### 2. Executive team

Current CEO, CTO, CPO, CFO. For each: name, tenure, where they came from, one sentence on their public posture (if any). Flag any executive hired in the last 12 months or any seat that has turned over more than once in 3 years.

The goal is to assess stability. A company where 3 of 4 C-suite seats have changed in 2 years is a different bet than one with a founding team still intact.

---

### 3. Financials

For private companies: last known valuation, most recent funding round (amount, date, lead investors), revenue estimates if available (Sacra, Latka, press), headcount trajectory, and cash position if reportable.

For public companies: market cap, revenue, margins, stock trajectory over 12 months, and any notable analyst commentary.

Always include: how compensation for a Staff/Principal frontend role benchmarks at this company. Use levels.fyi, Glassdoor, Blind, or job posting salary bands.

---

### 4. Red flags

Be direct. This section should answer: "What would make a senior engineer regret joining?" Look for:

- Layoff history (dates, percentage of headcount, which teams)
- Executive churn (see section 2, but synthesize the pattern here)
- Strategic pivots (how many times has the product direction shifted in the last 3 years?)
- Glassdoor/Blind sentiment — specifically management ratings and recurring themes
- Competition pressure (is the core product under threat?)
- Anything else that would give a Staff+ IC pause

Don't soften bad news. If the signals are bad, say so.

---

### 5. Engineering culture

How does engineering work at this company? Look for:

- Tech stack (frontend specifically: React? Vue? Svelte? TypeScript? Monorepo?)
- IC career ladder — does Staff/Principal exist? Are there people at that level?
- Design systems infrastructure: is there a design system? What's it called? Is it public (open-source or documented)?
- Engineering blog quality — does the team write about technical problems, or is the blog just product announcements?
- Shipping cadence and development methodology signals
- Key Values profile or equivalent culture signals

---

### 6. Design talks (with links)

Search YouTube, conference archives, and speaker pages for talks by anyone at this company about design, design systems, frontend architecture, or developer experience.

Format each entry as:
- **Title** — Speaker Name, Conference/Event (Year)
  URL

If nothing is found, note what was searched (e.g., "Searched YouTube for '[company] design system', '[company] design engineering', '[company] Config/Clarity/CSS Day' — no results").

---

### 7. Podcast appearances (with links)

Search for podcast episodes featuring the company's design leaders, engineering leaders, or CEO/CTO discussing the product, culture, or technical approach.

Format each entry as:
- **Podcast Name** — "Episode Title" with Guest Name
  URL

Same rule: if nothing found, document what was searched.

---

### 8. CEO/CTO on mission (with links)

Find 2–4 specific links where the CEO or CTO speaks about the company's mission, vision, or product direction. Prioritize video interviews, keynotes, and long-form podcast episodes over short press quotes.

Format each entry as:
- **Title/Venue** — Speaker Name (Year)
  URL

---

### 9. Conclusion

A 2–3 paragraph synthesis. First paragraph: the opportunity — what makes this company worth a Staff/Principal design systems engineer's time. Second paragraph: the risks — what could go wrong. Third paragraph (optional): specific questions to ask in an interview, or next steps for due diligence.

End with a clear **fit rating** on a 5-star scale:
- ★★★★★ = Apply immediately, strong match across all dimensions
- ★★★★ = Strong match with 1–2 caveats worth probing
- ★★★ = Interesting but meaningful concerns to resolve
- ★★ = Misaligned on multiple dimensions, proceed only if specific conditions are met
- ★ = Not a fit

---

## Writing style

- Write without bullshit. No filler, no hedging, no "it's worth noting that."
- Use direct statements. Say what's true and what's not.
- Use contractions. Write like a smart colleague briefing you, not like a consultant padding a deliverable.
- Don't use "comprehensive" or "robust" or "landscape" or "leverage" or "innovative."
- Bold names and titles on first mention. Don't bold everything.
- Keep paragraphs short — 2–4 sentences.
- When data conflicts across sources, show both and say which you trust more.

## Output format

Deliver the report as a Markdown artifact. Title it: `[Company Name]: Employer Evaluation for a Staff/Principal Design Systems Engineer`
