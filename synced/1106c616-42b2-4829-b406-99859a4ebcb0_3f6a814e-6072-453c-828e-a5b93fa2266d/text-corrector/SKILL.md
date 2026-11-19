---
name: text-corrector
description: Corrects and improves text in Portuguese and English with balanced grammar, spelling, semantics, and style adjustments. Handles plain text, Markdown, and code comments. Use whenever the user asks to "correct this text", "improve this writing", "review this", "fix this", "polish this", "refine this", or "make this better" — for emails, documentation, blog posts, code comments, reports, or any written content. By default, returns only the corrected text. User can request a detailed list of changes by saying "revise o texto" or "list the changes".
---

# Text & Semantic Corrector

Corrects and enhances written content across Portuguese and English with a balanced approach: fixes errors while preserving the author's voice and tone.

## How to Use

### Default Mode (Corrected Text Only)

When the user provides text to correct:

1. **Analyze** the text for errors and improvements:
   - Spelling & typos
   - Grammar & syntax
   - Punctuation
   - Semantic clarity & flow
   - Word choice & redundancy
   - Tone consistency (if context is provided)

2. **Preserve** the original:
   - Author's voice and tone
   - Capitalization patterns (unless it's an error)
   - Structural intent (paragraphs, lists, etc.)
   - Code syntax (for code comments)
   - Markdown formatting

3. **Return** the corrected text directly — clean, readable, no preamble.

### List Mode (Detailed Changes)

When the user says "revise o texto" or asks for a list of changes:

1. Return the corrected text
2. Follow with a **Changes List** section:
   - **Original → Corrected** (show the change)
   - **Reason** (brief explanation: grammar, clarity, style, etc.)
   - Number each entry

## Style Guidelines

- **Active voice preferred** (when natural)
- **Clarity over cleverness**
- **Conciseness** (remove redundancy)
- **Natural flow** (read aloud mentally)
- **Consistency** (terminology, tense, formatting)
- **Tone match** (formal/informal/technical per context)

## Special Cases

### Code Comments
- Fix grammar & typos
- Improve clarity
- Keep code-style markers (backticks, brackets)
- Don't change code logic or variable names

### Markdown
- Preserve all formatting (# headers, **bold**, _italic_, links, etc.)
- Fix text within formatting markers
- Don't change structure unless it breaks readability

### Mixed Language
- If text switches between Portuguese and English, correct each section in its language
- Note if mixing is inconsistent and could be improved

### Tone Inference
- If the user provides context (e.g., "this is for a client email"), adapt corrections to match that tone
- Without context, assume balanced professional/neutral tone
- Always ask if uncertain

## Examples

**Input (plain text):**
```
I have recebido seu email sobre o projecto. Porfavor, revise os detalhes mais cuidadoso. 
A data limite é muito proxima.
```

**Output (default mode):**
```
I have received your email about the project. Please review the details more carefully. 
The deadline is very close.
```

**Output (list mode):**
```
I have received your email about the project. Please review the details more carefully. 
The deadline is very close.

**Changes:**
1. "recebido" → "received" — Mixed language correction (Portuguese word in English sentence)
2. "projecto" → "project" — Spelling (Portuguese spelling changed to English)
3. "Porfavor" → "Please" — Typo + flow improvement (awkward phrasing)
4. "mais cuidadoso" → "more carefully" — Grammar + mixed language
5. "proxima" → "close" — Spelling + improved phrasing (avoid "very proxima")
```

## When NOT to Over-Correct

- Don't "fix" intentional stylistic choices (fragments for emphasis, casual tone, etc.)
- Don't rewrite entire sentences if a small tweak works
- Don't impose corporate jargon if the author's voice is personal
- Don't change technical terminology without context

---

## Troubleshooting

**Q: The user hasn't provided text to correct.**
A: Ask them to paste the text they'd like corrected.

**Q: Text is very long.**
A: Process it all — no artificial limits. Break output into logical sections if readability helps.

**Q: The user wants a specific style guide (AP, Chicago, company handbook).**
A: Ask them to provide or describe it, then apply those rules.

**Q: Unclear language — multiple valid interpretations.**
A: Correct what's clearly wrong; flag ambiguities in the changes list (if requested) with "Suggestion: could also be…"
