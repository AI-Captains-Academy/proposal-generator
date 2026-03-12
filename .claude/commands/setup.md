# /setup -- Member Customization Wizard

Walk the member through configuring their Proposal Generator instance. Fills out `config/member-profile.md` with their business details, services, and branding.

## Input: $ARGUMENTS

No arguments expected. This is an interactive wizard.

## Context Rules (MANDATORY)

Follow the Context Engineering rules in CLAUDE.md:
- This command runs directly (no agent spawning needed)
- Write the completed profile to `config/member-profile.md`

## Pipeline

### Step 1: Welcome

Tell the member:

```
Welcome to Proposal Generator setup.

I'll ask you a few questions to configure your branding, services, and preferences.
This only needs to be done once -- your settings will be used for every
proposal you generate.
```

### Step 2: Collect Information

Ask the member for the following information. Collect in this order:

**Your Business**
1. What's your business or agency name?
2. What's your name?
3. What's your website URL?
4. What's your contact email?
5. What's your phone number? (optional -- for proposals)

**Your Services**
6. What services do you offer? (list your main offerings)
7. Who is your ideal client? (industry, size, type)
8. What's your typical engagement range? (e.g., "$2,000-$10,000 projects")
9. What's your primary pitch type? (service proposal / brand sponsorship / speaking engagement / collaboration / other)

**Your Branding**
10. What should the proposal header say? (e.g., "Proposal from [Name] | [Business]")
11. What should the proposal footer say? (e.g., "[Business] | [Website] | [Email]")
12. What's your primary brand color? (hex code, e.g., #2563eb)
13. What's your accent color? (hex code, e.g., #f59e0b)
14. Do you have a logo? (file path or URL -- optional)

**Social Proof**
15. Do you have testimonials to include in proposals? (paste 1-3, or "none yet")
16. Do you have notable clients or projects to reference? (list, or "none yet")
17. Any certifications, awards, or credentials? (list, or "none")

### Step 3: Write Profile

Write the completed profile to `config/member-profile.md`:

```markdown
# Member Profile

## Your Business
- Business name: [answer]
- Your name: [answer]
- Website: [answer]
- Email: [answer]
- Phone: [answer or "not provided"]

## Your Services
- Services offered: [answer]
- Ideal client: [answer]
- Typical engagement range: [answer]
- Primary pitch type: [answer]

## Branding
- Proposal header: [answer]
- Proposal footer: [answer]
- Primary color: [answer]
- Accent color: [answer]
- Logo: [path/URL or "none"]

## Social Proof
- Testimonials: [answer]
- Notable clients: [answer]
- Credentials: [answer]
```

### Step 4: Confirm

Tell the member:

```
Setup complete. Your profile is saved at config/member-profile.md.

You can edit it anytime. Run /build-proposal to generate your first prospect proposal.

Note: Tool permissions are pre-configured in .claude/settings.local.json.
You won't need to approve individual tool calls during proposal builds.
```
