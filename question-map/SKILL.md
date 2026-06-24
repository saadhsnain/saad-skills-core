---
name: question-map
description: Design or audit a landing page by mapping every section to the single visitor question it must answer. Three modes — Design (build a question spine before building), Audit (section-by-section question table with gap flags), Panel (6 judges score the page from their angle). Works directly on code being built in Claude — reads JSX/TSX components or fetches a URL. No wireframes required.
---

# Question Map

Every section on a landing page has one job: resolve the question a visitor is already holding in their head. If a section answers nothing, it is invisible — users scan past it until they find resolution or leave.

This skill formalizes that intuition into a repeatable process. It works directly on live code or URLs — no wireframes needed.

The theoretical grounding: **question-led messaging hierarchy** (Joanna Wiebe / Copy Hackers), **objection-based page design** (Pedro Cortés, CXL), and **cognitive walkthrough** adapted from usability. The six-judge panel adds angles the question-map table can't catch: emotional resonance, competitive positioning, word-level craft, financial credibility, and aesthetic intentionality.

---

## Usage

```
/question-map                          ← Audit mode, paste sections or URL when prompted
/question-map design                   ← Design mode, build the question spine before building
/question-map audit <url or paste>     ← Audit: section table + gap flags
/question-map panel                    ← Full judge panel (5 perspectives + scores)
/question-map full                     ← Audit table + full judge panel together
```

---

## Reading the Page (all modes except Design)

Before any analysis, extract the visitor-facing content. Do this before asking the user for anything.

**If a URL is provided:**
Use WebFetch to retrieve the page. Extract: H1, all H2s, subheads, body copy per section, CTAs, testimonials, FAQ questions, and section order.

**If the user is building a page in this Claude session:**
Read the relevant component files directly. Look for:
- Section components (Hero, About, Services, Testimonials, FAQ, CTA, etc.)
- The text content rendered to the visitor — headlines, subheads, body copy, button labels
- Not just the component names — the actual visible strings

**If code or HTML is pasted:**
Extract the same: H1 → H2s → key claims → CTAs → testimonials in order.

**Then confirm back:**
> "I'm reading [N] sections: [list them]. Proceeding with [audience type] as the primary visitor — correct this if wrong."

Don't ask the user to describe their own page if you can read it directly.

---

## Mode 1 — Design

Use before building. You start with audience and offer; the skill walks you through building a question spine — the ordered list of questions the page must answer — before any code is written.

### Step 1 — Establish audience and offer

Ask (or infer from context):
1. **Who is the primary visitor?** Cold traffic, warm referral, or high-intent searcher?
2. **What's the offer?** Service, SaaS, product, event, newsletter?
3. **What's the risk level?** Low (free trial, £35 workshop) vs high (multi-month engagement, enterprise contract)?
4. **What brought them here?** Ad, organic search, referral link, social bio?

| Visitor type | First question they're holding | CTA timing |
|---|---|---|
| Cold traffic | "What even is this — is it for me?" | Late — after problem + solution + trust |
| Warm referral | "Okay I've heard of this — is it worth it?" | Mid — after social proof |
| High-intent searcher | "Can this solve my specific thing?" | Early — once fit is confirmed |
| Return visitor | "What's the process / price?" | Very early — they already trust |

### Step 2 — Build the question spine

Output the question spine as a section brief:

| # | Section name | Question it answers | Q-type | Evidence / content to resolve it | Notes |
|---|---|---|---|---|---|

Use the canonical sequence below as a starting template. Adjust for audience and offer complexity.

**Canonical question sequence (cold/medium-awareness visitors):**

| Position | Question | Type | Notes |
|---|---|---|---|
| 1 | "Can this person / brand actually help me?" | clarity | Hero. 3-second test. Personality and specificity are the filter. |
| 2 | "Do you understand my actual problem?" | problem | Visitor vocabulary, not company jargon. Names the pain precisely. |
| 3 | "What makes you different — why you specifically?" | differentiator | Ideological or methodological, not a feature list. |
| 4 | "Who is behind this — who am I actually dealing with?" | credibility | Origin story. Positions the maker as shaped by the same world as the reader. |
| 5 | "Do people like me actually get results?" | proof | Testimonials tied to specific prior claims, not generic praise. |
| 6 | "What exactly do you offer and how do I get it?" | offer | Clear service ladder. Three-tier is common: entry → core → bespoke. |
| 7 | "How can I try this without a big commitment?" | entry | Workshop, newsletter, audit, free resource. Lowers stakes before main CTA. |
| 8 | "Show me the work — what does this look like in practice?" | portfolio | Case studies as "before / after" — not deliverable lists. |
| 9 | "What about cost, timing, process — the things I'm nervous about?" | objection | FAQ that addresses real hesitations, not generic product info. |
| 10 | "What happens if I click?" | cta | CTA = call to value. Explain what actually happens next. |

