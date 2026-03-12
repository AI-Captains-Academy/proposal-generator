# Prospect Research Skill

Domain knowledge for researching a prospect before building a proposal. Used by the Prospect Researcher agent.

## What to Look For

### Business Fundamentals
- **What they do**: Core product/service, value proposition, target customer
- **How they make money**: Revenue model (subscription, project-based, retainer, product sales)
- **Business stage**: Startup signals (new domain, sparse content) vs. established (deep content, team page, case studies)
- **Team size**: About page headcount, LinkedIn signals, "we" vs. "I" language

### Gap Signals (Opportunities for the Member)

**Website Gaps:**
- Outdated design (copyright dates, old frameworks, broken elements)
- No clear CTA or lead capture
- Positioning confusion (hard to tell what they actually do in 5 seconds)
- No social proof visible
- Blog that stopped 6+ months ago
- No case studies or portfolio
- Slow page load

**Marketing Gaps:**
- No visible funnel (no lead magnet, no email capture beyond "subscribe")
- Social media present but inactive or inconsistent
- No content marketing strategy visible
- Ad presence without proper landing pages
- SEO gaps (no blog, thin meta descriptions, missing structured data)

**Sales Gaps:**
- No clear pricing or packaging visible
- No "how it works" or process page
- No testimonials or case studies
- Contact form but no clear next step

**Technology Gaps:**
- Using basic tools when better options exist
- No visible analytics or tracking
- Manual processes that could be automated

### Pain Signals (Active Problems)

- Recent negative reviews or complaints
- Job postings that suggest growing pains
- Pricing changes or restructuring
- New competitor entries in their space
- Platform/tool migrations mentioned in blog or social
- Declining social engagement or content frequency

## Where to Look

### Primary Sources (always check)
1. **Homepage**: Business model, positioning, visual quality, CTA quality
2. **About/Services page**: What they offer, how they describe it, team signals
3. **Blog/Content**: Activity level, topics, quality, last publish date

### Secondary Sources (check if relevant)
4. **Social profiles**: LinkedIn company page, Twitter/X, Instagram -- activity and engagement
5. **Google search**: "[Business name] reviews", "[Business name] news", recent press
6. **Job postings**: Indeed/LinkedIn for open roles -- signals about growth and gaps

### Do NOT
- Assume information you can't observe
- Fabricate team sizes, revenue numbers, or client counts
- Present opinions as facts
- Research competitors extensively (brief mention is fine)
- Spend more than 3 WebFetch calls

## Confidence Levels

Always indicate confidence:
- **High confidence**: Directly observable on their website (e.g., "Their pricing page lists three tiers")
- **Medium confidence**: Inferable from multiple signals (e.g., "Based on team page and job postings, they appear to have 10-20 employees")
- **Low confidence**: Single weak signal (e.g., "A 2023 blog post mentioned they were exploring [X]")

## Connecting Research to Proposals

The research output must answer these questions for downstream agents:
1. **What problem can the member solve?** (Offer Architect needs this)
2. **What specific details prove we did homework?** (Proposal Writer needs this)
3. **What's the single best hook for cold outreach?** (Cover Letter Writer needs this)
4. **What objections will they likely raise?** (Objection Handler needs this)
