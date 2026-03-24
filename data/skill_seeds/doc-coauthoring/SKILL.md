---
name: doc-coauthoring
description: "Use when the user wants to co-author documentation, proposals, technical specs, decision docs, PRDs, RFCs, or any structured written content. Guides through context gathering, iterative section-by-section refinement, and reader testing to produce documents that work for their audience."
---

# Doc Co-Authoring Workflow

Guide users through three stages of collaborative document creation: Context Gathering, Refinement & Structure, and Reader Testing.

## Stage 1: Context Gathering

**Goal:** Close the knowledge gap to enable smart guidance.

1. **Ask meta-context questions:**
   - Document type (technical spec, decision doc, proposal, PRD, RFC)
   - Primary audience and desired impact
   - Template or format requirements
   - Constraints and context
2. **Encourage context dumping** -- user provides background, related docs, discussions, architecture, stakeholder concerns in any format
3. **Use integrations** (Slack, Drive, SharePoint, MCP servers) to pull context when available. If unavailable, suggest enabling connectors or pasting content directly
4. **Ask 5-10 clarifying questions** based on gaps. Accept shorthand answers
5. **Exit when** you can ask about edge cases and trade-offs without needing basics explained

**If user provides a template:** Read it. **If editing an existing doc:** Read current state, flag images without alt-text.

## Stage 2: Refinement & Structure

**Goal:** Build the document section by section through brainstorming, curation, and iterative refinement.

### Section Ordering
- Start with the section with most unknowns (core proposal for decision docs, technical approach for specs)
- If user doesn't know sections needed, suggest 3-5 appropriate ones
- Leave summary sections for last

### Per-Section Workflow

1. **Clarify** -- Ask 5-10 questions about what to include
2. **Brainstorm** -- Generate 5-20 numbered options, surfacing forgotten context and unconsidered angles
3. **Curate** -- User selects what to keep/remove/combine (accept shorthand like "Keep 1,4,7" or freeform feedback)
4. **Gap check** -- Ask if anything important is missing
5. **Draft** -- Use `str_replace` to replace placeholder with content
6. **Refine** -- Iterate via surgical edits until user is satisfied

**Key behaviors:**
- Create scaffold with all section headers and `[To be written]` placeholders using `create_file` (artifacts) or a markdown file
- Use `str_replace` for all edits, never reprint the whole doc
- After 3 iterations with no substantial changes, ask if anything can be removed
- On first section, instruct user to describe changes rather than edit directly (helps learn their style)

### Near Completion
When 80%+ sections are done, re-read entire document checking for: flow consistency, redundancy, contradictions, generic filler. Review for coherence before moving to Stage 3.

## Stage 3: Reader Testing

**Goal:** Test the document with a fresh Claude (no context bleed) to catch blind spots.

### With Sub-Agents (Claude Code)
1. Predict 5-10 realistic reader questions
2. Test each with a sub-agent given only the document
3. Run additional checks for ambiguity, false assumptions, contradictions
4. Fix any gaps found by looping back to refinement

### Without Sub-Agents (claude.ai)
1. Predict 5-10 reader questions
2. Instruct user to open a fresh Claude conversation, paste the doc, and ask the questions
3. Have Reader Claude also check for ambiguity, assumed knowledge, contradictions
4. Iterate on any issues found

**Exit when** Reader Claude consistently answers correctly with no new gaps.

## Final Review

1. Recommend user does a final read-through (they own quality)
2. Suggest verifying facts, links, technical details
3. Confirm it achieves the intended impact

**Final tips:** Link this conversation in an appendix, use appendices for depth, update the doc as real reader feedback arrives.

## Behavioral Guidelines

- **Tone:** Direct and procedural. Brief rationale only when it affects user behavior
- **Deviations:** If user wants to skip a stage, let them. If frustrated, suggest ways to move faster. Always give agency
- **Context:** Proactively ask about gaps as they arise -- don't let them accumulate
- **Artifacts:** Use `create_file` for drafts, `str_replace` for edits, never use artifacts for brainstorming lists
