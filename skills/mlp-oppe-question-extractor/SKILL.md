# MLP-OPPE Question Extraction and Deduplication Skill

## Purpose

This document is a complete handover for Claude to extract, normalize, deduplicate, validate, and maintain the Machine Learning Practice (MLP) OPPE question bank from:

- Repository: https://github.com/vraghavans/mlp-oppe
- Main branch: `main`
- Question destination: `questions/`
- Image-question backlog: `questions/image_questions_backlog.md`

The goal is to create a high-fidelity, reusable question bank for MLP OPPE preparation. The source repository is authoritative. Do not invent missing wording, examples, datasets, constraints, diagrams, or outputs.

## Operating rules

1. Scan every directory and every file recursively, including notebooks, Markdown, Python, CSV, PDFs, images, HTML, text, answer files, practice files, and supporting assets.
2. Process the entire repository without waiting for user confirmation between folders or file categories.
3. Report progress after each major directory or file category, but continue automatically.
4. Work directly on the `main` branch unless explicitly instructed otherwise.
5. Always include the original repository URL in READMEs, reports, and relevant source references.
6. Make separate commits for: the skill document, extracted questions, image backlog, and README/reports whenever practical.
7. Do not delete or modify source materials. Add derived question-bank files separately.

## Scope of questions

Include all practical OPPE-relevant machine-learning tasks, including but not limited to:

- Python programming and code completion
- NumPy and pandas operations
- data loading, cleaning, transformation, and preprocessing
- feature engineering and feature selection
- visualization and plotting tasks
- statistics or probability tasks when implemented through code
- regression and classification
- clustering
- dimensionality reduction
- model training and prediction
- scikit-learn workflows and pipelines
- model evaluation and metrics
- cross-validation and hyperparameter tuning
- neural networks and deep-learning implementation tasks
- algorithm implementation used in an ML workflow
- debugging, output analysis, and correction of code
- SQL or other practical data tasks if present
- notebook-based multi-cell practical tasks

Exclude only questions that are purely theoretical and do not ask the student to write code, execute/analyze code, produce a practical ML result, manipulate data, build/evaluate a model, or perform an implementation task.

## Source fidelity

Preserve the first-occurring student-facing version exactly wherever possible:

- wording and order
- headings and subheadings
- examples
- input and output descriptions
- constraints
- dataset names and column names
- code templates and function signatures
- imports and comments
- `YOUR CODE HERE` markers
- notebook cell order and relevant cell boundaries
- expected outputs and displayed results

Do not silently rewrite source text for style. If formatting must be represented in Markdown, preserve the meaning and content and state when formatting was normalized.

## Notebook handling

For every `.ipynb` file:

1. Parse the notebook as JSON.
2. Preserve cell order.
3. Classify each cell as markdown, code, raw, or output-bearing.
4. Treat markdown cells as likely question/context material.
5. Treat code cells as student templates, reference implementations, setup, demonstrations, or unrelated support code; determine their role from surrounding cells.
6. Preserve useful imports, variable definitions, function signatures, comments, and setup needed to understand or run the task.
7. Capture dataset references, file names, URLs, column names, shapes, sample data, and expected output shown in cells.
8. Do not mistake exploratory analysis or a solution cell for a student question without checking the surrounding context.
9. When a question spans multiple cells, include the complete relevant sequence in the student-facing context and identify cell boundaries where useful.
10. If a notebook contains plots, screenshots, embedded images, or rendered tables, preserve the original asset or notebook reference; do not redraw it.

## Classification rules

A practical question should normally have at least one of these signals:

- explicit instruction to write, complete, implement, modify, or debug code
- a required function, class, query, notebook cell, or executable workflow
- a specified input dataset or data frame operation
- required output, prediction, metric, plot, transformed data, or model artifact
- an implementation task embedded in an OPPE/practice exercise

Do not include merely incidental code that supports a prose explanation unless the student is expected to perform that code task.

## Deduplication rules

Use independent MLP IDs: `MLP-Q001`, `MLP-Q002`, etc.

Treat questions as duplicates when they are materially the same task and differ only in wording, formatting, variable names, or minor presentation details.

Do not deduplicate solely because the topic is the same. Keep questions separate when any material element differs, such as:

- task or requested operation
- algorithm or method required
- dataset or schema
- input structure
- output structure
- constraints
- number or type of classes/features/records
- evaluation metric
- expected behavior
- code interface or function signature
- model family or hyperparameters

For every occurrence, count each appearance, including repeated appearances within one file. Record all source paths and, where available, notebook cell ranges or page numbers.

