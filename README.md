# LLM Guardrail Bypass Testing

## Overview

This project documents an authorized security research exercise investigating the resilience of a locally hosted Large Language Model (LLM) against adversarial prompting and guardrail-bypass techniques.

The objective was to understand how prompt construction, contextual manipulation, and in-context examples can influence model behaviour and potentially cause a model to produce responses that should have been restricted by its safety controls.

The assessment was conducted against a locally hosted model using Ollama, providing an isolated environment for experimentation.

---

## Research Objective

The primary objective of this assessment was to:

* Understand how LLM safety guardrails respond to adversarial prompts.
* Explore prompt-based techniques that can alter model behaviour.
* Identify conditions under which safety restrictions may fail.
* Document reproducible model behaviour.
* Develop a foundation for future LLM red-team and AI security assessments.

This project is part of a broader learning path toward AI/LLM security research, AI red teaming, AI bug bounty research, and secure adoption of AI-powered workflows.

---

## Environment

| Component             | Configuration                 |
| --------------------- | ----------------------------- |
| Operating Environment | Local development environment |
| Interface             | Jupyter Notebook              |
| Programming Language  | Python                        |
| LLM Runtime           | Ollama                        |
| Model                 | `dolphin-llama....`           |
| API Interface         | OpenAI-compatible local API   |
| API Endpoint          | `http://localhost:...../v1`   |

The model was accessed locally through Ollama rather than through a production organization's infrastructure.

---

## Testing Methodology

The assessment explored several categories of adversarial prompting.

### 1. Context Manipulation

The model was presented with altered scenarios intended to change the context in which a request was interpreted.

Examples of concepts investigated included:

* Alternate scenarios
* Fictional contexts
* Research-oriented framing
* Role/context manipulation

The purpose was to determine whether changing the surrounding context affected the model's safety behaviour.

---

### 2. Rephrasing and Semantic Manipulation

Requests were reformulated to determine whether changing wording or framing could influence the model's response.

This included investigating whether a restricted request could receive a different response when presented as:

* Academic research
* Historical analysis
* Hypothetical discussion
* Educational analysis

---

### 3. Few-Shot / In-Context Manipulation

The assessment also explored whether providing examples inside a prompt could influence the model's subsequent behaviour.

The experiment investigated whether carefully constructed examples could cause the model to follow an undesired response pattern.

---

## Technical Implementation

The model was accessed using Python and the Ollama runtime.

The notebook demonstrates both an OpenAI-compatible client configuration and direct interaction with the Ollama Python client.

Example configuration:

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:.../v1",
    api_key="ollama"
)
```

The model used during the assessment was:

```text
dolphin-llama...
```

---

## Observed Behaviour

During testing, the model demonstrated that adversarial prompting could result in unsafe behaviour.

In one test, the model generated actionable harmful content in response to a request that should have triggered a safety refusal.

This demonstrated that model behaviour can vary significantly depending on:

* Model architecture
* Training
* Alignment
* Safety configuration
* Prompt construction
* Context
* In-context examples

The result demonstrates the importance of evaluating LLM safety controls rather than assuming that a model's stated safety behaviour will always hold under adversarial conditions.

---

## Security Significance

The experiment demonstrates an important principle in LLM security:

> A model's normal behaviour does not necessarily represent its behaviour under adversarial interaction.

For organizations deploying LLMs, relying solely on model-level safety alignment may therefore be insufficient.

A secure AI deployment should consider multiple defensive layers, including:

* Input validation
* Prompt isolation
* System-prompt protection
* Output validation
* Content filtering
* Tool permission boundaries
* Least-privilege access
* Monitoring and logging
* Human approval for high-risk actions
* Adversarial testing
* Continuous red-team evaluation

---

## Finding

### Finding: Guardrail Bypass Through Adversarial Prompting

**Severity:** High — model-dependent

**Category:** LLM Safety / Jailbreak / Prompt Manipulation

**Affected Component:** Local LLM inference

**Model Tested:** `dolphin-llama...`

### Description

The tested model was susceptible to adversarial prompting that caused it to provide content that should have been restricted by a safety-oriented deployment.

The issue demonstrates that prompt-level manipulation can influence the model's safety behaviour.

### Impact

If similar weaknesses exist in an AI system connected to business workflows, the consequences could extend beyond inappropriate text generation.

Depending on the application's architecture, an attacker could potentially attempt to:

* Circumvent application policies
* Extract protected information
* Manipulate model instructions
* Influence downstream decisions
* Abuse connected tools
* Trigger unauthorized workflow actions
* Generate unsafe content

The actual impact depends on the privileges and integrations available to the AI application.

### Recommendation

Organizations deploying LLM applications should implement defense-in-depth rather than relying exclusively on the underlying model's alignment.

Recommended controls include:

1. Treat all user-controlled input as untrusted.
2. Separate system instructions from user-controlled content.
3. Validate model outputs before executing downstream actions.
4. Apply least-privilege permissions to AI agents and tools.
5. Introduce approval gates for high-impact actions.
6. Log and monitor suspicious model interactions.
7. Perform recurring adversarial testing.
8. Test both direct and indirect prompt-injection scenarios.
9. Evaluate the complete AI application, not only the underlying model.

---

## Limitations

This experiment was conducted against a locally hosted model in a controlled research environment.

The observed behaviour should therefore not be interpreted as evidence that every LLM or AI application is vulnerable to the same technique.

Model behaviour can vary according to:

* Model version
* Fine-tuning
* System prompts
* Safety configuration
* Runtime
* Sampling parameters
* Application architecture
* External guardrails

Further testing against additional models is required to establish broader conclusions.

---

## Ethical Considerations

This project was conducted in a controlled environment for security research and education.

Testing of third-party AI systems should only be performed where explicit authorization exists, including through an applicable bug bounty or security research program.

The objective of this research is to improve the security and resilience of AI systems rather than to enable harmful use.

---
