# MLP-OPPE Question Report

## What this contains

This directory contains the Claude-ready MLP-OPPE question intelligence layer extracted from the supplied repository/notebook set.

Files:

- `question_bank.json` — canonical questions for normal Claude/app retrieval.
- `question_patterns.json` — hierarchical skill/pattern groupings and generation constraints.
- `source_questions.json` — source-level records preserving the original notebook question text exactly, including Markdown/code/surrounding wording captured in the source question block.
- `generation_rules.md` — rules Claude must follow when generating variants and evaluating answers.

## Source

The supplied `mlp-oppe-main.zip` was treated as the authoritative source. It contained 18 Jupyter notebooks. The extraction inspected notebook Markdown cells and nearby Python code cells. CSV files included in the supplied repository were also treated as available source material, but this pass did not execute every notebook end-to-end.

## Extraction method

1. Enumerate all `.ipynb` files recursively.
2. Identify explicit question headings such as `Q1`, `Q2`, `Question 1`, `Que 1`, and numbered question headings.
3. Also identify standalone question-form Markdown cells in notebooks where question headings are absent.
4. Treat a question plus its immediate common-instruction/context block as one question unit rather than splitting every Markdown cell into a separate question.
5. Preserve the original source question block in `source_questions.json`.
6. Inspect nearby code cells to capture model setup and computational context.
7. Normalize only the structural metadata; do not generalize away technical details such as datasets, targets, random states, scoring metrics, hyperparameters, or requested precision.
8. Consolidate exact/near-exact repeated question wording into canonical records while retaining source provenance.
9. Assign a hierarchical pattern using topic, subtopic, algorithm and requested output/metric. Technically different configurations remain separate canonical questions even when they belong to the same broad pattern.
10. Keep answer provenance explicit. Notebook-stated answers are marked `source_stated_unverified` unless independently reproduced; questions requiring notebook/model execution are marked `requires_execution`.

## Current extraction counts

- 18 notebooks processed.
- 283 source question units identified.
- 250 canonical question records.
- 82 hierarchical patterns.
- 44 canonical records contain source-stated answer evidence.
- 206 canonical records require execution for an answer in the current pass.

The distinction between 283 source units and 250 canonical questions is intentional: repeated question wording is consolidated, while the original occurrences remain available in `source_questions.json`.

## Why the canonical layer is different from the source layer

The app should not send raw notebook material directly to Claude. Raw notebooks contain repeated questions, common instructions, formatting noise, answer keys, code, and supporting cells. The canonical layer converts that material into a stable schema that Claude can consume without having to rediscover the structure each time.

Claude normally receives only the canonical record and relevant pattern. Original source variants remain in the repository for audit/provenance and can be retrieved when needed.

## Important answer-status rule

A printed answer in a notebook is not automatically an independently verified answer. This report therefore does not silently upgrade source answer keys into verified results. Where independent execution has not been performed, the record says so explicitly.

## Intended application flow

`source notebooks -> source_questions.json -> canonical question_bank.json -> pattern retrieval -> Claude generation/evaluation`

For generation, Claude must preserve the technical constraints in the canonical record. For evaluation, the canonical answer/status is the reference; do not invent missing answers.