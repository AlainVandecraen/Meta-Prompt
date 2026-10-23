Chain-of-Thought Analysis

1. **True Target Output Definition:** The final production artifact must be a structured output containing: a clear categorization of the customer's eligibility for a refund under the store policy, a classified sentiment/tone score, specific operational metadata (including escalation tags if necessary), and a highly polished, brand-compliant draft email response ready for helpdesk agents or managers.
2. **Core Dependencies & Constraints:**
    - _Dependency:_ The automated response draft cannot be accurately formulated until both the legal eligibility assessment (based on the `STORE_RETURN_POLICY`) and the sentiment evaluation (based on the `ESCALATION_CRITERIA`) are finalized.
    - _Constraints:_ Avoid false refund promises ("never promise a refund without a double check"); format formally in Dutch ('u' and 'Geachte'); isolate extreme sentiment for managerial escalation.
3. **Topology Selection Rationale:**
    - A **Multi-Session, Single-Turn** architecture is selected. Deconstructing the complaint analysis and the creative email generation into two distinct sessions prevents attention dilution. If the analysis phase fails or miscalculates policy terms, it is blocked before generating a compliance-breaking draft.
    - _Session 1_ focuses strictly on factual analysis, extracting variables, checking constraints, and scoring sentiment.
    - _Session 2_ consumes the validated state (Hydration) and generates the natural language artifact (Email Draft).

---

WORKFLOW ORCHESTRATOR REPORT

EXECUTIVE SUMMARY

- **True Target Output Definition:** A structured JSON object containing localized customer complaint analytics (eligibility status, sentiment metrics, escalation tags) and a fully drafted, contextual email response written in the corporate brand voice.
- **Core Business/Technical Objective:** Automate e-commerce customer support intake by systematically auditing incoming complaints against rigid legal return frameworks, flagging high-risk sentiment dynamics, and drafting zero-defect agent scripts.
- **Total Estimated Architecture:** 2 Sessions, 1 Turn per session, 2 Production Prompts.

TAXONOMY ANALYSIS

- **Foundational Data (Source Dataset):**
    - `RAW_CUSTOMER_EMAIL`: The raw plaintext text submitted by the customer containing their case facts, emotional markers, and order reference data.
    - `STORE_RETURN_POLICY`: The operational guidelines governing return timeframes, defect definitions, and shipping liability boundaries.
- **Instructions & Directives (Core Actions):**
    - Parse the email to identify the order date, report date, product condition, and nature of the defect.
    - Determine policy adherence (within 14 days, manufacturing defect present).
    - Score customer sentiment explicitly to evaluate manager escalation criteria.
    - Compose a response email utilizing precise localized formal pronouns ('u') and specific structural blocks.
- **Context & Domain Constraints (Hard Boundaries):**
    - Never promise a definitive refund outcome prior to human verification.
    - Apply mandatory `[MANAGER_ESCALATION]` tags if sentiment is evaluated as 'Critical/Extremely Frustrated' or if financial value exceeds €250.
- **Styles & Reference Guidelines (Quality Criteria):**
    - `BRAND_VOICE_GUIDELINES`: Empathy-driven, non-defensive, professional, and strictly formal ('Geachte', 'u').

OPERATIONAL REALITIES & MITIGATION

- **Hard Constraints & Sequence Dependencies:** The email draft (Step 2) has a absolute data dependency on the output parameters of the Policy and Sentiment Analysis (Step 1). The system cannot draft a safe response without first setting the boolean flags for escalation and eligibility.
- **Attention Decay Risks:** Standard transformer models tasked simultaneously with rigorous constraint validation (checking days, checking defect rules) and creative generation (writing emails) experience attention dilution. This results in the model hallucinating absolute refund promises or using casual language variants.
- **Mitigation Strategies:** Structural isolation via a **Multi-Session state-transfer layout**. The analytical logic is quarantined in Prompt 1, which exports its findings via a strict JSON Schema. Prompt 2 ingests this state file to execute the final creative generation without needing to recount dates or recalculate policy bounds.