Design mode output: the question spine table becomes the brief before any code or copy is written.

---

## Mode 2 — Audit

Use on an existing page or draft. Maps each section to its question, flags gaps, checks sequence.

### Step 1 — Read the page

Follow the "Reading the Page" instructions above. Confirm the section list before proceeding.

### Step 2 — Establish context

Confirm or infer:
- Primary visitor type (cold / warm / high-intent)
- Offer and risk level
- Where traffic is coming from

### Step 3 — Fill the audit table

One row per section (screenful, not component):

| # | Section | Implied visitor question | Q-type | Answer present? | Leak risk | Gap / note |
|---|---|---|---|---|---|---|

**Q-types:** `clarity` · `fit` · `problem` · `differentiator` · `credibility` · `proof` · `offer` · `entry` · `objection` · `cta`

**Answer present?** `Yes` / `Partial` / `No` / `Wrong question answered`

**Leak risk:** `High` · `Med` · `Low`

### Step 4 — Call out gap types explicitly

- **Dead section** — no identifiable question answered; remove or merge
- **Orphaned question** — a question the visitor definitely holds but no section addresses
- **Sequence break** — a question appears before its prerequisite (e.g., pricing before trust is established)
- **Double-load** — one section tries to answer two unrelated questions; split it
- **Vocabulary mismatch** — answered in company language, not visitor language

### Step 5 — Sequence check by audience type

**Cold traffic:** Does the page establish "what is this + is it for me" before any commitment ask?
**Warm referrals:** Does the hero skip basics and lead with differentiation or proof?
**Skeptics / high-risk offers:** Is there a trust section before the main CTA that specifically names their fears?
**Universal:** Is there at least one low-barrier entry point before the main ask?

### Step 6 — Cognitive walkthrough for flagged sections

For any section marked Partial, No, or Wrong:
1. **Recognition** — Will the visitor recognize the question they're holding here, or does the section open with company-centric framing?
2. **Visibility** — Is the answer in the primary visual hierarchy (H1/H2, image) or buried in body copy?
3. **Connection** — Does the copy use the visitor's words or internal vocabulary?
4. **Progress** — Does the visitor feel closer to their goal after this section?

### Step 7 — Audit output

1. The audit table
2. Gap list (all gap types called out)
3. Top 3 fixes ranked by leak risk
4. Sequence recommendation if sections are misordered
5. One vocabulary note — the most common place visitor language is replaced by company language

---

## Mode 3 — Panel

Six judges, six angles. Each reads the same page and scores it from their perspective.

Run this after extracting the page content. Each judge is opinionated, distinct, and looking for something different. Do not blend them into consensus — the value is in the friction between them.

---

### Judge 1: The Skeptical CFO

**Profile:** Has approved budgets and rejected pitches for 15 years. Reads landing pages like due diligence. Assumes everything is marketing until proven otherwise. Time-poor, bullshit-sensitive, looking for the one claim that doesn't hold up.

**What they're scoring:**
- Are the claims substantiated with specific numbers, not adjectives?
- Is the social proof credible and verifiable (not just logos and first names)?
- Are trust signals (registration, track record, credentials) visible near the CTA — or buried?
- Is the risk of buying from you clearly addressed?
- Is there a legible ROI argument somewhere on the page?

**What kills their score:**
Unsubstantiated superlatives ("largest," "best," "world-class"), generic testimonials with no company or outcome, missing process clarity, no answer to "what happens if this doesn't work."

**Output format:**
> **CFO — [score]/10**
> *First impression:* [what they noticed in 3 seconds]
> *What almost lost them:* [one specific thing]
> *What kept them:* [one specific thing, or "nothing"]
> *One ask:* [the single change that would move their score by 2+ points]

---

### Judge 2: The Founder at Midnight

