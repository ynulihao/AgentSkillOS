---
name: mcp-builder
description: "Use when building an MCP (Model Context Protocol) server to integrate external APIs or services with LLMs. Guides through research, implementation, testing, and evaluation creation for TypeScript (MCP SDK) or Python (FastMCP) servers with proper tool design, error handling, and pagination."
license: Complete terms in LICENSE.txt
---

# MCP Server Development Guide

Build MCP servers that enable LLMs to interact with external services through well-designed tools. Quality is measured by how effectively LLMs can accomplish real-world tasks using the server.

## Phase 1: Research and Planning

### 1.1 MCP Design Principles

- **API coverage first:** Prioritize comprehensive endpoint coverage over workflow tools. Workflow tools add convenience but coverage gives agents composability
- **Tool naming:** Use consistent prefixes and action-oriented names (e.g., `github_create_issue`, `github_list_repos`)
- **Context management:** Return focused, relevant data with filter/pagination support
- **Error messages:** Guide agents toward solutions with specific suggestions and next steps

### 1.2 Study Protocol and Framework Docs

**MCP specification:** Start at `https://modelcontextprotocol.io/sitemap.xml`, fetch pages with `.md` suffix. Review specification overview, transport mechanisms, tool/resource/prompt definitions.

**Recommended stack:** TypeScript with streamable HTTP (remote) or stdio (local). See [MCP Best Practices](./reference/mcp_best_practices.md).

**Load SDK docs:**
- **TypeScript (recommended):** WebFetch `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md` + [TypeScript Guide](./reference/node_mcp_server.md)
- **Python:** WebFetch `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md` + [Python Guide](./reference/python_mcp_server.md)

### 1.3 Plan Implementation

Review the target API documentation for endpoints, auth, and data models. List endpoints to implement starting with the most common operations.

## Phase 2: Implementation

### 2.1 Project Setup

See language-specific guides: [TypeScript Guide](./reference/node_mcp_server.md) | [Python Guide](./reference/python_mcp_server.md)

### 2.2 Core Infrastructure

Create shared utilities: API client with auth, error handling helpers, response formatting (JSON/Markdown), pagination support.

### 2.3 Implement Tools

For each tool, define:

- **Input schema:** Zod (TS) or Pydantic (Python) with constraints, descriptions, and examples
- **Output schema:** Define `outputSchema` for structured data; use `structuredContent` in responses
- **Description:** Concise summary with parameter descriptions and return type
- **Implementation:** Async/await, actionable error handling, pagination support
- **Annotations:** `readOnlyHint`, `destructiveHint`, `idempotentHint`, `openWorldHint`

## Phase 3: Review and Test

1. **Code quality:** DRY, consistent error handling, full type coverage, clear tool descriptions
2. **Build and test:**
   - TypeScript: `npm run build` then `npx @modelcontextprotocol/inspector`
   - Python: `python -m py_compile your_server.py` then MCP Inspector
3. See language-specific guides for detailed checklists

## Phase 4: Create Evaluations

Load [Evaluation Guide](./reference/evaluation.md) for complete guidelines.

### Process
1. **Inspect** available tools and capabilities
2. **Explore** data using READ-ONLY operations
3. **Generate** 10 complex, realistic questions requiring multiple tool calls
4. **Verify** each answer yourself

### Question Requirements
Each question must be: independent, read-only, complex, realistic, verifiable by string comparison, and stable over time.

### Output Format
```xml
<evaluation>
  <qa_pair>
    <question>Your complex question here</question>
    <answer>Verifiable answer</answer>
  </qa_pair>
</evaluation>
```

## Reference Files

Load as needed during development:

| Resource | When | Link |
|----------|------|------|
| MCP Best Practices | Phase 1 | [mcp_best_practices.md](./reference/mcp_best_practices.md) |
| Python SDK README | Phase 1-2 | `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md` |
| TypeScript SDK README | Phase 1-2 | `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md` |
| Python Implementation Guide | Phase 2 | [python_mcp_server.md](./reference/python_mcp_server.md) |
| TypeScript Implementation Guide | Phase 2 | [node_mcp_server.md](./reference/node_mcp_server.md) |
| Evaluation Guide | Phase 4 | [evaluation.md](./reference/evaluation.md) |