Use the exact wording from the earliest occurrence as the representative wording. If the first occurrence is incomplete and a later occurrence supplies missing context, preserve both and explain the relationship rather than silently fabricating a merged source.

## Required question record

Each deduplicated question should be stored in a readable Markdown file under `questions/`, preferably one file per question or a clearly indexed collection. Each record must include:

- Question ID: `MLP-Q###`
- Topic and subtopic
- Short descriptive title
- Occurrence count
- First occurrence
- All occurrences/source paths
- Original repository URL
- Exact representative question wording
- Complete student-facing context
- Input format and data description
- Output format or expected result
- Constraints and assumptions
- Examples and expected outputs
- Dataset/file dependencies
- Student code template
- Full Reference Code
- Reference-code provenance: official, corrected official, or AI-generated
- Correction notes if an official solution was incomplete, incorrect, or inconsistent
- Notebook cell/page/image references where relevant
- Unresolved issues or manual-review notes

## Code-template rules

### When `YOUR CODE HERE` exists

- Preserve the complete original runnable student template.
- Preserve every existing `YOUR CODE HERE` marker exactly.
- Do not replace the marker in the student template.
- Add a separate `Full Reference Code` section containing the completed implementation.

### When no marker exists

- Do not invent a `YOUR CODE HERE` marker.
- Preserve the original function/class/query/signature and surrounding context.
- State that the student is expected to implement or modify the relevant portion if that is what the source requires.
- Include the full reference implementation separately.

### Reference solutions

1. Prefer an official solution from the repository.
2. If the official solution is incomplete, incorrect, or inconsistent, correct it and label it `Corrected official solution`.
3. The original flawed solution does not need to be stored in the question record, but the correction must be documented.
4. If no official solution exists, generate a suitable implementation and label it `AI-generated reference solution`.
5. Never claim an AI-generated solution is official.
6. Validate reference code against the stated task, examples, and available data where feasible.
7. Include dependencies and execution notes when necessary.

## Images, graphs, tables, and diagrams

Images and visual material are source evidence, not content to recreate.

- Never redraw, regenerate, simplify, or replace a source graph, diagram, screenshot, table, or plot.
- Preserve the original visual exactly through the original notebook, source asset, image URL, or page reference.
- OCR text may be included only as an aid and must be labelled as OCR/transcription.
- The original visual remains authoritative.
- If a visual question cannot be faithfully extracted, place it in `questions/image_questions_backlog.md` instead of guessing.
- For each backlog item include question/backlog ID, source path, notebook cell or page, original image URL or asset path, available metadata, OCR text if any, and the reason it needs manual verification.

## Suggested file layout

```text
questions/
├── README.md
├── MLP-Q001.md
├── MLP-Q002.md
├── ...
├── image_questions_backlog.md
├── extraction_report.md
└── source_manifest.md
```

If many questions are better grouped by topic, use subfolders, but retain stable IDs and a top-level index.

## Quality-control checklist

Before considering the extraction complete:

- [ ] Every repository directory and file was enumerated.
- [ ] Every supported notebook was parsed.
- [ ] Binary or inaccessible files were recorded rather than silently skipped.
- [ ] Pure theory-only material was excluded.
- [ ] Practical ML tasks were included even when embedded in notebooks.
- [ ] Original wording and student context were preserved.
- [ ] `YOUR CODE HERE` markers were preserved exactly.
- [ ] No artificial markers were inserted.
- [ ] Official solutions were checked for correctness.
- [ ] Corrected solutions are labelled clearly.
- [ ] AI-generated solutions are labelled clearly.
- [ ] Duplicate detection was based on task equivalence, not topic similarity.
- [ ] Every occurrence was counted.
- [ ] All source paths and notebook cell/page references were recorded.
- [ ] Visual questions were preserved or moved to the image backlog.
- [ ] No graph, table, or diagram was recreated.
- [ ] README and extraction reports include the original repository URL.
- [ ] The final report lists completed files, unresolved items, and inaccessible sources.

## Progress-report format

After each major directory or file category, report:

- category processed
- files scanned
- practical questions found
- new deduplicated questions
- duplicate occurrences added
- image questions moved to backlog
- unresolved/inaccessible files
- commit SHA, if committed

Continue processing automatically after the report.

## Final deliverables

At minimum, produce:

1. `skills/mlp-oppe-question-extractor/SKILL.md`
2. `questions/README.md`
3. one or more question-bank files with `MLP-Q###` IDs
4. `questions/image_questions_backlog.md`
5. `questions/extraction_report.md`
6. `questions/source_manifest.md`

The final response should provide the repository URL, summarize counts, list unresolved items, and identify the commits created.
