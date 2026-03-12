# Proposal Design Skill

HTML proposal design principles for building professional, branded, print-friendly proposals. Used by the Front-End Designer and Proposal Writer agents.

## Design Philosophy

A proposal is a business document, not a marketing page. It must:
- **Look professional** on screen AND when printed to PDF
- **Be scannable** -- prospects skim before they read
- **Build trust** through clean design, not flashy effects
- **Guide the eye** from problem to solution to pricing to CTA

## Layout Principles

### Page Structure
- **Max width**: 800px (standard print width, comfortable reading on screen)
- **Margins**: 40px horizontal, 40px vertical minimum
- **Section spacing**: 48px between major sections (page-break candidates)
- **Line height**: 1.6 for body text, 1.3 for headings

### Visual Hierarchy
1. **Proposal title**: Largest text on the page
2. **Section headers (H2)**: Bold, clearly separated, often with a colored underline or border
3. **Subsection headers (H3)**: Slightly smaller, same font family
4. **Body text**: 16px minimum, comfortable reading width
5. **Supporting text**: 14px, lighter color, captions and meta info

### Content Flow
```
Cover/Header
  ↓
Executive Summary (full mode)
  ↓
About the Prospect (shows you understand them)
  ↓
The Opportunity (problem/gap framed positively)
  ↓
Proposed Solution (what you'd do)
  ↓
Deliverables (what they get)
  ↓
Timeline (when they get it)
  ↓
Investment (pricing tiers)
  ↓
Why You (social proof)
  ↓
Next Steps (CTA)
  ↓
Footer
```

## Branding Rules

The proposal is FROM the member TO the prospect:
- **Header**: Member logo (if provided) + member business name
- **Colors**: Member's brand palette (primary + accent from member profile)
- **Footer**: Member contact info (name, email, phone, website)
- **No prospect branding** in the design -- the proposal carries the member's identity

## Pricing Section Design

The pricing section is the most important visual element:

### 3-Column Card Layout
```
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│   STARTER   │  │  STANDARD   │  │   PREMIUM   │
│             │  │  ★ Popular  │  │             │
│ [YOUR PRICE │  │ [YOUR PRICE │  │ [YOUR PRICE │
│  - STARTER] │  │ - STANDARD] │  │  - PREMIUM] │
│             │  │             │  │             │
│ • Item 1    │  │ • Item 1    │  │ • Item 1    │
│ • Item 2    │  │ • Item 2    │  │ • Item 2    │
│             │  │ • Item 3    │  │ • Item 3    │
│             │  │ • Item 4    │  │ • Item 4    │
│             │  │             │  │ • Item 5    │
│             │  │             │  │ • Item 6    │
│ Best for:   │  │ Best for:   │  │ Best for:   │
│ [desc]      │  │ [desc]      │  │ [desc]      │
└─────────────┘  └─────────────┘  └─────────────┘
```

Design rules for pricing:
- **Standard tier highlighted**: Border, background tint, or "Most Popular" badge
- **Price placeholders prominent**: Large font, bold, clearly placeholder-looking
- **Clear tier differentiation**: Each tier adds visible value
- **"Best for" descriptions**: Help the prospect self-select
- **Responsive**: Stack to single column on mobile

## Print CSS Requirements

Every proposal MUST include print styles:

```css
@media print {
  body {
    font-size: 11pt;
    color: #000;
    background: #fff;
  }

  .page-break {
    page-break-before: always;
  }

  .no-print {
    display: none;
  }

  /* Ensure pricing table prints cleanly */
  .pricing-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 16px;
  }

  /* Remove shadows and backgrounds that waste ink */
  .card {
    box-shadow: none;
    border: 1px solid #ddd;
  }

  /* Ensure links show URLs */
  a[href]:after {
    content: " (" attr(href) ")";
    font-size: 0.8em;
    color: #666;
  }
}
```

Page break recommendations:
- After the cover/header section
- Before the pricing section
- Before the "Why [Member]" section
- Before the "Next Steps" section

## Technical Constraints

- **Single HTML file**: All CSS embedded, no external stylesheets
- **Google Fonts**: Max 2 font families, loaded via `<link>` tags
- **No JavaScript**: Pure HTML + CSS (except optional Tailwind CDN)
- **Responsive**: Must look good at 375px, 768px, and 1024px+
- **Valid HTML**: Semantic elements, proper nesting, lang attribute
- **Accessibility**: Color contrast ratios, semantic headings, alt text

## Common Anti-Patterns

| Anti-Pattern | Fix |
|---|---|
| Walls of text | Short paragraphs, bullet lists, generous spacing |
| Generic "we deliver excellence" language | Specific deliverables tied to the prospect |
| Pricing buried at the bottom | Pricing section visually prominent with clear tiers |
| No clear next step | Bold CTA section with specific action |
| Dark backgrounds that waste printer ink | Light backgrounds, dark text, print-friendly |
| Decorative elements that distract | Minimal decoration, focus on content |
| Tiny text to fit everything | Generous page length, let it flow to multiple pages |