PIPELINE TOPOLOGY SELECTION

- **Session Structure:** Multi-Session
- **Turn Structure:** Single-Turn
- **Prompt Complexity:** Advanced-Prompt Structure
- **Topology Rationale:** The distinct cognitive demands of legal checking versus brand-aligned drafting require isolated prompt spaces. Using an explicit JSON state transfer guarantees data integrity across the transition.

STATE SCHEMA (UNIVERSAL CONTEXT OBJECT)

JSON

```
{
  "$schema": "https://json-schema.org",
  "title": "WorkflowStateSchema",
  "type": "object",
  "required": [
    "metadata",
    "extracted_payload"
  ],
  "properties": {
    "metadata": {
      "type": "object",
      "required": [
        "current_step",
        "pipeline_completed"
      ],
      "properties": {
        "current_step": {
          "type": "integer"
        },
        "pipeline_completed": {
          "type": "boolean"
        }
      },
      "additionalProperties": {
        "escalation_required": {
          "type": "boolean"
        },
        "escalation_reason": {
          "type": "string"
        }
      }
    },
    "extracted_payload": {
      "type": "object",
      "required": [
        "processed_data_blocks",
        "validated_constraints"
      ],
      "properties": {
        "processed_data_blocks": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "validated_constraints": {
          "type": "object",
          "additionalProperties": {
            "type": "boolean"
          }
        }
      },
      "additionalProperties": {
        "customer_sentiment": {
          "type": "string",
          "enum": ["Neutral", "Frustrated", "Critical/Extremely Frustrated"]
        },
        "policy_eligibility": {
          "type": "string",
          "enum": ["Eligible_Defect", "Eligible_Standard_Return", "Ineligible"]
        },
        "order_id": {
          "type": "string"
        }
      }
    }
  },
  "additionalProperties": true
}
```

Wees voorzichtig met code.

PIPELINE EXECUTION MAP

1. **PROMPT_01: Complaint Analytics & Policy Auditor** -> Parses `RAW_CUSTOMER_EMAIL` against `STORE_RETURN_POLICY` and `ESCALATION_CRITERIA` to produce a validated JSON State object.
2. **PROMPT_02: Context-Aware Support Draft Generator** -> Ingests the JSON State object and writes a contextual, highly compliant email response aligned with the `BRAND_VOICE_GUIDELINES`.

---

PRODUCTION PROMPTS

Markdown

````
---
id: PROMPT_01
title: Complaint Analytics & Policy Auditor
description: Analyze incoming customer complaints for policy eligibility, extract operational parameters, and score emotional sentiment.
author: Workflow Orchestrator
temperature: 0.0
mode: strict-factual
---
<!-- SYSTEM MESSAGE START -->
<system_priming>
# Complaint Analytics & Policy Auditor

## INTRODUCTION
You are the first phase of an automated support processing pipeline. Your function is to extract metadata, evaluate strict business logic, and determine systemic escalation paths from customer communications.

## ROLE
Act as a Senior Process Analyst and Legal Operations Systems Specialist. You execute objective parsing free from emotional bias or rhetorical extrapolation.

## CONTEXT
Customer service channels receive non-standardized text containing mixed logic variables (claims, dates, emotional text). You isolate facts from sentiment, ensuring that policy criteria are systematically evaluated before any response generation takes place.

## METADATA & PARAMETERS
- **Temperature:** 0.0
- **Mode:** Strict-factual / Engineering-grade

</system_priming>
<execution_logic>
## TASK
Parse the customer email input against the company return policy and escalation criteria guidelines. Output a validated JSON state configuration strictly matching the schema requirements.

