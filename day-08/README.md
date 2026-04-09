# Day 08 - Data Preparation for AI Pipelines

![Day 08 - Data Preparation for AI Pipelines](day%208.png)

**Theme:** Week 2 - Data, Prompting, and Evaluation  
**Date:** 2026-04-08

---

## Learning Objectives

By the end of Day 8, you should be able to:

- Explain why data quality often matters more than model size for practical outcomes.  
- Build a repeatable preprocessing workflow (cleaning, normalization, deduplication).  
- Structure datasets for common AI tasks (classification, generation, instruction tuning).  
- Design basic train/validation/test splits that reduce leakage and misleading metrics.  
- Run a lightweight data quality checklist before training or evaluating any model.

---

## Why Data Preparation Is Critical

Most AI project failures are not caused by "bad models." They are caused by:

- Noisy or inconsistent data.  
- Label errors.  
- Data leakage between train and evaluation sets.  
- Mismatch between training data and real production inputs.

A clean, representative dataset gives you:

- More stable model behavior.  
- Better generalization.  
- Faster iteration (fewer "mystery bugs").  
- More trustworthy evaluation results.

Rule of thumb: spend serious time on data before you spend money on bigger models.

---

## Data Preparation Pipeline (Practical Sequence)

Use this sequence as your default:

1. **Collect and unify sources** into one schema.  
2. **Clean text and fields** (encoding, nulls, formatting).  
3. **Normalize** values (dates, units, casing, label names).  
4. **Deduplicate** near-identical records.  
5. **Validate labels** and class balance.  
6. **Split** into train/validation/test sets.  
7. **Run quality checks** and produce a short data report.

This turns "random files" into a reproducible dataset asset.

---

## Cleaning: What to Remove vs Keep

Common cleaning steps:

- Remove empty rows and malformed records.  
- Strip HTML artifacts if they are not task-relevant.  
- Fix broken Unicode and whitespace issues.  
- Standardize line breaks and punctuation edge cases.  
- Convert obvious placeholders (`N/A`, `-`, `unknown`) into explicit nulls.

Be careful not to over-clean:

- For many NLP tasks, punctuation and casing can carry signal.  
- Domain-specific jargon, abbreviations, and misspellings may reflect real user input.

Only remove information that clearly harms the task.

---

## Normalization and Schema Consistency

Normalization reduces accidental variation:

- Dates in one format (`YYYY-MM-DD`).  
- Consistent units (e.g., always USD, always metric).  
- Canonical label names (`positive`, `neutral`, `negative`).  
- Stable identifier fields for joining and tracking provenance.

Create a minimal schema contract for each row, for example:

- `id`  
- `text`  
- `label` (if supervised)  
- `source`  
- `created_at`

Consistency in schema makes downstream training and evaluation much easier.

---

## Deduplication and Leakage Prevention

Duplicates can inflate metrics and hide weak generalization.

- **Exact duplicates:** identical text/record repeats.  
- **Near duplicates:** same content with tiny edits.

Basic strategy:

- Remove duplicates before splitting.  
- For near-duplicates, use similarity heuristics and manual spot-checking.  
- Split by entity/time when needed (for example, by customer ID or date windows) to avoid leakage.

If train and test contain near-identical samples, evaluation becomes overly optimistic.

---

## Splits: Train, Validation, Test

Use clear split intent:

- **Train:** fit model parameters.  
- **Validation:** tune prompts/hyperparameters/model choices.  
- **Test:** final unbiased score, used sparingly.

Guidelines:

- Keep label distribution similar across splits (stratify when appropriate).  
- For temporal data, prefer time-based split over random split.  
- Never tune repeatedly on test data.

Common ratio: `80/10/10` (adjust by dataset size and domain constraints).

---

## Dataset Formats You Should Know

For most AI workflows, you will frequently see:

- **CSV/Parquet** for tabular and analytics-friendly pipelines.  
- **JSONL** for LLM tasks and instruction-style records.  
- Task-specific formats for libraries (`datasets`, custom loaders, etc.).

Typical JSONL examples:

- Classification: `{ "text": "...", "label": "..." }`  
- Instruction tuning: `{ "messages": [...], "metadata": {...} }`  
- QA/extraction: `{ "input": "...", "output": "..." }`

Pick the format that your training/evaluation stack can consume with minimal conversion.

---

## Data Quality Checklist (Before Training)

Run this quick checklist:

- Missing values are measured and acceptable.  
- Label set is complete and unambiguous.  
- Class imbalance is understood and documented.  
- Duplicates/near-duplicates are handled.  
- Splits are leakage-resistant.  
- A random manual sample (20-50 rows) has been reviewed.

This single checklist prevents many downstream failures.

---

## Suggested Hands-On Exercises

- Build a small `raw -> cleaned -> split` pipeline for one dataset you already have.  
- Create a one-page data report with row counts, null rates, and label distribution.  
- Detect and remove exact duplicates; estimate near-duplicate rate.  
- Compare model baseline results before and after cleaning to quantify impact.

---

## What Comes Next

- **Day 9 - Prompt Engineering Fundamentals:** prompt structure, role instructions, and output constraints.  
- You will use today's clean datasets to create better prompts and more reliable evaluations.

---

## References and Resources

- Data-centric AI principles and practical case studies.  
- Documentation for `pandas`, `pyarrow`, and Hugging Face `datasets`.  
- Guides on train/validation/test split design and leakage prevention.  
- JSONL best practices for LLM instruction and evaluation workflows.

You can replace these placeholders with your preferred links, notebooks, and internal standards.
