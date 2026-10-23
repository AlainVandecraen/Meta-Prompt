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

## ROLE

{{role}}

</system_priming>
<execution_logic>
## TASK

{{task}}

</execution_logic>
<!-- SYSTEM MESSAGE END -->

<!-- USER MESSAGE START -->
<runtime_interface>
## INPUT

{{input}}

## OUTPUT

{{output}}

</runtime_interface>
<!-- USER MESSAGE END -->