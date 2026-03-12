# QAS Checklist — Proposal Generator

Consolidated validation criteria for the Quality Reviewer agent. This is the ONLY skill file the QAS needs to read.

## Reading Strategy

### Priority Order
1. **HTML files first** (highest risk of visible errors):
   - `proposal/proposal.html`
   - `deck/pitch-deck.html`
2. **Sales files** (prospect-facing):
   - `sales/cover-letter.md` or `sales/cold-email.md`
   - `sales/follow-up-sequence.md`
3. **Strategy files** (internal quality):
   - `strategy/proposal-structure.md`
   - `strategy/objection-handling.md`
4. **Reference files** (cross-check):
   - `research/prospect-brief.md`
   - `design/visual-brief.md`
   - `config/member-profile.md`

### Grep-First Checks (run before deep reading)
```
Grep for "{{" in proposal/    -> catch template placeholders
Grep for "PLACEHOLDER" in .   -> catch unmarked placeholders
Grep for "YOUR PRICE" in .    -> verify pricing placeholders present
Grep for "\$[0-9]" in .       -> CRITICAL: catch real dollar amounts
Grep for "Lorem" in .         -> catch dummy text
```

## Proposal HTML Checklist

### Content Quality
- [ ] Problem statement references specific details from prospect brief (not generic)
- [ ] Solution maps to prospect's actual needs
- [ ] Deliverables are concrete and specific to this prospect
- [ ] Timeline has realistic phases and milestones
- [ ] Executive summary (full mode) hooks with a prospect-specific insight

### Pricing (CRITICAL)
- [ ] 3 tiers present: Starter, Standard, Premium
- [ ] EXACT placeholders used: `[YOUR PRICE - STARTER]`, `[YOUR PRICE - STANDARD]`, `[YOUR PRICE - PREMIUM]`
- [ ] NO real dollar amounts anywhere in the document
- [ ] Standard tier visually highlighted
- [ ] Each tier has: name, price placeholder, included items, timeline, "best for"

### Branding
- [ ] MEMBER brand applied (member's colors, business name, contact info)
- [ ] Member logo referenced (if provided in member profile)
- [ ] Header shows member business name
- [ ] Footer shows member contact info
- [ ] No prospect branding in the design

### HTML Quality
- [ ] No remaining `{{PLACEHOLDER}}` or `{{` markers
- [ ] No `Lorem ipsum` or dummy text
- [ ] Valid HTML structure (proper nesting, closing tags)
- [ ] Google Fonts loaded correctly
- [ ] Colors match visual design brief
- [ ] Typography matches brief

### Print Quality
- [ ] @media print styles present
- [ ] Page breaks between major sections
- [ ] Clean formatting when printed
- [ ] Pricing table prints cleanly
- [ ] No backgrounds that waste ink (or transparent in print)

### Responsiveness
- [ ] Readable at 375px mobile width
- [ ] Pricing cards stack on mobile
- [ ] No horizontal scrolling

## Pitch Deck HTML Checklist (full mode)

- [ ] 8-12 slides total
- [ ] One idea per slide
- [ ] Narrative flow: Hook > Problem > Solution > Process > Deliverables > Timeline > Pricing > Proof > Next Steps
- [ ] Pricing uses exact [YOUR PRICE - X] placeholders
- [ ] NO real dollar amounts
- [ ] Keyboard navigation works (arrow keys)
- [ ] Slide counter present and accurate
- [ ] Text large enough (48px+ titles, 24px+ body)
- [ ] Member branding consistent across slides
- [ ] Prospect name/details on relevant slides

## Objection Handling Checklist (full mode)

- [ ] 5 objections present
- [ ] Each has: objection text, "why they think this", response, reframe
- [ ] Objections are specific to this prospect type (not generic)
- [ ] Responses are conversational (not scripted)
- [ ] Quick reference "If They Say → You Say" table present
- [ ] No pricing mentioned in responses

## Follow-Up Sequence Checklist (full mode)

- [ ] 3 emails: Day 2, Day 5, Day 10
- [ ] Each has subject line
- [ ] Each is 100-150 words
- [ ] Day 2: Value-add (gives something useful, no ask)
- [ ] Day 5: Social proof (case study, result, or relevant insight)
- [ ] Day 10: Gentle close (low-friction next step, no false urgency)
- [ ] Each references the proposal/prospect specifically
- [ ] Tone escalates appropriately (not desperate)

## Cover Letter / Cold Email Checklist

- [ ] 150-250 words
- [ ] 3 subject line options (proof hook, gap hook, curiosity hook)
- [ ] First person from member's perspective
- [ ] References specific prospect details from research
- [ ] Mentions the proposal
- [ ] Single gap focus (not a summary of everything)
- [ ] No pricing mentioned
- [ ] Soft CTA with member contact info
- [ ] No corporate filler phrases
- [ ] Member name and business in signature

## Cross-Deliverable Consistency

- [ ] Service/offer description matches across all files
- [ ] Pricing placeholders identical everywhere ([YOUR PRICE - STARTER/STANDARD/PREMIUM])
- [ ] Prospect business name spelled consistently
- [ ] Member name and business spelled consistently
- [ ] Tone consistent across proposal, deck, cover letter, follow-ups
- [ ] No contradictions between deliverables
- [ ] Deliverables listed in proposal match those in deck and proposal structure

## Severity Guide

### CRITICAL (must block)
- Real dollar amounts in pricing
- Wrong prospect name
- Broken HTML (unclosed tags, missing sections)
- Missing required sections (pricing, CTA)
- Generic content that could apply to any prospect
- Wrong branding (prospect brand on proposal, or agency brand contamination)

### IMPORTANT (should fix, may block if multiple)
- Voice inconsistency across deliverables
- Weak or vague problem statement
- Missing print styles
- Social proof section empty with no placeholder guidance
- Follow-up emails that are generic

### MINOR (note but don't block)
- Stylistic preferences
- Minor spacing inconsistencies
- Optional section ordering differences
