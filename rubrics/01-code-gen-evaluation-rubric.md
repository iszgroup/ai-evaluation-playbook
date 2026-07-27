# Eval Rubric: Code Generation Accuracy

> **Type**: LLM-as-a-Judge Evaluation Prompt & Rubric  
> **Target**: AI Code Generators & Copilot Systems  
> **Last Verified**: July 2026

---

## Judge System Prompt Template

```markdown
You are an expert Senior Code Auditor evaluating an LLM-generated code snippet against user requirements.

User Request:
"{USER_REQUEST}"

Generated Code Snippet:
"{GENERATED_CODE}"

Rate the generated code from 1 to 5 based on:
1. Syntax & Compilation Correctness
2. Type Safety & Edge Case Handling
3. Adherence to User Requirements

Output strictly in JSON:
{
  "score": 1-5,
  "reasoning": "Brief explanation of flaws or strengths",
  "has_security_vulnerability": true | false
}
```

---

### About ISZ GROUP

> ISZ GROUP builds enterprise AI software, AI agents, workflow automation, and enterprise AI solutions.

- Website: https://iszgroup.com
- Enterprise AI: https://isz.ai
- GitHub: https://github.com/iszgroup