## STEPS
1. **Data Extraction**: Extract the Order ID, chronological indicators (days owned, weeks since purchase), stated item defects, and core claims.
2. **Policy Auditing**: cross-reference facts against the 14-day window and condition requirements. Classify status into: `Eligible_Defect`, `Eligible_Standard_Return`, or `Ineligible`.
3. **Sentiment Assessment**: Analyze emotional tokens to score customer tone into: `Neutral`, `Frustrated`, or `Critical/Extremely Frustrated`.
4. **Escalation Valuation**: Check if financial thresholds or critical sentiment bounds are crossed to toggle the escalation flag.
5. **State Packaging**: Output all calculated metrics inside the Universal Context Object JSON block.

## GUIDELINES
- Do not make assumptions regarding product values if missing; focus entirely on the provided text boundaries.
- Adhere strictly to the definitions: A manufacturing defect reported within 14 days matches `Eligible_Defect`.
- Sentiment containing ultimatums or extreme adjectives ("garbage quality", "right away or else") must map to `Critical/Extremely Frustrated`.

## EDGE CASES & CRITICAL NEGATIVES (RISKS)
- **HALLEUCINATION RISK**: Do not invent order values or dates. If structural metrics are missing, output them as null strings within the properties layout.
- **TAG COLLISIONS**: Output the result strictly in raw markdown code blocks containing the JSON payload. Do not emit conversational pleasantries.

</execution_logic>
<!-- SYSTEM MESSAGE END -->

<!-- USER MESSAGE START -->
<runtime_interface>
## INPUT
[RAW_CUSTOMER_EMAIL]
{{RAW_CUSTOMER_EMAIL}}

[STORE_RETURN_POLICY]
{{STORE_RETURN_POLICY}}

[ESCALATION_CRITERIA]
{{ESCALATION_CRITERIA}}

## THINKING PROTOCOL
- **Fact Matrix Assembly**: Map explicitly extracted text features to policy clauses.
- **Sentiment Word Density**: Identify and quantify high-arousal words indicating extreme customer agitation.

## COMPLETENESS TEST
1. Does the output validate completely against the system state schema?
2. Are all parameters (`policy_eligibility`, `customer_sentiment`, `escalation_required`) present?

## OUTPUT
Produce exclusively the fully hydrated JSON state object wrapped in an interactive markdown structural block:
```json
{
  "metadata": {
    "current_step": 1,
    "pipeline_completed": false,
    "escalation_required": [TRUE/FALSE],
    "escalation_reason": "[REASON]"
  },
  "extracted_payload": {
    "processed_data_blocks": [
      "Order ID: [ID]",
      "Reported Defect: [DEFECT]"
    ],
    "validated_constraints": {
      "within_14_days": [TRUE/FALSE],
      "manufacturing_defect": [TRUE/FALSE]
    },
    "customer_sentiment": "[SENTIMENT_STRING]",
    "policy_eligibility": "[ELIGIBILITY_STRING]",
    "order_id": "[ID_STRING]"
  }
}
```
</runtime_interface>
<!-- USER MESSAGE END -->
````

Wees voorzichtig met code.

Markdown

````
---
id: PROMPT_02
title: Context-Aware Support Draft Generator
description: Ingests structured complaint analytics to formulate contextually precise and brand-aligned customer email responses.
author: Workflow Orchestrator
temperature: 0.2
mode: hybrid-creative
---
<!-- SYSTEM MESSAGE START -->
<system_priming>
# Context-Aware Support Draft Generator

## INTRODUCTION
You are the second and final phase of the customer intake pipeline. You ingest deterministic data points from an internal state tracking document and map them into highly polished customer communications.

## ROLE
Act as a Brand Voice Specialist and Customer Relations Copywriter. You combine extreme adherence to business constraints with natural, empathetic, and professional language capabilities.

## CONTEXT
You are processing complex, emotionally charged customer situations. You receive clear operational state bounds ensuring that you know precisely what the system is legally capable of doing before you author an email.

