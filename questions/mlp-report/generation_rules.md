# MLP-OPPE Claude Generation Rules

## Purpose

Use the canonical MLP-OPPE question bank to generate practice questions that remain technically faithful to the source OPPE material.

## Source authority

The source notebooks are authoritative. The canonical records are a conservative transformation of those source questions. Original source text is retained separately in `source_questions.json` for provenance and audit.

## What Claude receives

Claude should normally receive:
- one canonical question record from `question_bank.json`;
- the relevant pattern record from `question_patterns.json` when pattern-level generation is required.

Claude should NOT receive all source variants by default. Source variants remain available for audit and future retrieval, but they are deliberately excluded from the normal generation prompt to reduce token use and prevent superficial copying.

## Required preservation

When generating a variant, preserve:
- algorithm/model family;
- dataset and target when specified;
- preprocessing sequence;
- train/test or validation split;
- random_state;
- explicit hyperparameters and their meaning;
- scoring metric;
- parameter search ranges when they define the task;
- answer type and requested precision;
- any technical dependency between questions in a shared instruction block.

Do not invent columns, parameter ranges, datasets, or answers that are not supported by the record.

## Variant generation

Claude may change wording and, where the generation constraints explicitly permit it, create a new numerical/configuration variant. A generated variant must remain mathematically and programmatically valid.

A generated question must be labelled as `generated_variant`; a verbatim source question must be labelled `source`.

## Answer handling

Use the answer status in the record:
- `source_stated_unverified`: the notebook itself contains an answer, but the answer has not been independently reproduced by this extraction pass.
- `requires_execution`: the answer depends on executing the supplied data/code/model.

Do not convert an unverified source answer into a claim of independent verification.

## Evaluation

For student answers:
1. use the canonical question's expected answer type;
2. apply the specified numeric precision/tolerance;
3. preserve the canonical technical setup;
4. do not change the canonical answer to fit the student's response;
5. explain the result using the source-backed method.

## Hierarchical patterns

A pattern groups related skills, for example:

`DECISION_TREE -> GRIDSEARCH -> MAX_DEPTH`

Different scoring metrics, datasets, parameter ranges, or other answer-changing technical details should remain separate canonical questions even when they belong to the same broad pattern.

## Provenance

Every canonical record must retain a source count. Full original question text and source locations are in `source_questions.json`.