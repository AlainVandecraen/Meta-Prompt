# Workflow Orchestrator Meta-Prompt & User Manual

An advanced prompt engineering system designed to analyze vague, amorphous objectives and architect deterministic, end-to-end multi-step prompt pipelines. 

Developed to mitigate structural reliability degradation, attention dilution, and compounding errors in large language models (LLMs). 

## Architecture Overview

The meta-prompt acts as a Cognitive NLP Systems Architect. It processes your unformatted goals and structures them into an engineering-grade report alongside direct, production-ready execution prompts. 

## Quick Start Guide

### 1. Initialize the Environment

* Copy the full contents of workflow_orchestrator_prompt.md.
* Paste it into your preferred LLM (e.g., Claude 3.5 Sonnet, GPT-4o).
* **Crucial Parameter:** Set the model's temperature to 0.0 to enforce strict factual, deterministic output.

### 2. Populate the Runtime Interface

Scroll to the absolute bottom of the meta-prompt and replace the placeholders inside the <runtime_interface> block with your specific data: 

* **{{USER_OBJECTIVE}}**: The overarching goal you want the final AI system to achieve.
* **{{USER_PRIMARY_INPUTS}}**: The primary data types or source datasets that the downstream prompts will process.
* **{{USER_SECONDARY_INPUTS}}**: Contextual boundaries, compliance rules, schemas, or formatting/style requirements.

### 3. Run and Extract

Execute the prompt. The LLM will perform a Chain-of-Thought analysis and output: 

1. **The Architectural Report:** Explaining the taxonomy and selection of the pipeline topology.
2. **The Production Prompts:** Complete, functional, copy-paste-ready prompts isolated within strict XML tags.

## Core Pipeline Concepts

### Single-Session vs. Multi-Session

* **Single-Session:** Selected automatically if the user task is lean and direct.
* **Multi-Session:** Mandated when the workflow contains sequential dependencies that risk memory saturation or attention decay. The objective is isolated into step-by-step prompts.

## Hydration & Dehydration

When a multi-session pipeline is deployed, the system utilizes a unified state-tracking mechanism: 

* **Dehydration:** Prompts at the beginning of the chain are forced to output their analytical state into a valid JSON object matching the **Universal Context Object JSON Schema**.
* **Hydration:** Downstream prompts ingest this JSON object at the absolute top of their execution block, instantly restoring the pipeline's operational state without context drift.

## Best Practices

* **Keep Prompts Production-Ready:** The generated production prompts utilize strict uppercase [VARIABLE] syntax. Make sure to map these accurately when deploying them in your code or UI wrappers.
* **Maintain Structural XML Tags:** Do not strip out tags like <system_priming> or <execution_logic> in the generated output; they ensure the target LLM strictly adheres to constraints.
