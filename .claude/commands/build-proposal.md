# /build-proposal -- Generate a Prospect-Tailored Proposal

Research a prospect and generate a polished, branded proposal. Choose light mode (one-page proposal + cold email for prospecting) or full mode (complete proposal package with pitch deck, objection handling, and follow-ups).

## Input: $ARGUMENTS

If no arguments provided, ask the user for:
1. **Prospect business name**: The prospect's business or organization name
2. **Prospect URL**: Their website URL
3. **What you're pitching**: The service, collaboration, or opportunity you're proposing
4. **Pitch type**: Service proposal / brand sponsorship / speaking engagement / collaboration
5. **Mode**: Light or Full?
   - **Light**: One-page proposal + cold email (~5 min). Good for initial outreach.
   - **Full**: Complete proposal package with deck, objections, follow-ups (~12-18 min). Good for closing deals.
6. **Notes**: Any additional context about the prospect or the opportunity (optional)

## Context Rules (MANDATORY)

Follow the Context Engineering rules in CLAUDE.md:
- Do NOT read agent or skill files yourself
- Pass file PATHS to agents, not contents
- Agents write to disk directly
- You track status and file paths only

## Pipeline

### Phase 1: Input & Planning

1. Read `config/member-profile.md` for member branding, services, and social proof
2. Collect prospect details (from $ARGUMENTS or by asking)
3. Create a URL-safe prospect slug from the business name (lowercase, hyphens, no special chars)
4. Create output directory structure:
   ```
   output/<prospect-slug>/
     research/
     strategy/
     design/
     proposal/
     deck/
     sales/
   ```
5. Present a brief plan to the user:
   - Prospect: [name]
   - URL: [url]
   - Pitching: [what]
   - Type: [pitch type]
   - Mode: [light/full]
6. Wait for user approval before proceeding
7. Save the approved plan to `output/<prospect-slug>/plan.md` with all details (include mode: light/full)

### Phase 2: Voice Check

8. Check for existing voice profile at `config/voices/member/voice-profile.md`. Note in plan whether it exists (downstream agents will use it if available).

### Phase 3: Prospect Research

9. Spawn **Prospect Researcher**:

```
Task tool:
  description: "Research prospect business"
  subagent_type: "general-purpose"
  max_turns: 10
  prompt: |
    You are the Prospect Researcher for the Proposal Generator.
    Read your full instructions at: .claude/agents/prospect-researcher.md

    ## Your Task
    Research the prospect business and produce a comprehensive prospect brief.

    ## Input Files (read these yourself)
    - Plan: output/<prospect-slug>/plan.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/research/prospect-brief.md

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

10. Verify `research/prospect-brief.md` exists.
    - If MISSING: re-spawn with max_turns: 10 and focused recovery prompt.

### Phase 4: Proposal Structure

11. Spawn **Offer Architect**:

```
Task tool:
  description: "Structure proposal"
  subagent_type: "general-purpose"
  max_turns: 12
  prompt: |
    You are the Offer Architect for the Proposal Generator.
    Read your full instructions at: .claude/agents/offer-architect.md

    ## Your Task
    Design the proposal structure based on prospect research and member services.

    ## Input Files (read these yourself)
    - Plan: output/<prospect-slug>/plan.md
    - Prospect Brief: output/<prospect-slug>/research/prospect-brief.md
    - Member Profile: config/member-profile.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/strategy/proposal-structure.md

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

12. Verify `strategy/proposal-structure.md` exists.

### Phase 4.5: Design

13. Spawn **Front-End Designer**:

```
Task tool:
  description: "Design proposal visual brief"
  subagent_type: "general-purpose"
  max_turns: 15
  prompt: |
    You are the Front-End Designer for the Proposal Generator.
    Read your full instructions at: .claude/agents/front-end-designer.md

    ## Your Task
    Create a visual design brief for the proposal HTML (and pitch deck if full mode).

    ## Input Files (read these yourself)
    - Plan: output/<prospect-slug>/plan.md
    - Proposal Structure: output/<prospect-slug>/strategy/proposal-structure.md
    - Member Profile: config/member-profile.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/design/visual-brief.md

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

14. Verify `design/visual-brief.md` exists.

### Phase 5: Content Generation

**Light mode**: Spawn only the Proposal Writer for a one-page proposal. Skip Deck Builder, Objection Handler, Follow-Up Writer. Proceed to Mode Branch.

Light mode Proposal Writer spawn:
```
Task tool:
  description: "Write one-page proposal"
  subagent_type: "general-purpose"
  max_turns: 25
  prompt: |
    You are the Proposal Writer for the Proposal Generator.
    Read your full instructions at: .claude/agents/proposal-writer.md

    ## Your Task
    Write a ONE-PAGE proposal (HTML) for light mode. Keep it concise:
    - Brief problem/opportunity statement
    - Proposed solution (2-3 sentences)
    - Key deliverables (bullet list)
    - Pricing placeholders (3-tier)
    - Next steps CTA

    ## Input Files (read these yourself)
    - Plan: output/<prospect-slug>/plan.md
    - Prospect Brief: output/<prospect-slug>/research/prospect-brief.md
    - Proposal Structure: output/<prospect-slug>/strategy/proposal-structure.md
    - Visual Design Brief: output/<prospect-slug>/design/visual-brief.md
    - Member Profile: config/member-profile.md
    - Voice Profile (if exists): config/voices/member/voice-profile.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/proposal/proposal.html

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

