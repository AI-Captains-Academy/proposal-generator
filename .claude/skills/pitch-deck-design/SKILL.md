# Pitch Deck Design Skill

Standalone HTML slide deck design principles. Used by the Front-End Designer and Deck Builder agents.

## Design Philosophy

A pitch deck is a visual companion to the proposal. It should:
- **Tell the story in 10 slides** -- not replace the proposal, but summarize it visually
- **One idea per slide** -- if you're explaining two things, you need two slides
- **Be presenter-ready** -- the member can walk through it on a screen share
- **Stand alone** -- the prospect can also click through it without narration

## Slide Structure

### Recommended 10-Slide Flow

| # | Slide | Content | Design |
|---|-------|---------|--------|
| 1 | **Title** | Member name, "For [Prospect]", date | Clean, branded, confident |
| 2 | **Hook** | One powerful statement + data point | Bold text, minimal |
| 3 | **Problem** | 2-3 pain points from research | Visual list or icons |
| 4 | **Solution** | Your approach in 1-2 sentences | Clean, authoritative |
| 5 | **Process** | Timeline overview | Visual timeline/steps |
| 6 | **Deliverables** | What they get | Clean bullet list |
| 7 | **Timeline** | Phase breakdown | Table or visual |
| 8 | **Investment** | 3-tier pricing | Card layout |
| 9 | **Why You** | Social proof, credentials | Quote + stats |
| 10 | **Next Steps** | CTA + contact info | Clear action |

### Optional Additional Slides
- **Case Study** (after slide 9): Brief before/after for a relevant client
- **FAQ** (after slide 10): 3-4 common questions answered
- **Contact** (final): Full contact details

Total: 8-12 slides. Never more than 12.

## Typography Rules

Large text is mandatory for decks:

| Element | Min Size | Weight |
|---------|---------|--------|
| Slide title | 48px | Bold |
| Body text | 24px | Regular |
| Supporting text | 18px | Regular |
| Captions/labels | 14px | Medium |

## Layout Patterns

### Full-Width Text
```
┌─────────────────────────────────┐
│                                 │
│     Big Statement Goes Here     │
│                                 │
│     Supporting detail below     │
│                                 │
└─────────────────────────────────┘
```
Use for: Title, Hook, CTA slides

### Split Layout (60/40)
```
┌──────────────────┬──────────────┐
│                  │              │
│   Text content   │   Visual /   │
│   on left side   │   Stats /    │
│                  │   Image      │
│                  │              │
└──────────────────┴──────────────┘
```
Use for: Problem, Solution, Why You slides

### Grid Layout
```
┌────────┬────────┬────────┐
│  Item  │  Item  │  Item  │
│   1    │   2    │   3    │
├────────┼────────┼────────┤
│  Item  │  Item  │  Item  │
│   4    │   5    │   6    │
└────────┴────────┴────────┘
```
Use for: Deliverables, Features slides

### Card Layout (Pricing)
```
┌──────────┐ ┌──────────┐ ┌──────────┐
│ Starter  │ │★Standard │ │ Premium  │
│          │ │          │ │          │
│ [PRICE]  │ │ [PRICE]  │ │ [PRICE]  │
│          │ │          │ │          │
│ • item   │ │ • item   │ │ • item   │
│ • item   │ │ • item   │ │ • item   │
│          │ │ • item   │ │ • item   │
│          │ │          │ │ • item   │
└──────────┘ └──────────┘ └──────────┘
```
Use for: Investment/Pricing slide

## Technical Requirements

### HTML Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pitch Deck: [Prospect] | [Member]</title>
  <link href="[Google Fonts]" rel="stylesheet">
  <style>/* All CSS embedded */</style>
</head>
<body>
  <div class="deck">
    <div class="slide active"><!-- Slide 1 --></div>
    <div class="slide"><!-- Slide 2 --></div>
    <!-- ... -->
  </div>
  <div class="slide-counter">1 / 10</div>
  <script>/* Navigation only */</script>
</body>
</html>
```

### Navigation
- **Arrow keys**: Left/Right to navigate
- **Spacebar**: Advance to next slide
- **Click**: Advance (unless clicking a link or button)
- **Slide counter**: Shows current position (e.g., "3 / 10")

### Slide Transitions
CSS-only transitions:
```css
.slide {
  opacity: 0;
  transition: opacity 0.3s ease;
  position: absolute;
  inset: 0;
}
.slide.active {
  opacity: 1;
}
```

### Aspect Ratio
16:9 is standard:
```css
.deck {
  position: relative;
  width: 100vw;
  max-width: 1280px;
  aspect-ratio: 16 / 9;
  margin: 0 auto;
  overflow: hidden;
}
```

## Color & Branding

- Use the member's brand colors (from visual design brief)
- Alternating slide backgrounds: primary on white, or white on primary
- Key numbers/stats in the accent color
- Consistent styling on every slide

## Common Anti-Patterns

| Anti-Pattern | Fix |
|---|---|
| Too much text per slide | One idea, max 6 bullet points |
| Tiny text | Minimum 24px body, 48px titles |
| No clear narrative flow | Follow the 10-slide structure |
| Decorative clip art | Clean icons or nothing |
| Automated slide transitions | Manual navigation only |
| More than 12 slides | Cut or combine |
| Real prices in placeholders | Always [YOUR PRICE - X] format |
