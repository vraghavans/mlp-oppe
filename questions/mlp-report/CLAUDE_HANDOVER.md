# Claude Handover Instructions — MLP-OPPE Question Intelligence

## Purpose

You are taking over the MLP-OPPE question-bank work for the repository:

https://github.com/vraghavans/mlp-oppe

The objective is to use the extracted MLP-OPPE material to build the OPPE application without losing fidelity to the original practical questions.

The source material consists of Jupyter notebooks from the supplied `mlp-oppe-main.zip`. The extraction process has already been completed as a first structured pass.

Do NOT start by re-extracting the notebooks blindly. First read the files under:

`questions/mlp-report/`

Especially:

- `README.md`
- `question_bank.json`
- `question_patterns.json`
- `source_questions.json`
- `generation_rules.md`

These files are the working knowledge layer for the application.

---

# 1. What Was Done

The original repository/notebook set was treated as the authoritative source.

18 Jupyter notebooks were processed.

The extraction deliberately used more than simple keyword extraction.

For each notebook, the analysis considered:

1. Markdown question headings.
2. Numbered question blocks.
3. Standalone question-form Markdown.
4. Common instructions surrounding a question.
5. Nearby Python code cells.
6. Model definitions.
7. Hyperparameters.
8. Dataset and target information.
9. Train/test split information.
10. `random_state`.
11. Scoring metrics.
12. Requested precision.
13. Code required to reproduce the answer.

A Markdown cell was NOT automatically treated as a separate question.

A question could span several cells. Common instructions and context were attached to the appropriate question.

This produced:

- 283 source question units.
- 250 canonical question records.
- 82 hierarchical patterns.

The difference between these numbers is intentional.

Repeated or near-identical source questions were consolidated into canonical questions, while their original source records were retained for provenance.

---

# 2. Three-Layer Architecture

The intended architecture is:

SOURCE LAYER
    ↓
CANONICAL KNOWLEDGE LAYER
    ↓
APPLICATION / CLAUDE LAYER

## Source layer

File:

`source_questions.json`

Contains original question material and provenance.

This is the audit layer.

It should be used when you need to:

- verify wording
- inspect the original question
- investigate an ambiguity
- check source provenance
- compare variants

Do NOT normally send the entire source layer to Claude for question generation.

## Canonical knowledge layer

Files:

`question_bank.json`

and

`question_patterns.json`

This is what the application should normally retrieve for Claude.

Claude should receive the relevant canonical question/pattern, not the entire source repository.

## Application layer

The OPPE app retrieves the relevant canonical records and gives Claude only the context needed for the current task.

---

# 3. Canonicalization Rules

The canonicalization was intentionally CONSERVATIVE.

Do not generalize away technical details.

For example, these are important:

- dataset
- target column
- preprocessing
- model
- solver
- scoring metric
- hyperparameter
- hyperparameter range
- train/test split
- random state
- requested precision
- calculation method

If a source question says:

`random_state=64`

do not silently remove it.

If the source specifies:

`scoring='recall'`

do not replace it with accuracy.

If the source specifies:

`solver='sag'`

preserve it.

The goal is to clean and structure the question, NOT to rewrite its technical meaning.

---

# 4. Hierarchical Patterns

Questions are grouped at two levels.

Example:

Broad pattern:

`DECISION_TREE → GRIDSEARCH → MAX_DEPTH`

Canonical variants can then remain separate:

- max_depth using accuracy
- max_depth using F1
- max_depth with a particular parameter grid
- max_depth on a particular dataset

Do NOT merge technically different questions simply because they test the same general skill.

If changing a parameter can change the answer or task, retain the distinction.

---

# 5. Source Variants

Original variants remain in the source/provenance layer.

Claude normally receives only the canonical version.

For example, source material might contain:

- "Calculate the recall..."
- "Find the recall score..."
- "What is the recall..."

These can map to one canonical pattern.

The source wording must remain available in `source_questions.json`.

Do not delete source evidence merely because it has been canonicalized.

---

# 6. Question Schema

The canonical records use a complete structured representation.

Important fields include:

- question_id
- pattern_id
- topic
- subtopic
- algorithm
- question
- context
- parameters
- asks_for
- answer_type
- precision
- source_evidence
- answer
- generation_constraints

There is intentionally NO difficulty field.

Do not introduce an inferred difficulty level unless the user explicitly changes this requirement.

---

# 7. Answer Verification

Answer provenance is extremely important.

Never assume that a printed answer in a notebook is automatically independently verified.

Possible statuses include:

- `verified`
- `calculable`
- `requires_execution`
- `insufficient_source_data`
- `source_stated_unverified`

Use the strongest status justified by evidence.

