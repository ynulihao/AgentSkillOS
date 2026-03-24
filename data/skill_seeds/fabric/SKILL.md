---
name: fabric
description: "Use when processing content through Fabric CLI patterns -- summarizing articles, extracting insights, threat modeling, analyzing code/claims/malware, improving writing, or rating content. Selects the optimal pattern from 242+ specialized prompts and executes it."
---

# Fabric Skill

## Setup

Verify the Fabric repository is available before pattern selection:

```bash
if [ ! -d "$HOME/.claude/skills/fabric/fabric-repo" ]; then
  cd "$HOME/.claude/skills/fabric" && git clone https://github.com/danielmiessler/fabric.git fabric-repo
fi
```

## Workflow

1. **Identify intent** -- match the user request to a pattern category below
2. **Select pattern** -- choose the most specific pattern for the task
3. **Execute** -- run `fabric` with the selected pattern immediately
4. **Validate** -- confirm output matches user expectations; try alternative pattern if not

## Execution Formats

```bash
fabric "your text here" -p [pattern]       # Direct text
fabric -u "URL" -p [pattern]               # From URL
fabric -y "YOUTUBE_URL" -p [pattern]       # From YouTube
cat file.txt | fabric -p [pattern]         # From file
```

## Pattern Selection Decision Tree

| User Intent | Recommended Patterns |
|-------------|---------------------|
| Threat model | `create_threat_model`, `create_stride_threat_model` |
| Summarize | `summarize`, `create_5_sentence_summary`, `summarize_paper`, `summarize_meeting` |
| Extract wisdom/insights | `extract_wisdom`, `extract_insights`, `extract_main_idea` |
| Analyze code/claims/malware | `analyze_code`, `analyze_claims`, `analyze_malware` |
| Improve writing/code/prompt | `improve_writing`, `improve_prompt`, `review_code` |
| Create document | `create_prd`, `create_design_document`, `create_user_story` |
| Visualize | `create_mermaid_visualization`, `create_markmap_visualization` |
| Rate/judge content | `rate_content`, `judge_output`, `rate_ai_response` |
| YouTube video | `youtube_summary`, `extract_wisdom` (with `-y` flag) |
| Git changes | `summarize_git_changes`, `summarize_git_diff`, `summarize_pull-requests` |

## Full Pattern Reference (242 patterns)

### Security (15 patterns)
`create_threat_model`, `create_stride_threat_model`, `create_threat_scenarios`, `create_security_update`, `create_sigma_rules`, `write_nuclei_template_rule`, `write_semgrep_rule`, `analyze_threat_report`, `analyze_threat_report_cmds`, `analyze_threat_report_trends`, `t_threat_model_plans`, `ask_secure_by_design_questions`, `create_network_threat_landscape`, `analyze_incident`, `analyze_risk`

### Summarization (20 patterns)
`summarize`, `create_5_sentence_summary`, `create_micro_summary`, `create_summary`, `summarize_micro`, `summarize_meeting`, `summarize_paper`, `summarize_lecture`, `summarize_newsletter`, `summarize_debate`, `summarize_legislation`, `summarize_rpg_session`, `summarize_board_meeting`, `summarize_git_changes`, `summarize_git_diff`, `summarize_pull-requests`, `summarize_prompt`, `youtube_summary`, `create_ul_summary`, `create_cyber_summary`

### Extraction (25+ patterns)
`extract_wisdom`, `extract_article_wisdom`, `extract_book_ideas`, `extract_insights`, `extract_insights_dm`, `extract_main_idea`, `extract_recommendations`, `extract_ideas`, `extract_questions`, `extract_predictions`, `extract_controversial_ideas`, `extract_business_ideas`, `extract_skills`, `extract_patterns`, `extract_sponsors`, `extract_references`, `extract_instructions`, `extract_primary_problem`, `extract_primary_solution`, `extract_product_features`, `extract_core_message`, `extract_extraordinary_claims`

### Analysis (27+ patterns)
`analyze_claims`, `analyze_malware`, `analyze_code`, `analyze_paper`, `analyze_logs`, `analyze_debate`, `analyze_incident`, `analyze_comments`, `analyze_answers`, `analyze_email_headers`, `analyze_mistakes`, `analyze_personality`, `analyze_presentation`, `analyze_product_feedback`, `analyze_proposition`, `analyze_prose`, `analyze_risk`, `analyze_sales_call`, `analyze_tech_impact`, `analyze_threat_report`, `analyze_bill`, `analyze_candidates`, `analyze_cfp_submission`, `analyze_terraform_plan`, `analyze_interviewer_techniques`

### Creation (27+ patterns)
`create_prd`, `create_design_document`, `create_user_story`, `create_coding_project`, `create_coding_feature`, `create_mermaid_visualization`, `create_markmap_visualization`, `create_visualization`, `create_report_finding`, `create_newsletter_entry`, `create_keynote`, `create_academic_paper`, `create_flash_cards`, `create_quiz`, `create_graph_from_input`, `create_tags`, `create_art_prompt`, `create_command`, `create_pattern`, `create_logo`, `create_video_chapters`, `create_upgrade_pack`

### Improvement (10 patterns)
`improve_writing`, `improve_academic_writing`, `improve_prompt`, `improve_report_finding`, `review_code`, `review_design`, `refine_design_document`, `humanize`, `enrich_blog_post`, `clean_text`

### Rating (8 patterns)
`rate_ai_response`, `rate_ai_result`, `rate_content`, `rate_value`, `judge_output`, `label_and_rate`, `check_agreement`, `arbiter-evaluate-quality`

## Advanced Usage

```bash
# Chain patterns
fabric -u "URL" -p extract_wisdom > wisdom.txt && cat wisdom.txt | fabric -p create_5_sentence_summary

# Pipe from clipboard
pbpaste | fabric -p summarize
```

## Key Principle

Select the RIGHT pattern and execute immediately. Do not present pattern options to the user -- match intent to pattern and run it.

## Resources

- **Full pattern list**: `ls ${PAI_DIR}/skills/fabric/fabric-repo/data/patterns/`
- **Documentation**: https://github.com/danielmiessler/fabric
- **Update patterns**: `cd ${PAI_DIR}/skills/fabric/fabric-repo && git pull origin main`
