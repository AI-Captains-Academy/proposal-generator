# Proposal Generator Rig

A Claude Code rig that researches a prospect and generates a polished, branded proposal tailored to their specific needs. Runs in two modes: **light mode** (one-page proposal + cold email for initial outreach) and **full mode** (complete proposal package with pitch deck, objection-handling cheat sheet, follow-up sequence, and cover letter). Give it a prospect's URL and what you're pitching -- it analyzes their business, finds gaps, structures the offer, and builds everything you need to close.

Built for the "I already did the work" sales motion: research the prospect, build a tailored proposal, send it cold. Works for freelancers pitching services, creators pitching brand deals or speaking gigs, and beginners who need their first professional proposal.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed and configured

## Setup

1. Clone this repo
2. Open the `proposal-generator/` directory in Claude Code
3. Run `/setup` to configure your branding, services, and social proof
4. (Optional) Run `/voice` to pre-build a voice profile from your writing samples
5. Run `/build-proposal` to generate your first prospect proposal

## Usage

### Generate a proposal

```
/build-proposal
```

You'll be asked for:
- Prospect business name and URL
- What you're pitching (service, collaboration, sponsorship, speaking)
- Mode: light or full

The rig runs a 10-agent pipeline:

1. **Prospect Researcher** -- researches the prospect's website, social presence, pain points, and gaps
2. **Offer Architect** -- structures the proposal: problem > solution > deliverables > timeline > 3-tier pricing
3. **Front-End Designer** -- creates a visual design brief for the proposal HTML and pitch deck
4. **Proposal Writer** -- builds the proposal as polished, branded HTML (1-page light, multi-page full)
5. **Deck Builder** -- builds a standalone HTML pitch deck, 8-12 slides (full mode only)
6. **Objection Handler** -- generates top 5 likely objections with ready responses (full mode only)
7. **Follow-Up Writer** -- writes a 3-email follow-up sequence: Day 2, 5, 10 (full mode only)
8. **Cover Letter Writer** -- writes a personalized cold email (150-250 words, 3 subject lines)
9. **Design Reviewer** -- reviews proposal and deck HTML for layout, print quality, and design brief adherence
10. **Quality Reviewer** -- validates all output for quality, consistency, and completeness

Output lands in `output/<prospect-slug>/`.

## What You Get

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
    proposal.html                 # Branded multi-page proposal
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
  plan.md                         # Proposal plan (mode: light)
  research/
    prospect-brief.md             # Prospect analysis
  strategy/
    proposal-structure.md         # Proposal architecture
  design/
    visual-brief.md               # Visual design brief
  proposal/
    proposal.html                 # One-page branded proposal
  sales/
    cold-email.md                 # Cold outreach + 3 subject lines
```

Light mode runs research + structure + design + one-page proposal, then produces a cold email. Send the email with the proposal attached, run full mode if the prospect responds.

## How the Sales Motion Works

1. **Find a prospect** -- a business, brand, or organization you want to work with
2. **Run `/build-proposal`** -- use light mode for initial outreach (one-page proposal + cold email). If the prospect responds, run full mode for the complete package.
3. **Replace pricing placeholders** -- the rig uses `[YOUR PRICE - STARTER]`, `[YOUR PRICE - STANDARD]`, `[YOUR PRICE - PREMIUM]` so you set your own prices
4. **Print to PDF** -- open `proposal.html` in your browser > Print > Save as PDF
5. **Send the cold email** with the proposal PDF attached
6. **Follow up** -- use the 3-email sequence (Day 2, 5, 10) from full mode
7. **Handle objections** -- reference the objection-handling cheat sheet during calls

This is proof-of-work selling. You don't pitch "I can help you" -- you send them a tailored proposal that shows you already understand their business.

## Key Details

- **Pricing is always placeholders** -- the rig never suggests actual dollar amounts. You fill in your own prices before sending.
- **Proposals are member-branded** -- the proposal carries YOUR brand (colors, logo, business name), not the prospect's.
- **Pitch deck is standalone HTML** -- arrow key navigation, no framework dependencies. Works in any browser.
- **Print-friendly** -- all HTML output includes `@media print` CSS for clean PDF generation.
- **Voice-matched** -- if you run `/voice` first, proposals will match your natural writing tone.

## Configuration

Your settings live in `config/member-profile.md`. Run `/setup` to configure, or edit directly.

## Commands

| Command | What it does |
|---------|-------------|
| `/setup` | Configure your branding, services, and social proof |
| `/voice` | (Optional) Pre-build a voice profile from your writing samples |
| `/build-proposal` | Generate a proposal for a prospect |