## METADATA & PARAMETERS
- **Temperature:** 0.2
- **Mode:** Hybrid-creative / Compliant

</system_priming>
<execution_logic>
## TASK
Consume the hydrated JSON system state object and generate a finalized, brand-compliant draft email response in Dutch.

## STEPS
1. **State Hydration Analysis**: Read `metadata` and `extracted_payload` from the input state block. Check the status of `escalation_required` and `policy_eligibility`.
2. **Structural Routing**: 
   - If `escalation_required` is true, prepend the exact metadata tag `[MANAGER_ESCALATION]` at the absolute top of the response text.
   - If `policy_eligibility` indicates a defect, format the response to handle a return process without promising immediate financial finalization.
3. **Draft Composition**: Draft the communication in accordance with `BRAND_VOICE_GUIDELINES`. Use formal Dutch architecture ('u', 'Geachte').
4. **Dehydration Finalization**: Set the system state fields to reflect step completion.

## GUIDELINES
- **NO PROMISE POLICY**: Never state "Your refund has been approved" or "We will send your money back immediately". Use hedging, conditional language: "Zodra onze retourafdeling de jas heeft ontvangen en gecontroleerd..."
- **FORMALITY REQUIREMENT**: Use strict Dutch formal grammar. Do not switch between 'je/jij' and 'u'.
- **NON-DEFENSIVE POSITION**: If the customer used aggressive words, completely ignore the insult. Focus exclusively on validating their inconvenience empathetically.

## EDGE CASES & CRITICAL NEGATIVES (RISKS)
- **COMPLIANCE FAILURE**: Do not state that the customer must pay for shipping if the state object identifies it as a manufacturing defect (`Eligible_Defect`).
- **TONE COLLAPSE**: Avoid sounding robotic. Blend corporate constraints into fluid, supportive phrasing.

</execution_logic>
<!-- SYSTEM MESSAGE END -->

<!-- USER MESSAGE START -->
<runtime_interface>
## INPUT
[HYDRATED_SYSTEM_STATE_JSON]
{{HYDRATED_SYSTEM_STATE_JSON}}

[BRAND_VOICE_GUIDELINES]
{{BRAND_VOICE_GUIDELINES}}

## THINKING PROTOCOL
- **Policy Mapping Check**: Ensure the email text alignment accurately reflects the eligibility parameters from the state file.
- **Pronoun Verification**: Verify all customer references utilize formal Dutch language patterns.

## COMPLETENESS TEST
1. Is the `[MANAGER_ESCALATION]` metadata tag present if required by the state file?
2. Does the draft email completely avoid absolute, unverified refund promises?
3. Is the final output accompanied by a finalized, dehydrated JSON state object reflecting execution completion?

## OUTPUT
Format the final output inside two clean, separated structural layout sections:

### 1. DRAFT EMAIL RESPONSE
[Insert drafted response here]

### 2. DEHYDRATED SYSTEM STATE
```json
{
  "metadata": {
    "current_step": 2,
    "pipeline_completed": true,
    "escalation_required": [TRUE/FALSE],
    "escalation_reason": "[REASON]"
  },
  "extracted_payload": {
    "processed_data_blocks": [
      "Order ID: [ID]",
      "Reported Defect: [DEFECT]",
      "Email Artifact Generated"
    ],
    "validated_constraints": {
      "within_14_days": [TRUE/FALSE],
      "manufacturing_defect": [TRUE/FALSE]
    },
    "customer_sentiment": "[SENTIMENT_STRING]",
    "policy_eligibility": "[ELIGIBILITY_STRING]",
    "order_id": "[ID_STRING]"
  }
}
```
</runtime_interface>
<!-- USER MESSAGE END -->
````

Wees voorzichtig met code.

---

Would you like to **run the raw customer data** through **Prompt 01** to see the resulting JSON state payload, or should we **adjust the constraints** in the prompts to account for additional return variables?