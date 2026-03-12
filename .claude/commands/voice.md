# /voice -- Analyze Your Writing Voice & Tone

Build a reusable voice profile for the member by analyzing their writing samples. This profile is saved and automatically used by `/build-proposal` for better tone matching in proposals.

## Input: $ARGUMENTS

If arguments provided, treat as source type. Otherwise ask.

## Context Rules (MANDATORY)

Follow the Context Engineering rules in CLAUDE.md:
- Do NOT read agent or skill files yourself
- Pass file PATHS to agents, not contents
- Agents write to disk directly
- You track status and file paths only

## When to Use

- **Before first proposal**: Build a voice profile so proposals sound like you from the start
- **For consistency**: Establish a persistent voice that carries across all proposals
- **During `/setup`**: Optional step to pre-configure a voice profile

## Pipeline

### Step 1: Collect Info

Ask the member:

1. **Source method**: How should we analyze your voice?
   - **"samples"**: I have writing samples to provide (URLs, file paths, or pasted text)
   - **"url"**: Analyze my website/blog/social media (provide URLs)
   - **"skip"**: I'll use the default professional tone

If "skip" is selected:
```
No voice profile created. Proposals will use a professional, confident default tone.

To build a custom voice profile later, run /voice again with writing samples.
```
STOP.

### Step 2: Collect Writing Samples

For "samples" or "url" mode:

Ask for **2-5 writing samples**. Ideal mix:
- A proposal, pitch, or sales email you've sent before
- A blog post or LinkedIn post in your voice
- Any casual writing (email, comment) that shows your natural tone

For each sample, accept:
- **URL**: Use WebFetch to retrieve the content
- **File path**: Read from disk
- **Pasted text**: Accept inline

Save all samples to `config/voices/member/samples/`:
- `sample-1.md`, `sample-2.md`, etc.

### Step 3: Spawn Voice Profiler

Create the voice directory if it doesn't exist:
```
config/voices/member/
  samples/
```

Spawn a general-purpose agent to build the voice profile:

```
Task tool:
  description: "Build member voice profile"
  subagent_type: "general-purpose"
  max_turns: 12
  prompt: |
    You are building a voice profile for a proposal writer.

    ## Your Task
    Analyze the writing samples and build a voice profile that captures:
    - Tone (formal/casual/confident/warm/direct)
    - Sentence structure patterns
    - Vocabulary preferences
    - How they open and close communications
    - Persuasion style (data-driven / story-driven / authority-driven / relationship-driven)
    - What to avoid (phrases, patterns that don't match their voice)

    ## Input Files (read these yourself)
    - Writing samples: config/voices/member/samples/ (read ALL .md files)

    ## Output Format
    Write to: config/voices/member/voice-profile.md

    Use this structure:
    ```markdown
    # Voice Profile: [Member Name]

    ## Tone
    [2-3 sentence description of overall tone]

    ## Sentence Structure
    - Average sentence length: [short/medium/long]
    - Preferred patterns: [e.g., "leads with action verbs", "uses rhetorical questions"]

    ## Vocabulary
    - Power words they use: [list]
    - Words/phrases to avoid: [list -- things that don't match their voice]
    - Industry jargon comfort: [high/medium/low]

    ## Opening Style
    [How they typically open communications]

    ## Closing Style
    [How they typically close -- soft CTA, direct ask, etc.]

    ## Persuasion Style
    [Primary: data/story/authority/relationship. Secondary if applicable.]

    ## Do / Don't
    - DO: [3-5 things that make their writing sound like them]
    - DON'T: [3-5 things that would break their voice]
    ```

    ## Return Format
    Return ONLY:
    - Status: SUCCESS or FAILED
    - Files created: [list of paths]
    - Samples analyzed: [count]
    - Issues: [any problems encountered]
```

### Step 4: Verify & Report

Verify `config/voices/member/voice-profile.md` exists.

Report results:

```
Voice profile created.

Saved to: config/voices/member/voice-profile.md
Samples analyzed: [count]

This profile will be automatically used when you run /build-proposal.
Proposals will match your natural tone and persuasion style.

To update: run /voice again with new or additional samples.
To review: open config/voices/member/voice-profile.md
```
