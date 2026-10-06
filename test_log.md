# Test Log

## Test case

Company: Sarvottam Cables Ltd  
Ticker: SRVCABLE  
Input: 8 documents from `research_pack/`  
Requirement: Produce a short, source-grounded research brief for a retail investor.

---

## Run 1 — Baseline

### Prompt/design
Used the baseline system prompt with the core requirements:
- Use only the supplied research pack.
- Ground factual claims in sources.
- Preserve exact numbers and dates.
- Distinguish company statements from third-party reporting.
- Flag missing or conflicting information.
- Produce Snapshot, Bull case, Bear case, Open questions, and Sources.

### Result
The agent produced a structured research brief with all required sections.

### Observation
The brief was generally well grounded, but conflicting numerical claims required stronger explicit handling to ensure the discrepancy was surfaced rather than silently resolved.

Output:
- `outputs/run1_brief.md`
- `outputs/run1_evidence.json`

---

## Run 2 — Conflict handling

### Prompt/design change
Added an explicit rule:

> For conflicting numerical claims, never silently choose one. Preserve both figures, identify the sources, and flag the discrepancy for the final Open questions section.

### Result
The agent continued to produce the required brief structure and more explicitly preserved conflicting numerical claims.

### Observation
This improved the agent's handling of source disagreement, especially where different sources reported different revenue figures.

Output:
- `outputs/run2_brief.md`
- `outputs/run2_evidence.json`

---

## Run 3 — Retail-investor prioritization

### Prompt/design change
Added a second design rule instructing the agent to prioritize information most relevant to a retail investor:
- recent financial performance
- growth drivers
- order-book strength
- margins
- working-capital risks
- promoter/governance signals
- regulatory/legal risks
- material conflicting information

### Result
The final brief remained concise while emphasizing financially material growth drivers and risks.

### Observation
The output became more focused on decision-relevant information rather than giving equal weight to every source detail.

Output:
- `outputs/run3_brief.md`
- `outputs/run3_evidence.json`

---

## Overall conclusion

The three runs show an iterative prompt-design process:

1. Establish a grounded baseline.
2. Strengthen conflict detection and preservation.
3. Improve prioritization for the target retail-investor audience.

The final design uses an evidence-first two-pass workflow:
1. Extract structured evidence and conflicts from the source pack.
2. Generate the final cited research brief only from that evidence.

This reduces unsupported claims and makes source conflicts visible to the reader.