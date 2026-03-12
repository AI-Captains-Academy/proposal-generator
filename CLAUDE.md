# Proposal Generator -- Prospect-Tailored Proposal & Pitch Deck Rig

## What This Is

A multi-agent rig that researches a prospect and generates a polished, branded proposal tailored to their specific needs. Runs in two modes:

- **Light mode**: Prospect research + one-page proposal (HTML) + 3 subject lines + cold email. Fewer tokens, produces proof-of-work for initial prospecting. Run this first to open the door.
- **Full mode**: Complete proposal package -- multi-page branded proposal (HTML), standalone pitch deck (HTML, 8-12 slides), objection-handling cheat sheet, 3-email follow-up sequence, and cover letter. Run when closing the deal.

A member provides their service/offer info, the prospect's website URL, and what they're pitching. The rig researches the prospect, structures the proposal, then generates everything needed to close.

Members use the output to close deals with the "I already did the work" motion: research the prospect, build a tailored proposal, send it cold. Works for freelancers pitching services, creators pitching brand deals/speaking, and beginners who need a professional proposal template.

## Quick Start

- `/setup` -- Configure your branding, services, and preferences (run once)
- `/voice` -- (Optional) Pre-build a voice profile from your writing samples
- `/build-proposal` -- Generate a proposal for a prospect (you'll choose light or full mode)

## The Agent Team

| Agent | Role | When to Use |
|-------|------|-------------|
| **Prospect Researcher** | Researches prospect website, social presence, pain points, gaps, recent news | Phase 3 of /build-proposal |
| **Offer Architect** | Structures proposal: problem > solution > deliverables > timeline > pricing tiers | Phase 4 of /build-proposal |
| **Front-End Designer** | Creates unique visual design brief (palette, typography, layout) for proposal HTML | Phase 4.5 of /build-proposal |
| **Proposal Writer** | Builds the proposal as polished, branded HTML (1-page for light, multi-page for full) | Phase 5 of /build-proposal |
| **Deck Builder** | Builds standalone HTML pitch deck (8-12 slides) | Phase 5 of /build-proposal (full mode only) |
| **Objection Handler** | Generates top 5 likely objections with ready responses | Phase 5 of /build-proposal (full mode only) |
| **Follow-Up Writer** | Writes 3-email follow-up sequence (Day 2, Day 5, Day 10) | Phase 5 of /build-proposal (full mode only) |
| **Cover Letter Writer** | Writes personalized cold email from member to prospect | Phase 5.5/L of /build-proposal |
| **Design Reviewer** | Reviews proposal HTML + deck HTML for layout quality, responsiveness, print quality | Phase 5.7 of /build-proposal |
| **Quality Reviewer** | **GATE**: Validates all output for quality, consistency, completeness | Phase 6 of /build-proposal (mandatory) |

## Context Engineering (CRITICAL)

### Rules for the Orchestrator (YOU)

1. **NEVER read skill files yourself.** Skills are for agents. They are at `.claude/skills/*/SKILL.md`.
2. **NEVER read agent definition files yourself.** Pass the agent file path in the Task prompt so the agent reads its own instructions. They are at `.claude/agents/*.md`.
3. **Pass file PATHS, not file CONTENTS** to agents. Tell the agent: "Read the file at `<path>`."
4. **Agents write directly to disk.** You do NOT receive generated content back.
5. **Track status, not content.** After an agent finishes, you need: (a) status, (b) file paths, (c) issues. NOT file contents.
6. **Quality Reviewer reads from disk.** Pass it the output directory path.
7. **Use max_turns on Task calls.** Prospect Researcher: 10, Offer Architect: 12, Front-End Designer: 15, Proposal Writer: 30, Deck Builder: 25, Objection Handler: 10, Follow-Up Writer: 12, Cover Letter Writer: 10, Design Reviewer: 15, Quality Reviewer: 20.

### How to Spawn Agents (Context-Safe Pattern)

CORRECT -- agent reads its own files, writes to disk:
```
Task tool:
  subagent_type: "general-purpose"
  prompt: |
    You are the [Agent Name] for the Proposal Generator.
    Read your full instructions at: .claude/agents/<agent>.md

    ## Your Task
    [Brief task description]

    ## Input Files (read these yourself)
    - Plan: output/<prospect-slug>/plan.md
    - [Other inputs]: <path>

    ## Output Files (write these yourself)
    - <path-to-output>

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

WRONG -- orchestrator reads everything and pastes it in:
```
Read .claude/agents/<agent>.md          <- wastes orchestrator context
Read .claude/skills/<skill>/SKILL.md    <- wastes orchestrator context
Task tool with all that content pasted  <- doubled context usage
```

## Workflow: /build-proposal

### Phase 1: Input & Planning (MANDATORY)
1. Read `config/member-profile.md` for member branding/services
2. Collect: prospect business name, prospect URL, what you're pitching (service/collaboration/speaking/sponsorship), any notes about the prospect
3. **Ask: "Light mode or full mode?"**
   - **Light**: One-page proposal + cold email. Faster, fewer tokens. Good for initial outreach.
   - **Full**: Complete proposal package with pitch deck, objection handling, follow-ups. Good for closing deals.
4. Create output directory: `output/<prospect-slug>/` with subdirs (research/, strategy/, proposal/, deck/, sales/)
5. Present plan to user, wait for approval
6. Save plan to `output/<prospect-slug>/plan.md` (include mode: light/full)

### Phase 2: Voice Check
7. Check for existing voice profile at `config/voices/member/voice-profile.md`. If exists, it will be used by downstream agents for tone matching.

### Phase 3: Prospect Research
8. Spawn **Prospect Researcher** (max_turns: 10)
9. Verify `research/prospect-brief.md` exists
   - If MISSING: re-spawn with max_turns: 10 and focused recovery prompt

### Phase 4: Proposal Structure
10. Spawn **Offer Architect** (max_turns: 12)
11. Verify `strategy/proposal-structure.md` exists

### Phase 4.5: Design
12. Spawn **Front-End Designer** (max_turns: 15)
13. Verify `design/visual-brief.md` exists

### Phase 5: Content Generation

- **Light mode**: Spawn Proposal Writer for one-page proposal HTML only. Proceed to Mode Branch.
- **Full mode**: Run full generation below.

**Group A (parallel)**:
14. Spawn **Proposal Writer** (max_turns: 30) -> `proposal/proposal.html`
15. Spawn **Objection Handler** (max_turns: 10) -> `strategy/objection-handling.md`

**Group B (parallel, after A)**:
16. Spawn **Deck Builder** (max_turns: 25) -> `deck/pitch-deck.html`
17. Spawn **Follow-Up Writer** (max_turns: 12) -> `sales/follow-up-sequence.md`

18. Verify all files exist

### Mode Branch (after Phase 5)
- If `mode: light` -> Phase L
- If `mode: full` -> Phase 5.5

### Phase 5.5: Cover Letter (full mode)
19. Spawn **Cover Letter Writer** (max_turns: 10) -> `sales/cover-letter.md`
20. Verify file exists

### Phase 5.7: Design Review
21. Spawn **Design Reviewer** (max_turns: 15)
22. If NEEDS FIXES: re-spawn Proposal Writer (and/or Deck Builder) with design feedback, max 1 iteration

### Phase 6: Quality Gate (MANDATORY)
23. Spawn **Quality Reviewer** (model: opus, max_turns: 20)
24. If APPROVED: proceed
25. If BLOCKED: re-spawn relevant agent(s) with QAS feedback, max 2 iterations

### Phase L: Light Output (light mode only)

Reached via Mode Branch after Phase 5.

L1. Spawn **Design Reviewer** (max_turns: 15) -- reviews proposal.html only
L2. If NEEDS FIXES: re-spawn Proposal Writer with feedback, max 1 iteration
L3. Spawn **Cover Letter Writer** (max_turns: 10) for cold email:
- Light mode differences: "I put together a proposal for [Prospect]" NOT "complete proposal package"
- Reference the attached one-page proposal
- Output to `sales/cold-email.md`

L4. Generate `output/<prospect-slug>/README.md` with light mode file list
L5. Report results, STOP

### Phase 7: Output (full mode)
26. Verify all files exist
27. Generate `output/<prospect-slug>/README.md`
28. Report results

## Quality Gate Checklist (Quality Reviewer Owns This)

### Proposal HTML
- [ ] Professional layout, clean typography, generous white space
- [ ] Problem statement specific to THIS prospect (not generic)
- [ ] Solution clearly maps to prospect's actual needs
- [ ] Deliverables are concrete with timeline
- [ ] 3-tier pricing with placeholders: [YOUR PRICE - STARTER], [YOUR PRICE - STANDARD], [YOUR PRICE - PREMIUM]
- [ ] Social proof section (placeholder format if no real testimonials)
- [ ] Next steps / CTA section present
- [ ] MEMBER branding applied (header, logo if provided, brand colors)
- [ ] Print-friendly (@media print CSS)
- [ ] No remaining {{PLACEHOLDER}} or {{ markers
- [ ] Responsive (mobile/tablet/desktop)
- [ ] Valid HTML

### Pitch Deck HTML (full mode)
- [ ] 8-12 slides, one idea per slide
- [ ] Hook > Problem > Solution > Process > Deliverables > Timeline > Pricing > Proof > Next Steps > Contact
- [ ] Pricing uses placeholders
- [ ] Standalone HTML (no framework dependencies)
- [ ] Keyboard navigation works (arrow keys)
- [ ] Clean, minimal design
- [ ] MEMBER branding applied

### Objection Handling (full mode)
- [ ] Top 5 objections specific to this prospect type and service
- [ ] Each has: the objection, why they think it, ready response, reframe
- [ ] Responses are conversational, not scripted

### Follow-Up Sequence (full mode)
- [ ] 3 emails: Day 2 (value-add), Day 5 (social proof), Day 10 (gentle close)
- [ ] Each references the proposal specifically
- [ ] Subject lines for each email
- [ ] Escalating but not desperate

### Cover Letter
- [ ] 150-250 words, 3 subject lines, single finding focus, no pricing
- [ ] Written from member's perspective (first person)
- [ ] References specific prospect details from research
- [ ] Mentions the proposal that was built
- [ ] Soft CTA with member contact info

### Cross-Deliverable Consistency
- [ ] Service description matches across proposal, deck, cover letter, follow-ups
- [ ] Pricing placeholders consistent everywhere
- [ ] Prospect name spelled consistently
- [ ] Tone consistent (professional/confident or matches voice profile)

## Output Structure

```
output/<prospect-slug>/
  README.md                       # Table of contents + delivery checklist
  plan.md                         # Approved proposal plan
  research/
    prospect-brief.md             # Prospect analysis
  strategy/
    proposal-structure.md         # Proposal architecture
    objection-handling.md         # Top 5 objections + responses (full mode)
  design/
    visual-brief.md               # Visual design brief for HTML
  proposal/
    proposal.html                 # Branded proposal (1-page light, multi-page full)
  deck/
    pitch-deck.html               # Standalone HTML pitch deck (full mode)
  sales/
    cover-letter.md               # Personalized cold email (full mode)
    cold-email.md                 # Cold outreach email (light mode)
    follow-up-sequence.md         # 3-email follow-up (full mode)
```

### Light Mode Output
```
output/<prospect-slug>/
  plan.md
  research/
    prospect-brief.md
  strategy/
    proposal-structure.md
  design/
    visual-brief.md
  proposal/
    proposal.html                 # One-page branded proposal
  sales/
    cold-email.md                 # Cold outreach + 3 subject lines
```

## Skills (Domain Knowledge)

| Skill | Location | Used By |
|-------|----------|---------|
| Prospect Research | `.claude/skills/prospect-research/SKILL.md` | Prospect Researcher |
| Proposal Design | `.claude/skills/proposal-design/SKILL.md` | Front-End Designer, Proposal Writer |
| Pitch Deck Design | `.claude/skills/pitch-deck-design/SKILL.md` | Front-End Designer, Deck Builder |
| Objection Handling | `.claude/skills/objection-handling/SKILL.md` | Objection Handler |
| QAS Checklist | `.claude/skills/qas-checklist/SKILL.md` | Quality Reviewer (ONLY skill the QAS reads) |

## File Reference

| What | Where |
|------|-------|
| This file (orchestration hub) | `CLAUDE.md` |
| Settings | `.claude/settings.local.json` |
| Commands | `.claude/commands/build-proposal.md`, `setup.md`, `voice.md` |
| Agents | `.claude/agents/prospect-researcher.md`, `offer-architect.md`, `front-end-designer.md`, `proposal-writer.md`, `deck-builder.md`, `objection-handler.md`, `follow-up-writer.md`, `cover-letter-writer.md`, `design-reviewer.md`, `quality-reviewer.md` |
| Skills | `.claude/skills/prospect-research/`, `proposal-design/`, `pitch-deck-design/`, `objection-handling/`, `qas-checklist/` |
| Member config | `config/member-profile.md` |
| Saved voice profiles | `config/voices/member/voice-profile.md` |
| Output | `output/<prospect-slug>/` |
