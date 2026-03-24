---
name: prompt-engineering-patterns
description: "Use when designing, optimizing, or debugging prompts for production LLM applications. Covers few-shot learning, chain-of-thought reasoning, template systems, system prompt design, and prompt performance optimization."
---

# Prompt Engineering Patterns

## Workflow

1. **Define objective** -- specify the task, target model, and success criteria
2. **Start simple** -- write a direct instruction prompt first
3. **Add structure** -- layer in constraints, examples, or reasoning as needed
4. **Test and measure** -- evaluate on diverse inputs, track accuracy/consistency/latency
5. **Iterate** -- refine based on failure modes and edge cases
6. **Validate** -- confirm output format compliance, token efficiency, and reliability

## Core Patterns

### Instruction Hierarchy
```
[System Context] → [Task Instruction] → [Examples] → [Input Data] → [Output Format]
```

### Progressive Disclosure
Add complexity only when simpler prompts fail:

1. **Direct instruction**: "Summarize this article"
2. **Add constraints**: "Summarize in 3 bullet points, focusing on key findings"
3. **Add reasoning**: "Identify the main findings, then summarize in 3 bullet points"
4. **Add examples**: Include 2-3 input-output demonstration pairs

### Few-Shot Learning
- Select examples by semantic similarity to the input
- Balance example count against context window limits
- Use input-output pairs that cover typical cases and edge cases
- Retrieve examples dynamically from a knowledge base when possible

### Chain-of-Thought (CoT)
- **Zero-shot**: Append "Let's think step by step" to elicit reasoning
- **Few-shot**: Provide reasoning traces in examples
- **Self-consistency**: Sample multiple reasoning paths, take majority answer
- **Verification**: Add a step asking the model to check its own reasoning

### System Prompt Design
- Set role, expertise, and behavioral constraints upfront
- Define output format and structure requirements
- Include safety guidelines and content policies
- Move stable instructions to system prompt to reduce per-request tokens

### Error Recovery
- Include fallback instructions for ambiguous inputs
- Request confidence scores alongside outputs
- Specify how to indicate missing or insufficient information
- Add self-verification steps that check output against criteria

## Integration Examples

### With RAG Systems
```python
prompt = f"""Given the following context:
{retrieved_context}

{few_shot_examples}

Question: {user_question}

Answer based solely on the context above. If the context is insufficient, state what's missing."""
```

### With Validation
```python
prompt = f"""{main_task_prompt}

Verify your response meets these criteria:
1. Answers the question directly
2. Uses only provided context
3. Cites specific sources
4. Acknowledges uncertainty

If verification fails, revise your response."""
```

## Optimization

- **Token efficiency**: Remove redundancy, use abbreviations after first definition, consolidate similar instructions
- **Latency**: Minimize prompt length, use streaming, cache common prefixes, batch similar requests
- **Reliability**: Test on boundary inputs, version-control prompts, A/B test variations

## Common Pitfalls

| Pitfall | Fix |
|---------|-----|
| Over-engineering | Start simple, add complexity only when needed |
| Example pollution | Use examples that match the target task distribution |
| Context overflow | Limit examples to fit within token budget |
| Ambiguous instructions | Specify output format, constraints, and edge case handling |

## Success Metrics

Track: **Accuracy**, **Consistency** (across similar inputs), **Latency** (P50/P95/P99), **Token usage**, **Success rate** (valid outputs), **User satisfaction**

## Resources

- `references/few-shot-learning.md` -- example selection and construction
- `references/chain-of-thought.md` -- advanced reasoning techniques
- `references/prompt-optimization.md` -- systematic refinement workflows
- `references/prompt-templates.md` -- reusable template patterns
- `references/system-prompts.md` -- system-level prompt design
- `assets/prompt-template-library.md` -- battle-tested templates
- `scripts/optimize-prompt.py` -- automated optimization tool