**Profile:** Running their own thing. Scrolling not to buy but to feel something — validation, curiosity, a sense that someone else gets it. If the page feels human and specific, they'll lean in. If it feels like marketing, they close the tab. They've seen a thousand agency sites. They know the smell.

**What they're scoring:**
- Does the problem framing feel real and personal — or could it describe anyone?
- Does the maker/brand come through as a person, or does it read like a committee?
- Is there genuine conviction somewhere on the page, or just positioning?
- Does the copy feel like it was written *for* them, or *at* them?

**What kills their score:**
Corporate language, generic pain points that apply to every business, no personality in the voice, "we help businesses grow" type filler, a page that could've been written by an AI with no brief.

**Output format:**
> **Founder at Midnight — [score]/10**
> *First impression:* [gut reaction, honest]
> *What almost lost them:* [specific moment they checked out]
> *What kept them:* [specific moment they leaned back in, or "nothing"]
> *One ask:* [what would make this feel human]

---

### Judge 3: The Biggest Competitor

**Profile:** Knows the market cold. Looking for positioning gaps to exploit, vague claims to counter-position against, and proof holes to fill with their own credentials. Also sharp enough to notice what they *can't* copy — and quietly respect it.

**What they're scoring:**
- How defensible is the positioning? Could they claim the same thing tomorrow?
- What's left unsaid that they'd say instead?
- What's genuinely hard to replicate (specific proof, authentic voice, proprietary approach)?
- What claim on this page is most vulnerable to being undercut?

