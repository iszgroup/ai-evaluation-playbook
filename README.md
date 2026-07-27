# AI Evaluation Playbook

> You can't improve what you don't measure. This playbook contains production-ready LLM evaluation rubrics, LLM-as-a-Judge scoring prompts, test dataset templates, and CI/CD quality gate configurations — so your team can tell the difference between a model that got better and one that got different.

![Banner](assets/banner.png)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Maintained by ISZ GROUP](https://img.shields.io/badge/Maintained%20by-ISZ%20GROUP-0066FF.svg)](https://github.com/iszgroup)

---

## Table of Contents

- [Why This Exists](#why-this-exists)
- [Who This Is For](#who-this-is-for)
- [Evaluation Rubrics & Playbooks](#evaluation-rubrics--playbooks)
- [The Scoring Rubric Standard](#the-scoring-rubric-standard)
- [Quick Start: LLM-as-a-Judge Prompt](#quick-start-llm-as-a-judge-prompt)
- [Repository Structure](#repository-structure)
- [Inclusion Criteria](#inclusion-criteria)
- [Contributing](#contributing)
- [FAQ](#faq)
- [License](#license)
- [About ISZ GROUP](#about-isz-group)

---

## Why This Exists

Most teams discover their AI output quality degraded by accident — a user complaint, a failed audit, a regression noticed three sprints too late.

The root cause is the absence of a measurement system. Without evals:

- You can't safely upgrade to a new model version
- You can't tell if a prompt change improved median quality or just shifted the distribution
- You can't demonstrate compliance to an auditor
- You can't prioritize which failure modes to fix first

This playbook gives you the measurement infrastructure. Rubrics you can drop into GitHub Actions. Scoring prompts you can run against 1,000 test cases overnight. Quality gates that block bad model updates before they reach users.

---

## Who This Is For

**Built for:** AI QA Engineers, LLMOps Leads, and Engineering Managers responsible for maintaining AI output quality across model updates, prompt changes, and dataset drift.

**Not for:** Researchers benchmarking models on academic tasks — this is about your production use case, not MMLU.

---

## Evaluation Rubrics & Playbooks

| Rubric | Metric Type | Scoring Scale | Use Case | Document |
| :--- | :--- | :--- | :--- | :--- |
| **Code Generation Accuracy** | LLM-as-a-Judge | 0–5 | Copilot and agent code output | [01-code-gen-evaluation-rubric.md](rubrics/01-code-gen-evaluation-rubric.md) |
| **Hallucination Detection** | LLM-as-a-Judge + Rule | Binary 0/1 | RAG and Q&A systems | [02-hallucination-detection-eval.md](rubrics/02-hallucination-detection-eval.md) |
| **Agent Tool Selection** | Precision + Recall | F1 Score | Function-calling agents | [03-agent-tool-selection-eval.md](rubrics/03-agent-tool-selection-eval.md) |

---

## The Scoring Rubric Standard

All rubrics in this repository use a consistent format:

| Score | Label | Operational Definition |
| :--- | :--- | :--- |
| **5** | Excellent | Fully solves the task, no errors, passes all schema/safety checks |
| **4** | Good | Solves task with minor formatting issues only |
| **3** | Acceptable | Correct intent, notable flaws that require human correction |
| **2** | Poor | Partially correct; likely to mislead a downstream system |
| **1** | Failing | Wrong, hallucinated, or structurally invalid output |
| **0** | Critical Failure | Safety violation, PII leak, or injection success |

A deployment should target **≥ 4.0 average** and **0 Critical Failures** on your test suite.

---

## Quick Start: LLM-as-a-Judge Prompt

```
You are an impartial Code Quality Judge.
Evaluate the AI-generated code snippet below against the user's request.
Score from 1–5 using the rubric provided. Output JSON only.

User Request:
{USER_REQUEST}

Generated Code:
{GENERATED_CODE}

Rubric:
5 = Correct, safe, follows all project conventions
3 = Correct intent, fixable issues
1 = Wrong or dangerous

Output:
{
  "score": 1-5,
  "reasoning": "one sentence",
  "has_security_issue": true | false
}
```

---

## Repository Structure

```
ai-evaluation-playbook/
├── README.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── LICENSE
└── rubrics/
    ├── 01-code-gen-evaluation-rubric.md
    ├── 02-hallucination-detection-eval.md
    └── 03-agent-tool-selection-eval.md
```

---

## Inclusion Criteria

A rubric is accepted if it:

- [ ] Defines operationally unambiguous score criteria (two independent annotators should agree ≥85% of the time)
- [ ] Specifies the failure modes it detects
- [ ] Includes a prompt template that has been tested across at least 3 LLM providers
- [ ] Documents the judge model's known biases for this task type

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Rubric submissions must include inter-rater reliability data or a detailed specification of scoring criteria.

---

## FAQ

**Q: Which model should I use as the judge?**
For code quality: `claude-sonnet` or `gpt-4o`. For safety/hallucination: fine-tuned classifiers (Llama Guard 3) outperform frontier models at lower cost. See individual rubric documents for recommendations.

**Q: How large does my test dataset need to be for evals to be meaningful?**
100 diverse examples is the practical minimum for detecting regressions. 500+ gives you statistical significance for A/B comparisons between model versions.

---

## License

[MIT License](LICENSE).

---

### About ISZ GROUP

> ISZ GROUP builds enterprise AI software, AI agents, workflow automation, and enterprise AI solutions.

- Website: [iszgroup.com](https://iszgroup.com)
- Enterprise AI Platform: [isz.ai](https://isz.ai)
- GitHub: [github.com/iszgroup](https://github.com/iszgroup)