**Full mode**: Run full generation.

**Group A (parallel)**:

15. Spawn **Proposal Writer** (max_turns: 30) -> `proposal/proposal.html`
16. Spawn **Objection Handler** (max_turns: 10) -> `strategy/objection-handling.md`

Proposal Writer (full mode):
```
Task tool:
  description: "Write full proposal"
  subagent_type: "general-purpose"
  max_turns: 30
  prompt: |
    You are the Proposal Writer for the Proposal Generator.
    Read your full instructions at: .claude/agents/proposal-writer.md

    ## Your Task
    Write a complete multi-page proposal (HTML) for full mode.

    ## Input Files (read these yourself)
    - Plan: output/<prospect-slug>/plan.md
    - Prospect Brief: output/<prospect-slug>/research/prospect-brief.md
    - Proposal Structure: output/<prospect-slug>/strategy/proposal-structure.md
    - Visual Design Brief: output/<prospect-slug>/design/visual-brief.md
    - Member Profile: config/member-profile.md
    - Voice Profile (if exists): config/voices/member/voice-profile.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/proposal/proposal.html

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

Objection Handler:
```
Task tool:
  description: "Generate objection responses"
  subagent_type: "general-purpose"
  max_turns: 10
  prompt: |
    You are the Objection Handler for the Proposal Generator.
    Read your full instructions at: .claude/agents/objection-handler.md

    ## Your Task
    Generate the top 5 likely objections and ready-to-use responses.

    ## Input Files (read these yourself)
    - Plan: output/<prospect-slug>/plan.md
    - Prospect Brief: output/<prospect-slug>/research/prospect-brief.md
    - Proposal Structure: output/<prospect-slug>/strategy/proposal-structure.md
    - Member Profile: config/member-profile.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/strategy/objection-handling.md

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

17. Verify Group A files exist.

**Group B (parallel, after A)**:

18. Spawn **Deck Builder** (max_turns: 25) -> `deck/pitch-deck.html`
19. Spawn **Follow-Up Writer** (max_turns: 12) -> `sales/follow-up-sequence.md`

Deck Builder:
```
Task tool:
  description: "Build pitch deck"
  subagent_type: "general-purpose"
  max_turns: 25
  prompt: |
    You are the Deck Builder for the Proposal Generator.
    Read your full instructions at: .claude/agents/deck-builder.md

    ## Your Task
    Build a standalone HTML pitch deck (8-12 slides).

    ## Input Files (read these yourself)
    - Plan: output/<prospect-slug>/plan.md
    - Prospect Brief: output/<prospect-slug>/research/prospect-brief.md
    - Proposal Structure: output/<prospect-slug>/strategy/proposal-structure.md
    - Visual Design Brief: output/<prospect-slug>/design/visual-brief.md
    - Member Profile: config/member-profile.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/deck/pitch-deck.html

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

Follow-Up Writer:
```
Task tool:
  description: "Write follow-up sequence"
  subagent_type: "general-purpose"
  max_turns: 12
  prompt: |
    You are the Follow-Up Writer for the Proposal Generator.
    Read your full instructions at: .claude/agents/follow-up-writer.md

    ## Your Task
    Write a 3-email follow-up sequence for after the proposal is sent.

    ## Input Files (read these yourself)
    - Plan: output/<prospect-slug>/plan.md
    - Prospect Brief: output/<prospect-slug>/research/prospect-brief.md
    - Proposal Structure: output/<prospect-slug>/strategy/proposal-structure.md
    - Member Profile: config/member-profile.md
    - Voice Profile (if exists): config/voices/member/voice-profile.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/sales/follow-up-sequence.md

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

20. Verify Group B files exist.

### Mode Branch (after Phase 5)
- If `mode: light` -> Phase L
- If `mode: full` -> Phase 5.5

### Phase 5.5: Cover Letter (full mode)

21. Spawn **Cover Letter Writer**:

