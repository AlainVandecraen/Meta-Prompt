---
title: Prompt Architecture - User Manual
description: An introduction to a production-grade prompt engineering framework that structures instructions into system priming, execution logic, and runtime interface layers to ensure deterministic, scalable, and safe LLM outputs.
author: "[[Alain Vandecraen]]"
---
# Prompt Architecture - User Manual

This document outlines a comprehensive framework for designing and structuring prompts for Large Language Models (LLMs). It breaks down the prompt engineering process into a structured, production-grade architecture. This framework covers everything from initial system setups to precise execution logic and runtime data handling.

The document is organized into three primary architectural layers:

- **System Priming:** Focuses on setting up the initial context, persona, metadata, and boundaries of the operational environment.
- **Execution Logic:** Defines the primary directives, sequential workflows, guardrails, and example-driven learning patterns for the model.
- **Runtime Interface:** Establishes how data is injected, how the model processes its internal reasoning, and the exact specifications for the final output payload.

> [!WARNING] XML tags in production prompts
> Do NOT remove the XML tags. These are used by the LLM for context. Removing any may affect the quality of the output.

Structural Analysis & Prompt Comparison

| Feature Layer         | [[Prompt (Base)\|Basic]] | [[Prompt (Advanced)\|Extended]]                            | Architectural Purpose                                            |
| --------------------- | ------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------------- |
| **System Priming**    | Title, Role.              | Title, Introduction, Role, Context, Metadata & Parameters. | Sets boundaries, persona constraints, and initial macro-context. |
| **Execution Logic**   | Task.                     | Task, Steps, Guidelines, Edge Cases, Examples.             | Dictates workflows, constraints, and error-handling paths.       |
| **Runtime Interface** | Input, Output.            | Input, Thinking Protocol, Completeness Test, Output.       | Manages data payload injections, reasoning space, and schemas.   |

The primary difference between the two templates lies in their structural complexity and operational intent, mapping directly to the "Prompt Component Hierarchy" established in the architecture. The **[[Prompt (Base)|Basic]]** prompt functions as a lightweight, streamlined layout restricted entirely to critical core sections—**Role, Task, Input, and Output**—making it ideal for rapid prototyping or trivial, low-risk classifications. In contrast, the **[[Prompt (Advanced)|Extended]]** prompt is a fully realized, production-grade template designed for complex enterprise pipelines. It integrates crucial optional layers such as **Metadata & Parameters** for software schema binding, **Edge Cases** for defensive error-routing, and a **Thinking Protocol** to expand the model's computational depth before generating the final immutable payload.

## System Priming (XML-tag)

This section establishes the foundational environment and initial boundaries for the Large Language Model. It focuses on configuring the model's persona, setting up the macro-context, allocating metadata parameters, and anchoring the model's attention to optimize its reasoning capabilities from the very first token.

### Title

The primary semantic anchor and metadata identifier for the prompt assembly. This serves as the unique identifier within a prompt version control registry or deployment pipeline. It establishes the initial contextual namespace for the Large Language Model, optimizing attention allocation from the very first token.
### Introduction

The macro-contextual priming zone. This section initializes the high-level boundaries of the operational environment, defining the domain, intent, and scope of the interaction. It establishes the baseline semantic field, ensuring the model's latent space is correctly activated before parsing fine-grained operational logic.

### Role

The persona configuration and cognitive framework definition. By defining a specific persona, you constraint the model's probabilistic output distribution to match a targeted domain expertise. This acts as an explicit instruction to adjust the model's tone, vocabulary, depth of reasoning, and systemic biases to mimic a specialized agent.

### Context

The situational framing and foundational data layer. This section delivers the external variables, business logic, constraints, or historical data required for execution. It maps out the current state of the system or scenario, giving the LLM the factual grounding necessary to prevent hallucinations and accurately contextualize incoming inputs.

### Metadata & Parameters

The manifest contract and system orchestration interface. This block serves as the schema definition layer that bridges the upstream software application with the LLM runtime. It declares variable namespaces, strict data types, version controls, and boundary limits. This ensures clean separation of prompt-logic from raw data payloads, facilitating automated regression testing and automated prompt deployment pipelines.

## Execution Logic (XML-tag)

This section serves as the core operational directive and behavioural control zone of the prompt. It provides the model with specific tasks, structured sequential steps, operational guardrails, and concrete examples to ensure accurate, deterministic, and safe output generation.

### Task

The primary directive and execution command. This is the core operational objective of the prompt. It specifies exactly what action the model must take, defining the functional transformation required to turn the raw input into the desired output. It acts as the anchor for the model's goal-directed attention.

### Steps

The deterministic execution protocol. This provides a structured, sequential workflow for the model to follow during inference. Breaking the transformation down into explicit computational steps forces linear token generation, significantly increasing reasoning accuracy and tracking capabilities for complex tasks.

### Guidelines

The operational constraints, guardrails, and behavioural parameters. This section establishes the boundaries of acceptable execution, including formatting rules, structural limitations, edge-case handling rules, and explicitly forbidden outputs. It serves as the primary mechanism for deterministic behaviour control and safety alignment.

### Edge Cases & Critical Negatives

The deterministic defensive boundary and error-routing protocol. While standard guidelines define the optimal path, this section builds the defensive perimeter against input anomalies, out-of-distribution data, and semantic drift. It establishes explicit fallback states, validation failures, and null-island responses. Architecturally, it stops the model from default-soft-hallucinating when faced with incomplete payload injections, forcing it instead into a pre-defined error state.

