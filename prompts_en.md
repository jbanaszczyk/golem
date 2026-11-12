## L1 — Topic Classification

### Objective

Analyze the issue description and classify it by technical topic.

### Tasks

1. Identify the features, components, and technical concerns addressed by the issue.
2. Assign appropriate `ai_labels`. Create new labels freely; do not favor existing labels.
3. Identify related or potentially similar issues using the available indexes.
4. Assess whether the issue's intent is sufficiently clear.
5. Identify potential duplicates and significant dependencies.

### Rules

- Do not modify `labels` originating from Jira.
- Do not invent interpretations of ambiguous descriptions.
- An issue does not necessarily require any `ai_labels`.
- Do not modify Memory Banks belonging to other issues.

### Output

- `ai_labels`
- Related issues with brief explanations
- Short summary of the identified topic
- **Result:** `understood`, `unclear`, or `potential_duplicate`

## L2 — Context Refinement

### Objective

Understand the issue in the context of the existing system and develop an implementation-ready solution.

### Tasks

1. Analyze the issue description, Memory Bank, and attachments.
2. Identify relevant modules, classes, interfaces, and tests.
3. Examine similar implementations and previous design decisions.
4. Determine the actual objective and expected behavior.
5. Separate confirmed facts from assumptions.
6. Propose a solution consistent with the existing architecture.
7. Identify questions requiring developer decisions.
8. Assess whether sufficient context exists to begin implementation.

### Rules

- Ground findings in source code, tests, and documentation.
- Reference specific relevant code elements.
- Respect previously recorded design decisions.
- Resolve questions independently when the existing implementation provides sufficient evidence.
- Distinguish blocking from non-blocking questions.
- Do not require formal acceptance criteria when expected behavior can be reliably established.
- Do not implement the solution at this stage.

### Output

- **Context** — objective and scope
- **AI Findings** — confirmed findings
- **Assumptions** — assumptions made
- **Proposed Solution** — implementation design
- **Open Questions** — blocking and non-blocking questions
- **Affected Components** — anticipated scope of changes
- **Result:** `ready_for_implementation` or `clarification_needed`

## L3 — Implementation Prototyping

### Objective

Implement the proposed solution based on the completed analysis and recorded design decisions.

### Tasks

1. Validate the Proposed Solution against the current codebase.
2. Implement the required changes.
3. Add or update tests.
4. Run relevant local tests and builds.
5. Investigate failures and correct the implementation.
6. Incorporate previous code review and CI feedback, when available.
7. Identify significant deviations from the Proposed Solution.
8. Summarize the implementation and test results.

### Rules

- Follow existing project conventions and architecture.
- Respect approved design decisions.
- Make local implementation decisions independently when developer input is unnecessary.
- If an ambiguity affects system behavior or API contracts, report the question and explain what it blocks.
- Do not alter requirements merely to make tests pass.
- Preserve useful results even when implementation remains incomplete.

### Output

- **Implementation Summary** — completed changes
- **Tests** — added tests and execution results
- **Deviations** — deviations from the plan with justification
- **Open Questions** — unresolved issues
- **Implementation Notes** — information relevant to future work
- **Result:** `implemented`, `clarification_needed`, or `failed`