```
Task tool:
  description: "Write sales cover letter"
  subagent_type: "general-purpose"
  max_turns: 10
  prompt: |
    You are the Cover Letter Writer for the Proposal Generator.
    Read your full instructions at: .claude/agents/cover-letter-writer.md

    ## Your Task
    Write a personalized cold email from the member to the prospect, introducing the proposal.

    ## Input Files (read these yourself)
    - Member Profile: config/member-profile.md
    - Plan: output/<prospect-slug>/plan.md
    - Prospect Brief: output/<prospect-slug>/research/prospect-brief.md
    - Proposal Structure: output/<prospect-slug>/strategy/proposal-structure.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/sales/cover-letter.md

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

22. Verify `sales/cover-letter.md` exists.

### Phase 5.7: Design Review

23. Spawn **Design Reviewer**:

```
Task tool:
  description: "Review HTML design quality"
  subagent_type: "general-purpose"
  max_turns: 15
  prompt: |
    You are the Design Reviewer for the Proposal Generator.
    Read your full instructions at: .claude/agents/design-reviewer.md

    ## Your Task
    Review all HTML deliverables for visual quality, layout, design brief adherence, responsiveness, and print quality.

    ## Input Files (read these yourself)
    - Visual Design Brief: output/<prospect-slug>/design/visual-brief.md
    - Proposal: output/<prospect-slug>/proposal/proposal.html
    - Pitch Deck (if exists): output/<prospect-slug>/deck/pitch-deck.html
    - Member Profile: config/member-profile.md

    ## Return Format
    Return:
    ## DESIGN REVIEW: APPROVED (or NEEDS FIXES)
    [If NEEDS FIXES: list specific issues with file paths and CSS/HTML fixes needed]
    [If APPROVED: one-line summary per file]
```

24. If **APPROVED**: proceed to Phase 6.
25. If **NEEDS FIXES**: Re-spawn the relevant builder agent(s) with design feedback. Max 1 design iteration.

### Phase L: Light Output (light mode only)

Reached via Mode Branch after Phase 5.

L1. Spawn **Design Reviewer** (max_turns: 15) -- reviews proposal.html only:
```
Task tool:
  description: "Review proposal design"
  subagent_type: "general-purpose"
  max_turns: 15
  prompt: |
    You are the Design Reviewer for the Proposal Generator.
    Read your full instructions at: .claude/agents/design-reviewer.md

    ## Your Task
    Review the one-page proposal HTML for visual quality, layout, responsiveness, and print quality.

    ## Input Files (read these yourself)
    - Visual Design Brief: output/<prospect-slug>/design/visual-brief.md
    - Proposal: output/<prospect-slug>/proposal/proposal.html
    - Member Profile: config/member-profile.md

    ## Return Format
    Return:
    ## DESIGN REVIEW: APPROVED (or NEEDS FIXES)
    [If NEEDS FIXES: list specific issues with CSS/HTML fixes needed]
    [If APPROVED: one-line summary]
```

L2. If NEEDS FIXES: re-spawn Proposal Writer with feedback, max 1 iteration.

L3. Spawn **Cover Letter Writer** (max_turns: 10) for cold email:
```
Task tool:
  description: "Write cold outreach email"
  subagent_type: "general-purpose"
  max_turns: 10
  prompt: |
    You are the Cover Letter Writer for the Proposal Generator (light mode).
    Read your full instructions at: .claude/agents/cover-letter-writer.md

    ## Your Task
    Write a cold outreach email (150-250 words) with 3 subject lines.

    ## IMPORTANT: LIGHT MODE DIFFERENCES
    - Say "I put together a quick proposal for [Prospect Name]" NOT "I built a complete proposal package"
    - Reference the attached one-page proposal
    - CTA: "I put this together specifically for your business -- worth a quick look?"
    - Do NOT mention pitch deck, follow-up sequence, or objection handling

    ## Input Files (read these yourself)
    - Member Profile: config/member-profile.md
    - Plan: output/<prospect-slug>/plan.md
    - Prospect Brief: output/<prospect-slug>/research/prospect-brief.md

    ## Output Files (write these yourself)
    - output/<prospect-slug>/sales/cold-email.md

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Issues: [any problems encountered]
```

L4. Verify `sales/cold-email.md` exists.

L5. Generate `output/<prospect-slug>/README.md`:
```markdown
# Light Mode Proposal: [Prospect Business Name]

Generated by [Member Name] using Proposal Generator (light mode)

## Files
| File | Description |
|------|-------------|
| `proposal/proposal.html` | One-page branded proposal (open in browser or print to PDF) |
| `sales/cold-email.md` | Cold outreach email with 3 subject lines |

## Next Steps
1. Open `proposal/proposal.html` in your browser
2. Replace pricing placeholders with your actual prices
3. Print to PDF if sending as attachment
4. Review and send the cold email with the proposal attached
5. If the prospect responds: run `/build-proposal` in full mode for the complete package
```

L6. Report results:
```
Light mode proposal complete for [Prospect Business Name].

