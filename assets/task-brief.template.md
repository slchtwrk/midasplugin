# Task brief: <plugin name>

Copy this template to `tasks/<task-name>/brief.md`. Fill in what you know; short
notes and source paths are enough to start. Leave unknowns as `TBD` and omit
inapplicable prompts. Paths are relative to the task folder unless stated
otherwise. Placeholders are not approved defaults.

## Common instructions for the agent

- Follow the workspace task-context policy and use the shared `midasplugin`
  skill for MIDAS implementation guidance.
- Check, do not assume: if the brief or reference material is ambiguous,
  incomplete, unreadable, or contradictory, identify the specific issue and ask
  the user before implementing affected behavior. Do not invent missing
  provisions, factors, defaults, or API payloads.
- Distinguish cited standard requirements, approved project choices, and proposed
  assumptions. Cite the source, clause/table/equation, and page for engineering
  rules. If sources or examples disagree, report the discrepancy rather than
  silently choosing one or adjusting calculations to match.
- Verify equations, subscripts, units, signs, figures, and table headings against
  the supplied material. Flag unreliable PDF/OCR extraction. Request missing
  definitions or cross-referenced provisions when they affect the result.
- Before initial implementation or a material engineering change, summarise the
  interpretation, reproduce relevant worked examples, list open decisions, and
  obtain approval for the affected engineering approach.
- Keep calculations separate from UI and API wiring. Turn approved examples into
  regression tests; also test applicability limits, invalid inputs, and boundary
  cases. Do not claim example agreement proves full compliance.
- Never silently omit invalid inputs, unavailable result series, or failed
  operations. Report actionable errors and distinguish partial completion from
  success.
- Preview model changes before commit. Do not write to a live model or run
  experimental API probes without explicit authorization; use an authorized
  scratch model for verification.
- Report exactly what was tested, expected versus actual results, and remaining
  limitations. Distinguish offline/mock checks from live CIVIL NX verification.
- Respect source licensing and confidentiality. Do not commit licensed source
  documents or credentials, or include them in release archives.
- Develop this brief progressively: read the supplied context first, extract
  supported details with source references, and ask focused questions for gaps
  that affect the next step. Do not require the user to complete an exhaustive
  specification before starting investigation. Record approved decisions and
  changed requirements here so later sessions do not rely on chat history alone.

## 1. Goal and scope

<What should the plugin do? Describe the intended result, key exclusions, and
whether this is a new plugin or a change to an existing one.>

## 2. Context and examples

<List source and worked-example paths. Include the standard edition, relevant
clauses, amendments or National Annex if known, plus any expected results.
Identify existing plugin code if applicable.>

## 3. Intended workflow and constraints

<Briefly describe user inputs and outputs, and whether the plugin reads or writes
to the model. Note any known units, conventions, CIVIL NX version, UI preferences,
or behavior that must be preserved. Leave details for discussion where needed.>

## 4. Success checks

<What would demonstrate that this works? Name useful examples, expected loads or
other outputs, or required deliverables. The agent should propose detailed tests
and tolerances from the context for review.>

## Decisions and next steps

<Initially optional. The agent maintains a concise record of open questions,
approved decisions, validation evidence, and remaining work as the task develops.>
