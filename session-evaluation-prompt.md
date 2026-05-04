## Session Evaluation Prompt

```
You are now operating in Session Evaluation Mode. Your task is to review the entire conversation that just occurred and produce a structured Session Evaluation Summary. Do not continue the prior task. Do not add new ideas or suggestions. Only summarize and evaluate what was done.

Output the summary using the exact section headers and structure below. Fill in each section with concise, objective descriptions.

---

**Session ID:** [Generate a unique ID based on today’s date and a short topic slug, e.g., 2026-04-29-digital-bill-of-rights]

**Date / Duration:** [Date of the session]; prompter active ≈ [estimate total hours the prompter spent reading, thinking, and writing]

**Project / Context:**
[One paragraph describing the overall task and domain.]

**Top-Level Component:**
[The primary deliverable or highest-level output produced during this session.]

**Second-Level Modules:**
[List each distinct sub-component, module, or section that was created or materially advanced. Use bullet points with short descriptors.]

**Prompter Contributions:**
[Summarize the human’s input: what they directed, decided, corrected, strategized, or contributed substantively. Focus on active, decision-making contributions, not passive receipt.]

**Model Contributions:**
[Summarize the AI’s output: what was produced, analyzed, drafted, structured, or advised upon. Include strategic, legal, technical, and procedural domains as applicable.]

**Prompter Time Estimate:**
- Reading and digesting model responses: ~[X] hours
- Thinking, strategizing, and weighing options: ~[Y] hours
- Writing messages and directives: ~[Z] hours
- **Total: [sum] hours** (cumulative, likely over several sittings)

**Model-Equivalent SME Time Estimate:**
[Estimate total hours a subject-matter expert or team would need to produce equivalent analysis and drafting. Include a brief breakdown of how the hours would be distributed across major tasks.]

**Required SME Expertise:**
[List the specific fields of expertise that would be required to replicate the model’s contributions. Use bullet points with short descriptors.]

**Aggregation Tags:**
[Provide 5–12 keyword tags, comma-separated, that capture the domain, activities, and outputs of the session for aggregation across multiple sessions.]

---

**Instructions for estimation:**
- When estimating prompter time, review the length and complexity of the prompter’s messages and the model’s responses the prompter had to read. Assume a careful reading pace of ~250 words per minute with additional time for technical comprehension and strategic thought.
- When estimating SME time, assume a highly skilled professional working efficiently but requiring time for research, thinking, drafting, and revision. Be specific about the tasks that drive the estimate.
- When listing SME expertise, be granular—name specific legal, technical, or strategic domains, not broad categories like “law” or “technology.”
- Maintain a neutral, evaluative tone throughout.
```
