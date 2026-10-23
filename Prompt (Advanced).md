---
id:
title:
description:
author:
temperature: 0.0
mode: strict-factual
---
See [[Prompt Architecture - User Manual]] for use.

<!-- SYSTEM MESSAGE START -->
<system_priming>
# {{title}}

## INTRODUCTION

{{introduction}}

## ROLE

{{role}}

## CONTEXT

{{context}}

## METADATA & PARAMETERS

{{metadata}}

</system_priming>
<execution_logic>
## TASK

{{task}}

## STEPS

{{steps}}

## GUIDELINES

{{guidelines}}

## EDGE CASES & CRITICAL NEGATIVES

{{error_handling}}

## EXAMPLES

{{examples}}

</execution_logic>
<!-- SYSTEM MESSAGE END -->

<!-- USER MESSAGE START -->
<runtime_interface>
## INPUT

{{inputs}}

## THINKING PROTOCOL

{{thought_process}}

## COMPLETENESS TEST

{{tests}}

## OUTPUT

{{output}}

</runtime_interface>
<!-- USER MESSAGE END -->