Here is the direct comparison between **Guidelines** and **Edge Cases & Critical Negatives**:

|Feature|Guidelines|Edge Cases & Critical Negatives|
|---|---|---|
|**Primary Focus**|**Optimal execution path**; manages the normal boundaries of acceptable behavior.|**Defensive boundaries**; manages anomalies, errors, and data deviations.|
|**Core Function**|Establishes formatting rules, structural limitations, and behavioral parameters.|Establishes explicit fallback states, validation failures, and null-island responses.|
|**System Benefit**|Serves as the primary mechanism for deterministic behavior control and safety alignment.|Stops the model from soft-hallucinating when faced with incomplete payload injections by forcing a pre-defined error state.|
|**Handling Method**|Dictates what the model **must or must not do** during standard execution.|Rules out **out-of-distribution data**, semantic drift, and payload gaps.|

Summary of the Difference:

- **Guidelines** keep the model on the correct track by defining formatting and structural rules for standard, successful inputs.
- **Edge Cases & Critical Negatives** act as an emergency routing system, telling the model exactly how to fail safely or handle broken, weird, or incomplete inputs without hallucinating.

### Examples

The in-context learning optimization layer. This section functions as a non-parametric weights-adjustment mechanism during inference. By providing explicit Input-Reasoning-Output exemplars, you condition the model's token distribution trajectory without architectural fine-tuning. This is the primary tool for alignment, establishing formatting invariants, enforcing structural syntax, and lowering the entropy of the output sequence.

## Runtime Interface (XML-tag)

This final section manages the active data exchange and quality control loop during execution. It defines how dynamic input payloads are injected, configures the internal thinking protocols for complex problem-solving, and enforces strict schema and format rules for the final output.

### Input

The runtime data injection variable. This placeholder acts as the interface for dynamic data payloads, user queries, raw source documents, or API responses. It separates static prompt orchestration logic from dynamic execution data, ensuring clean code-prompt separation and preventing prompt injection vulnerabilities.

### Thinking Protocol

The state-space expansion and hidden-token latency allocation layer. This block explicitly decouples the compute path from the final output payload by enforcing a scratchpad mechanism before target token generation. For native reasoning models, this section configures the orchestration boundaries of the reasoning tokens. For standard LLMs, it expands the computational depth per token, allowing the model to solve complex semantic cross-dependencies before writing to the immutable output stream.

Here is the direct comparison between between **Steps** and a **Thinking Protocol**:

|Feature|Steps|Thinking Protocol|
|---|---|---|
|**Architectural Layer**|**Execution Logic** layer (defines the behavioral workflow).|**Runtime Interface** layer (manages active data exchange and processing).|
|**Core Mechanism**|Breaking the task down into a structured, **sequential workflow** for the model to follow linearly.|Creating a **scratchpad mechanism** or configuring reasoning tokens to expand computational depth before generating the final output.|
|**Primary Purpose**|Forces linear token generation to significantly increase reasoning accuracy and tracking for **complex, multi-stage tasks**.|Decouples the compute path from the final response, allowing the model to **solve complex semantic cross-dependencies** before writing the final payload.|
|**Prompt Necessity**|**Optional**; it can be skipped for basic or trivial tasks.|**Optional**; it is reserved for complex reasoning and advanced problem-solving.|

Summary of the Difference:

- **Steps** outline the deterministic, visible path of execution that the prompt author wants the model to follow stage-by-stage.
- **Thinking Protocol** allocates hidden-token latency or scratchpad space, giving the model the cognitive "room" to figure out the solution internally before it commits to the final, unchangeable output stream.

### Completeness Test

The self-correction and evaluation loop embedded within the prompt. This acts as a localized quality assurance mechanism. It instructs the LLM to run a final validation pass over its own generated output against a set of predefined acceptance criteria before finalizing the token stream.

### Output

The schema, format, and syntax specification for the delivery payload. This dictates the precise structure of the model's response, whether it requires a specific JSON schema, a Markdown layout, or an XML wrapper. It ensures that the generated text can be reliably parsed by downstream applications, APIs, or software systems.

> [!TIP] Prompt Component Hierarchy
> **Critical Sections** form the absolute core of any interaction; they must be included in every prompt to define who the model is, what it must do, what data it processes, and how the response should look. **Optional Sections** (such as metadata, structured steps, and edge cases) can be skipped for basic or trivial tasks, as they are primarily designed for complex logic, software integration, error handling, and strict formatting in production environments.
> 
> **Critical Sections** (Required for all prompts):
> - **Role:** Establishes the persona, tone, and domain expertise.
> - **Task:** The core directive and primary execution command.
> - **Input:** The placeholder for dynamic user queries or runtime data.
> - **Output:** Specifies the final delivery payload, format, or syntax.
> 
> **Optional Sections** (Skippable for basic prompts):
>- **Title & Introduction:** Macro-contextual priming and pipeline registration tags.
> - **Context:** Non-dynamic historical data or system status framing.
> - **Metadata & Parameters:** Application schemas and orchestration boundaries.
> - **Steps:** A sequential workflow for multi-stage computational logic.
> - **Guidelines:** Fine-grained formatting limits and guardrails.
> - **Edge Cases & Critical Negatives:** Defensive error routing and fallback states.
> - **Examples:** Exemplars for in-context formatting optimization.
> - **Thinking Protocol:** Scratchpad token allocation for complex reasoning.
> - **Completeness Test:** Localized self-correction loops embedded in the token stream.