**Output format:**
> **Competitor — [score]/10** *(lower = easier for them to beat you)*
> *What they'd exploit:* [the positioning gap or proof hole they'd attack]
> *What they can't copy:* [the one thing that's genuinely defensible]
> *Their counter-move:* [what they'd put on their own page in response]
> *One ask:* [what would make this page much harder to compete against]

---

### Judge 4: The Ideal Customer

**Profile:** The person the page was built for. Arrived with a real question. Scanning to see if they feel seen. Not evaluating the marketing — evaluating whether this is actually for them. Leaves when the examples don't match their situation or the vocabulary is slightly off.

**Characterize this judge from the brief:** Before scoring, state who the ideal customer is for this specific page (role, context, what they're trying to solve) — either inferred from the page content or asked upfront.

**What they're scoring:**
- Is the vocabulary theirs — or is it the brand's?
- Do the examples, case studies, and testimonials reflect their situation?
- Does the objection handling address their actual hesitations?
- At what point (if any) did they feel: "yes, this is for me"?

**Output format:**
> **Ideal Customer — [score]/10**
> *First impression:* [did the hero feel written for them?]
> *Where they felt seen:* [specific section or line, or "nowhere"]
> *Where they felt like an afterthought:* [specific section or vocabulary mismatch]
> *One ask:* [what would make them feel like this page was made specifically for them]

---

### Judge 5: The Conversion Copywriter

**Profile:** Harry Dry's framework applied at word level. Every sentence passes three questions: can I visualize it, can I falsify it, can only you say it? Has zero patience for adjective soup or claims a competitor can sign. Will show a rewrite.

**Three tests applied to every key line:**
1. **Visualize** — Close your eyes. Can you see it? "Innovative solution" → no. "Reads on the tube" → yes.
2. **Falsify** — Is this provably true or false? "Great quality" → no. "Holds 50kg" → yes. Facts guarantee you're saying something.
3. **Only you** — Could a competitor sign this? "We help businesses grow" → any company can say it. "Worn by supermodels in London and dads in Ohio" → only New Balance.

**What they're scoring:**
- H1: does it pass all three tests?
- Top subheads and key claims: same
- Testimonials: specific outcomes, or generic praise?
- CTA copy: call to value or call to action?
- Overall: how much of the copy is "balloon smoke" (word-shaped air that means nothing)?

**What kills their score:**
"Seamless," "innovative," "powerful," "effortless," "world-class," "game-changing." Any claim the same product from a competitor could make. Beautiful words that say nothing.

**Output format:**
> **Copywriter — [score]/10**
> *H1 verdict:* [pass/fail on the three tests, one-line explanation]
> *Worst offender:* [the specific line that fails hardest — quote it]
> *Best line on the page:* [the one that passes all three — quote it]
> *Rewrite:* [one rewrite of the worst offender, passing all three tests]
> *One ask:* [the single copy change that would move the score most]

---

### Judge 6: The Tasteful Artist

**Profile:** Has spent years building the internalized standard that lets them spot the statistical mean at a glance. Not scoring whether the page is pretty — scoring whether a human with a felt opinion was in the room. Allergic to "ambient design": the safe, palatable-to-everyone layout that AI defaults to when no one pushes back on it.

**What they're scoring:**
- Does the page look like *this specific brand*, or could the logo be swapped for any competitor's with zero rework?
- Is there one deliberate, slightly uncomfortable choice (a color, a crop, a rhythm break) — or does everything sit at the safe default?
- Do spacing, hierarchy, and type rhythm show fundamentals were practiced, or just inherited from a template?
- Does the page push against something (a dated convention, an over-saturated trend in the category) or just repeat what's already everywhere?

**What kills their score:**
Centered hero + gradient blob + rounded card grid with no variation — the default AI/template look. Stock photography that could illustrate any company. Decisions that feel chosen by consensus rather than felt by someone.

**Output format:**
> **Tasteful Artist — [score]/10**
> *First impression:* [does this look like a specific choice or the average of every SaaS site this year?]
> *What feels templated:* [the most "ambient design" moment on the page]
> *What feels intended:* [the one choice that feels like a human decided it, or "nothing"]
> *One ask:* [the single change that would make the page unmistakably theirs]

---

### Panel Output

After all five judges:

**Scores at a glance:**

| Judge | Score | Biggest concern |
|---|---|---|
| Skeptical CFO | /10 | |
| Founder at Midnight | /10 | |
| Biggest Competitor | /10 | |
| Ideal Customer | /10 | |
| Conversion Copywriter | /10 | |
| Tasteful Artist | /10 | |
| **Panel average** | **/10** | |

**The one fix that moves the most scores:**
Identify the single change that would improve the largest number of judges simultaneously. State it as a concrete action, not a principle.

**Priority order:**
Rank the top 3 fixes, noting which judges each fix addresses.

---

## Mode 4 — Full

Runs Audit (question table + gap flags) then Panel (all six judges) on the same page. Use when you want the complete picture — structural gaps AND perspective-specific feedback — before pushing changes.

Output order: page content summary → audit table → gap list → top fixes from audit → panel scores → panel priority order.

---

## Key failure modes (what designers most often miss)

- **Questions framed from the company's perspective** — "Our approach" instead of "How does this actually work?" Technically exists, doesn't register.
- **Sequence mismatched to traffic source** — Hero doesn't match the ad/email that brought the visitor. Cold traffic lands on brand-first hero with no problem acknowledgement and leaves.
- **Trust signals buried** — Credibility proofs (ratings, registration, track record) sitting mid-page or in FAQ instead of near the CTA where they're needed.
- **Sections that answer three questions** — Visually noisy, cognitively tiring. One screenful, one question.
- **No sense of progress** — Visitors need to feel they're moving toward resolution. Narrative continuity between sections signals momentum.
- **FAQ as info dump** — Should mop up objections raised earlier, not answer random product questions.
- **Social proof as decoration** — Testimonials placed generically instead of directly after the claim they validate.

---

## Cross-domain anchors (use when reframing stuck sections)

**Socratic structure** — Don't just answer the question; surface the hidden assumption behind it. "If you're thinking X, here's why that framing breaks down" is more persuasive than answering the surface question.

**IRAC (legal argument)** — Issue → Rule → Application → Conclusion. For complex or high-trust offers: name the problem precisely, establish the principle that makes your approach necessary, show it applied, state the conclusion. Gives skeptical visitors a complete argument, not just a claim.

**Clinical intake** — Don't push CTAs before the visitor feels seen. A "Which of these describes you?" segmentation moment lets visitors self-select into the right narrative path and signals the page was built for them specifically.

---

## Quality gates

- Every section must map to exactly one primary question
- No section in the audit table without a Q-type assigned
- Orphaned questions must be explicitly called out — not just implied by missing sections
- Vocabulary check: at least one section should be reviewed against actual visitor language (reviews, support tickets, call transcripts)
- Panel mode: each judge must reach a different conclusion at least once — if all six agree on everything, the review is too generic
- Design mode: do not proceed to building until every position in the question spine has a section owner
