---
name: tooluniverse
description: "Use when searching for, executing, or composing scientific research tools across bioinformatics, cheminformatics, genomics, structural biology, proteomics, and drug discovery. Provides standardized access to 600+ tools including ML models, datasets, and APIs (OpenTargets, PubChem, UniProt, PDB, ChEMBL). Supports tool discovery via keyword/semantic/LLM search, multi-step research pipelines, and MCP integration with Claude."
---

# ToolUniverse

## Overview

ToolUniverse enables AI agents to discover, execute, and compose 600+ scientific tools across bioinformatics, cheminformatics, genomics, structural biology, proteomics, and drug discovery through a standardized interface.

## Quick Start

### Basic Setup
```python
from tooluniverse import ToolUniverse

# Initialize and load tools
tu = ToolUniverse()
tu.load_tools()  # Loads 600+ scientific tools

# Discover tools
tools = tu.run({
    "name": "Tool_Finder_Keyword",
    "arguments": {
        "description": "disease target associations",
        "limit": 10
    }
})

# Execute a tool
result = tu.run({
    "name": "OpenTargets_get_associated_targets_by_disease_efoId",
    "arguments": {"efoId": "EFO_0000537"}  # Hypertension
})
```

### Model Context Protocol (MCP)
For Claude Desktop/Code integration:
```bash
tooluniverse-smcp
```

## Core Workflows

### 1. Tool Discovery

Find relevant tools for your research task:

**Three discovery methods:**
- `Tool_Finder` - Embedding-based semantic search (requires GPU)
- `Tool_Finder_LLM` - LLM-based semantic search (no GPU required)
- `Tool_Finder_Keyword` - Fast keyword search

**Example:**
```python
# Search by natural language description
tools = tu.run({
    "name": "Tool_Finder_LLM",
    "arguments": {
        "description": "Find tools for RNA sequencing differential expression analysis",
        "limit": 10
    }
})

# Review available tools
for tool in tools:
    print(f"{tool['name']}: {tool['description']}")
```

**See `references/tool-discovery.md` for:**
- Detailed discovery methods and search strategies
- Domain-specific keyword suggestions
- Best practices for finding tools

### 2. Tool Execution

Execute individual tools through the standardized interface:

**Example:**
```python
# Execute disease-target lookup
targets = tu.run({
    "name": "OpenTargets_get_associated_targets_by_disease_efoId",
    "arguments": {"efoId": "EFO_0000616"}  # Breast cancer
})

# Get protein structure
structure = tu.run({
    "name": "AlphaFold_get_structure",
    "arguments": {"uniprot_id": "P12345"}
})

# Calculate molecular properties
properties = tu.run({
    "name": "RDKit_calculate_descriptors",
    "arguments": {"smiles": "CCO"}  # Ethanol
})
```

**See `references/tool-execution.md` for:**
- Real-world execution examples across domains
- Tool parameter handling and validation
- Result processing and error handling
- Best practices for production use

### 3. Tool Composition and Workflows

Compose multiple tools for complex research workflows:

**Drug Discovery Example:**
```python
# 1. Find disease targets
targets = tu.run({
    "name": "OpenTargets_get_associated_targets_by_disease_efoId",
    "arguments": {"efoId": "EFO_0000616"}
})

# 2. Get protein structures
structures = []
for target in targets[:5]:
    structure = tu.run({
        "name": "AlphaFold_get_structure",
        "arguments": {"uniprot_id": target['uniprot_id']}
    })
    structures.append(structure)

# 3. Screen compounds
hits = []
for structure in structures:
    compounds = tu.run({
        "name": "ZINC_virtual_screening",
        "arguments": {
            "structure": structure,
            "library": "lead-like",
            "top_n": 100
        }
    })
    hits.extend(compounds)

# 4. Evaluate drug-likeness
drug_candidates = []
for compound in hits:
    props = tu.run({
        "name": "RDKit_calculate_drug_properties",
        "arguments": {"smiles": compound['smiles']}
    })
    if props['lipinski_pass']:
        drug_candidates.append(compound)
```

**See `references/tool-composition.md` for:**
- Complete workflow examples (drug discovery, genomics, clinical)
- Sequential and parallel tool composition patterns
- Output processing hooks
- Workflow best practices

## Scientific Domains

ToolUniverse supports 600+ tools across major scientific domains:

**Bioinformatics:**
- Sequence analysis, alignment, BLAST
- Gene expression (RNA-seq, DESeq2)
- Pathway enrichment (KEGG, Reactome, GO)
- Variant annotation (VEP, ClinVar)

**Cheminformatics:**
- Molecular descriptors and fingerprints
- Drug discovery and virtual screening
- ADMET prediction and drug-likeness
- Chemical databases (PubChem, ChEMBL, ZINC)

**Structural Biology:**
- Protein structure prediction (AlphaFold)
- Structure retrieval (PDB)
- Binding site detection
- Protein-protein interactions

**Proteomics:**
- Mass spectrometry analysis
- Protein databases (UniProt, STRING)
- Post-translational modifications

**Genomics:**
- Genome assembly and annotation
- Copy number variation
- Clinical genomics workflows

**Medical/Clinical:**
- Disease databases (OpenTargets, OMIM)
- Clinical trials and FDA data
- Variant classification

**See `references/domains.md` for:**
- Complete domain categorization
- Tool examples by discipline
- Cross-domain applications
- Search strategies by domain

## Reference Documentation

This skill includes comprehensive reference files that provide detailed information for specific aspects:

- **`references/installation.md`** - Installation, setup, MCP configuration, platform integration
- **`references/tool-discovery.md`** - Discovery methods, search strategies, listing tools
- **`references/tool-execution.md`** - Execution patterns, real-world examples, error handling
- **`references/tool-composition.md`** - Workflow composition, complex pipelines, parallel execution
- **`references/domains.md`** - Tool categorization by domain, use case examples
- **`references/api_reference.md`** - Python API documentation, hooks, protocols

**Workflow:** When helping with specific tasks, reference the appropriate file for detailed instructions. For example, if searching for tools, consult `references/tool-discovery.md` for search strategies.

## Example Scripts

Two executable example scripts demonstrate common use cases:

**`scripts/example_tool_search.py`** - Demonstrates all three discovery methods:
- Keyword-based search
- LLM-based search
- Domain-specific searches
- Getting detailed tool information

**`scripts/example_workflow.py`** - Complete workflow examples:
- Drug discovery pipeline (disease → targets → structures → screening → candidates)
- Genomics analysis (expression data → differential analysis → pathways)

Run examples to understand typical usage patterns and workflow composition.

## Best Practices

- **Discovery**: Start broad, refine. Use `Tool_Finder_Keyword` for known terms, `Tool_Finder_LLM` for semantic queries
- **Execution**: Validate input formats (SMILES, UniProt IDs, gene symbols) before running tools
- **Composition**: Test steps individually, checkpoint long workflows, respect API rate limits
- **Integration**: Initialize `ToolUniverse()` once, call `load_tools()` once at startup

## Resources

- **GitHub**: https://github.com/mims-harvard/ToolUniverse
- **Documentation**: https://zitniklab.hms.harvard.edu/ToolUniverse/
- **Installation**: `pip install tooluniverse`
- **MCP Server**: `tooluniverse-smcp`