Proposal: output/<prospect-slug>/proposal/proposal.html
Cold email: output/<prospect-slug>/sales/cold-email.md

Next steps:
1. Open the proposal in your browser and replace pricing placeholders
2. Print to PDF for sending
3. Review and send the cold email
4. If the prospect responds, run /build-proposal in full mode for the complete package
```

L7. STOP -- do NOT continue to Phase 5.5 or beyond.

### Phase 6: Quality Gate (MANDATORY)

26. Spawn **Quality Reviewer**:

```
Task tool:
  description: "QA review all output"
  subagent_type: "general-purpose"
  model: "opus"
  max_turns: 20
  prompt: |
    You are the Quality Reviewer (QAS) for the Proposal Generator.
    Read your full instructions at: .claude/agents/quality-reviewer.md

    ## Your Task
    Review ALL output files for quality, consistency, completeness, and design brief adherence.

    ## Files to Review (read these yourself)
    - Proposal: output/<prospect-slug>/proposal/proposal.html
    - Pitch Deck: output/<prospect-slug>/deck/pitch-deck.html
    - Objection Handling: output/<prospect-slug>/strategy/objection-handling.md
    - Follow-Up Sequence: output/<prospect-slug>/sales/follow-up-sequence.md
    - Cover Letter: output/<prospect-slug>/sales/cover-letter.md
    - Prospect Brief: output/<prospect-slug>/research/prospect-brief.md
    - Proposal Structure: output/<prospect-slug>/strategy/proposal-structure.md
    - Visual Design Brief: output/<prospect-slug>/design/visual-brief.md
    - Member Profile: config/member-profile.md

    ## Return Format
    Return:
    ## QAS REVIEW: APPROVED (or BLOCKED)
    [If BLOCKED: list specific issues with file paths]
    [If APPROVED: one-line summary per file]
```

27. If **APPROVED**: proceed to Phase 7.
28. If **BLOCKED**: Re-spawn the relevant agent(s) with QAS feedback. Max 2 re-review iterations.

### Phase 7: Output

29. Verify all files exist in `output/<prospect-slug>/`:
    - `research/prospect-brief.md`
    - `strategy/proposal-structure.md`
    - `strategy/objection-handling.md`
    - `design/visual-brief.md`
    - `proposal/proposal.html`
    - `deck/pitch-deck.html`
    - `sales/cover-letter.md`
    - `sales/follow-up-sequence.md`

30. Generate `output/<prospect-slug>/README.md`:

```markdown
# Proposal Package: [Prospect Business Name]

Generated by [Member Name] using Proposal Generator

## Deliverables

| File | Description |
|------|-------------|
| `proposal/proposal.html` | Branded multi-page proposal (open in browser or print to PDF) |
| `deck/pitch-deck.html` | Standalone pitch deck (8-12 slides, arrow key navigation) |
| `strategy/objection-handling.md` | Top 5 objections with ready responses |
| `sales/follow-up-sequence.md` | 3-email follow-up sequence (Day 2, 5, 10) |
| `sales/cover-letter.md` | Personalized cold email to the prospect |

## Before Sending

- [ ] Open `proposal/proposal.html` and replace pricing placeholders:
  - [YOUR PRICE - STARTER] -> your starter price
  - [YOUR PRICE - STANDARD] -> your standard price
  - [YOUR PRICE - PREMIUM] -> your premium price
- [ ] Replace any [PLACEHOLDER] content with real information
- [ ] Review social proof section (add real testimonials if you have them)
- [ ] Print proposal to PDF: open in browser > Print > Save as PDF
- [ ] Open `deck/pitch-deck.html` and replace pricing placeholders
- [ ] Review and personalize the cover letter
- [ ] Review the objection-handling cheat sheet before any call
- [ ] Queue the follow-up sequence (Day 2, 5, 10 after sending)

## Sending Order

1. Send the cold email (cover-letter.md) with proposal PDF attached
2. If they engage: share the pitch deck link for a visual walkthrough
3. Follow up on Day 2, 5, 10 using the follow-up sequence
4. Reference the objection-handling guide during any conversation
```

31. Report results:

```
Proposal package complete for [Prospect Business Name].

Proposal: output/<prospect-slug>/proposal/proposal.html
Pitch deck: output/<prospect-slug>/deck/pitch-deck.html
Cover letter: output/<prospect-slug>/sales/cover-letter.md
Follow-ups: output/<prospect-slug>/sales/follow-up-sequence.md
Objection guide: output/<prospect-slug>/strategy/objection-handling.md

Next steps:
1. Open the proposal and replace pricing placeholders with your actual prices
2. Print to PDF for sending
3. Review and personalize the cover letter
4. Send the cold email with the proposal attached
5. Queue follow-ups for Day 2, 5, and 10
```