A source notebook answer should not be called "verified" merely because it appears in the notebook.

Where the original dataset and executable code are available, reproduce the calculation/model where practical.

Where reconstruction is possible but the original execution environment is unavailable, clearly label it as reconstructed/calculable rather than source-verified.

Never invent missing answers.

---

# 8. Generation Rules for Claude

When generating a new OPPE practice question:

1. Start from a canonical question/pattern.
2. Preserve the algorithm.
3. Preserve the dataset semantics.
4. Preserve the target.
5. Preserve relevant preprocessing.
6. Preserve relevant hyperparameters.
7. Preserve scoring criteria.
8. Preserve random_state when it matters.
9. Do not invent nonexistent dataset columns.
10. Do not change a metric simply to make the question easier.
11. Do not turn a regression task into classification or vice versa.
12. Keep generated variants technically executable.
13. Clearly mark generated questions as generated variants.
14. Keep a link/reference to the source canonical question.

Generated questions must NOT be presented as original OPPE questions.

Use an explicit origin such as:

`source`

or

`generated_variant`

---

# 9. Student-Facing Questions

The student should NOT normally see all the internal metadata.

The application can show:

- question number
- question text
- required inputs
- answer format
- precision requirement
- relevant instructions

Do not expose internal provenance or generation constraints unless required.

---

# 10. Evaluation

When evaluating a student's answer:

1. Retrieve the canonical question.
2. Retrieve its expected answer/reference solution if available.
3. Respect the specified precision/tolerance.
4. Recognize mathematically equivalent numeric answers.
5. Do not change the canonical answer during evaluation.
6. If the answer is not established, do not fabricate one.
7. Explain why an answer is correct/incorrect when feedback is requested.

For code-based questions, evaluation should consider whether the student's implementation satisfies the actual task, not merely whether it resembles the source code.

---

# 11. Do Not Send the Entire Question Bank to Claude

The application should retrieve only relevant records.

Preferred flow:

User selects:
    topic/model/question type
        ↓
Application retrieves matching patterns
        ↓
Application retrieves canonical question(s)
        ↓
Claude receives relevant JSON
        ↓
Claude generates/evaluates
        ↓
Application stores the result

This reduces token usage and makes Claude's behavior more deterministic.

---

# 12. When Modifying the Question Bank

If you discover an error:

1. Check the canonical question.
2. Check `source_questions.json`.
3. Check the original notebook.
4. Determine whether the issue is extraction, canonicalization, or answer verification.
5. Correct the canonical record without destroying source provenance.
6. Update the relevant pattern if necessary.
7. Document material corrections.

Never overwrite source evidence simply to make the canonical question look cleaner.

---

# 13. Important Distinction: Extraction vs Generation

The original extraction answers:

"What did the OPPE source material actually contain?"

The generation system answers:

"How can Claude create another valid practice question based on that source pattern?"

These are different operations.

Never modify source questions to make them better generation prompts.

Keep:

SOURCE QUESTION
    separate from
CANONICAL QUESTION
    separate from
GENERATED VARIANT

---

# 14. Current Repository Contract

The working files are:

`questions/mlp-report/README.md`
    Methodology and architecture.

`questions/mlp-report/question_bank.json`
    Canonical Claude-facing question records.

`questions/mlp-report/question_patterns.json`
    Hierarchical patterns and generation constraints.

`questions/mlp-report/source_questions.json`
    Original source question/provenance layer.

`questions/mlp-report/generation_rules.md`
    Detailed generation/evaluation rules.

This directory is the current source of truth for the OPPE question-intelligence layer.

---

# 15. What You Should Do Next

When continuing development:

1. Read all five files in `questions/mlp-report/`.
2. Understand the schema before changing the app.
3. Inspect the existing OPPE application architecture.
4. Identify where questions are currently stored/retrieved.
5. Replace ad-hoc question handling with retrieval from the canonical question layer where appropriate.
6. Keep source provenance available.
7. Implement Claude prompts around the canonical schema.
8. Implement answer evaluation using the answer-status rules.
9. Do not blindly regenerate the question bank.
10. Before changing any question, verify against source evidence.

The priority is:

FIDELITY > CONSISTENCY > GENERATION FLEXIBILITY

The app should reproduce the technical nature of real MLP-OPPE questions before attempting creative question generation.

---

# 16. Final Rule

When uncertain about what a question means, do not guess.

Trace:

canonical question
    ↓
pattern
    ↓
source evidence
    ↓
original notebook/code

Then make the smallest technically justified interpretation.

The objective is not merely to create a large number of ML questions.

The objective is to build a reliable representation of the actual MLP-OPPE question patterns that Claude can use safely and consistently.
