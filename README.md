# ICSThreatQA-Dataset

## Overview

ICSThreatQA is a question-answering benchmark for cybersecurity threats in Industrial Control Systems (ICS). It consists of 620 human-in-the-loop question-answer (QA) pairs built from a threat knowledge base describing ICS-relevant entities (malware, threat groups, attack patterns, techniques) and the relationships between them.

The dataset is the official data release accompanying:

> Rani, R., Kumar, M., Epiphaniou, G., and Maple, C. (2025). *ICSThreatQA: A Knowledge-Graph Enhanced Question Answering Model for Industrial Control System Threat Intelligence*. Expert Systems with Applications.

This repository contains the benchmark QA pairs, the outputs produced by several candidate QA approaches evaluated against it, and the associated evaluation spreadsheets.

## Why This Benchmark Matters

ICS environments (SCADA systems, PLCs, industrial networks) sit at the intersection of cybersecurity and physical safety — an incorrect or unsupported answer from a QA system used by a security analyst can lead to a missed threat, a wrong containment decision, or misplaced trust in a tool. Evaluating whether QA systems answer ICS security questions correctly, and *how* they arrive at those answers, is therefore not just a natural-language-processing concern but a prerequisite for using such systems safely in security-critical workflows.

## Dataset

`qa_pairs_benchmark/` contains 620 QA pairs split across four question types, one file per type (provided as both `.xlsx` and `.csv`):

- **Factual** (`factual.xlsx` / `.csv`) — direct, verifiable questions about ICS entities, e.g. "What is the primary function of the Stuxnet malware in ICS environments?"
- **Contrastive** (`contrastive.xlsx` / `.csv`) — questions comparing two or more entities (malware, attacks, defenses), e.g. "How does the Triton malware differ from the Industroyer malware in their impact on ICS?"
- **Inferential** (`inferential.xlsx` / `.csv`) — questions requiring analytical reasoning beyond stated facts, e.g. preventive or mitigation actions given a described threat trend.
- **Opinion-based** (`opinion.xlsx` / `.csv`) — questions seeking expert judgment on security practices, policies, or tools, e.g. whether MFA is adequate for securing remote ICS access.

The QA pairs were constructed from a threat knowledge base of ICS entities, attributes, and relationships, with human-in-the-loop review used to keep the questions diverse and contextually grounded rather than templated.

## Repository Structure

```
ICSThreatQA-Dataset/
├── README.md
├── LICENSE                                       # CC BY 4.0
├── CITATION.cff                                  # machine-readable citation metadata
├── qa_pairs_benchmark/
│   ├── factual.xlsx
│   ├── factual.csv
│   ├── contrastive.xlsx
│   ├── contrastive.csv
│   ├── inferential.xlsx
│   ├── inferential.csv
│   ├── opinion.xlsx
│   └── opinion.csv
├── model_responses/
│   ├── knowledge_based_rag_model.xlsx
│   ├── knowledge_based_rag_model.csv
│   ├── hybrid_based_model.xlsx
│   ├── hybrid_based_model.csv
│   ├── keyword_based_model_outcome.xlsx
│   └── keyword_based_model_outcome.csv
└── experiment_results/
    ├── combined_evaluation.xlsx
    ├── combined_evaluation.csv
    ├── zero_shot/
    │   ├── rag_evaluation_zero_shot_results.xlsx
    │   ├── rag_evaluation_zero_shot_results__factual.csv
    │   ├── rag_evaluation_zero_shot_results__contrastive.csv
    │   ├── rag_evaluation_zero_shot_results__inferential.csv
    │   ├── rag_evaluation_zero_shot_results__opinion.csv
    │   ├── gpt4o_evaluation_zero_shot_results.xlsx
    │   ├── gpt4o_evaluation_zero_shot_results__*.csv        # one per sheet: factual/contrastive/inferential/opinion
    │   ├── keyword_evaluation_zero_shot_results.xlsx
    │   ├── keyword_evaluation_zero_shot_results__*.csv      # one per sheet
    │   ├── knowledge_evaluation_zero_shot_results.xlsx
    │   ├── knowledge_evaluation_zero_shot_results__*.csv    # one per sheet
    │   ├── hybrid_evaluation_zero_shot_results.xlsx
    │   ├── hybrid_evaluation_zero_shot_results__*.csv       # one per sheet
    │   └── total_gpt4_evaluation_few_shot_results.csv       # note: named "few_shot" but located under zero_shot/
    └── few_shot/
        ├── hybrid_evaluation_few_shot_results.xlsx
        ├── hybrid_evaluation_few_shot_results__*.csv        # one per sheet
        ├── rag_evaluation_few_shot_results.xlsx
        ├── rag_evaluation_few_shot_results__*.csv           # one per sheet
        ├── keyword_evaluation_few_shot_results.xlsx
        └── keyword_evaluation_few_shot_results__*.csv       # one per sheet
```

Notes on the structure:
- All folders and files now use lowercase `snake_case`, with the original typo `Models reponses/` corrected to `model_responses/`.
- The empty top-level `Model response` file (previously present, no content) has been removed.
- Every `.xlsx` file has a `.csv` mirror with identical data, generated directly from the spreadsheet and not hand-edited. Workbooks with one sheet per question category (`factual`/`contrastive`/`inferential`/`opinion`) export one CSV per sheet, named `<file>__<category>.csv`; single-sheet workbooks export as `<file>.csv`.
- `total_gpt4_evaluation_few_shot_results.csv` is a few-shot result file that remains inside `zero_shot/` rather than `few_shot/`. This placement is reported as observed and was intentionally **not** moved, since it touches experiment-result organization and may reflect a decision by the dataset authors.
- Some original workbooks contain minor typos in their internal sheet names (e.g. a `Contrastive ` sheet with a trailing space, a `Contrative` sheet, and a `Knowldge_based model` sheet). These are part of the original experiment data and were left untouched; where a sheet name feeds a generated CSV filename (e.g. `keyword_evaluation_few_shot_results__contrative.csv`), the filename mirrors the source sheet name exactly rather than silently correcting it.

## Evaluated Approaches

Based on the files present in `model_responses/` and `experiment_results/`, the following QA approaches were run against the benchmark:

- **Knowledge-based RAG** — a retrieval-augmented approach grounded in the ICS threat knowledge graph (`knowledge_based_rag_model.xlsx`, `rag_evaluation_*_results.xlsx`).
- **Hybrid model** — combining retrieval/knowledge grounding with another method (`hybrid_based_model.xlsx`, `hybrid_evaluation_*_results.xlsx`).
- **Keyword-based model** — a non-neural, keyword-matching baseline (`keyword_based_model_outcome.xlsx`, `keyword_evaluation_*_results.xlsx`).
- **GPT-4o** — evaluated in a zero-shot setting (`gpt4o_evaluation_zero_shot_results.xlsx`), with an additional GPT-4 few-shot result file present (see structure note above).

Each of the Knowledge-based, Hybrid, and Keyword-based approaches has both zero-shot and few-shot evaluation results, allowing prompting-strategy comparisons within each approach. This README does not restate specific accuracy or score values from these files — see [Experimental Results](#experimental-results) for where to find them.

## Experimental Results

Evaluation outputs are provided as spreadsheets rather than summarized here, so that consumers of the dataset can inspect and reanalyze the raw scoring:

- `experiment_results/combined_evaluation.xlsx` — a combined view across approaches.
- `experiment_results/zero_shot/` — per-approach evaluation results under zero-shot prompting.
- `experiment_results/few_shot/` — per-approach evaluation results under few-shot prompting.

This README intentionally does not report specific metric values, scores, or rankings, since those are only meaningful in the context of the original paper's methodology. Refer to the published article and the spreadsheets themselves for evaluation criteria and results.

## AI Assurance Perspective

The dataset's structure — multiple candidate QA approaches evaluated against the same fixed benchmark, under both zero-shot and few-shot conditions — makes it usable as a testbed for several AI assurance concerns, in addition to whatever the original paper measured directly:

- **Answer reliability** — whether an approach consistently produces correct answers to the same class of question (factual vs. contrastive vs. inferential vs. opinion-based).
- **Knowledge grounding** — whether knowledge-graph-grounded approaches (Knowledge-based RAG, Hybrid) produce answers traceable to the underlying threat knowledge base, versus approaches without such grounding (Keyword-based, general-purpose LLM).
- **Retrieval quality** — for the RAG and hybrid approaches, whether retrieved context is relevant to the question before it is used to generate an answer.
- **Reasoning consistency** — particularly for inferential questions, whether an approach's reasoning stays consistent across similarly structured questions.
- **Comparison of knowledge-enhanced vs. baseline approaches** — the presence of a keyword-based baseline alongside knowledge-based and hybrid approaches supports direct comparison of how much grounding contributes to answer quality.
- **Human oversight** — the dataset's human-in-the-loop construction process is a documented part of how the benchmark itself was built; whether human review is also needed downstream, when using any of these approaches operationally, is a separate open question this benchmark can help study.

These are broader interpretive framings of what the existing artifacts *support studying*, not a restatement of the original paper's claims. Consult the paper directly for what was actually measured and concluded.

## Potential AI Assurance Extensions

The following are **future research directions** that this benchmark could support, not existing results in this repository:

- Hallucination evaluation — measuring how often each approach produces claims unsupported by the knowledge base.
- Evidence attribution — testing whether generated answers can be traced back to specific knowledge-graph entities or retrieved passages.
- Retrieval failure analysis — characterizing cases where RAG/hybrid retrieval returns irrelevant or missing context, and how that propagates to answer quality.
- Calibration — assessing whether an approach's confidence (where available) matches its actual correctness rate.
- Robustness testing — evaluating stability of answers under paraphrased or reworded versions of the same question.
- Adversarial or ambiguous queries — constructing edge-case questions designed to probe failure modes not covered by the original 620 pairs.
- Knowledge poisoning — studying the effect of corrupted or manipulated entries in the underlying threat knowledge base on downstream answers.
- Prompt-injection resilience in RAG — testing whether retrieved context can be crafted to hijack a RAG pipeline's output.
- Human–AI escalation or review policies — defining when a QA system's output should be routed to a human analyst rather than trusted directly, using this benchmark's question types as a starting taxonomy.

## Reproducibility

This repository provides:

- The fixed benchmark QA pairs (`qa_pairs_benchmark/`), enabling any approach to be run against the same question set.
- Raw model outputs for the three evaluated non-LLM-baseline approaches (`model_responses/`), which can be used to inspect actual generated answers rather than only aggregate scores.
- Evaluation spreadsheets broken out by approach and by prompting strategy (`experiment_results/`), enabling side-by-side comparison and independent reanalysis of the reported evaluation criteria.
- A `.csv` mirror of every `.xlsx` file, so the data can be loaded without Excel or a spreadsheet-specific library, diffed in version control, and used directly in analysis scripts (pandas, R, etc.).
- `CITATION.cff` for unambiguous, machine-readable attribution, and a `LICENSE` clarifying reuse terms.

Together these support reproducing comparisons between approaches and re-running additional evaluation criteria (including those listed under AI Assurance Extensions above) against the same fixed set of questions and existing model responses, without requiring the original experiments to be rerun from scratch.

## Responsible Use

This dataset is provided for research purposes. Answers in this dataset — whether human-authored reference answers or model-generated outputs — should not be used as a substitute for validated professional security analysis in an operational ICS environment. Any use of QA systems evaluated on this benchmark in a real security context should retain qualified human review before acting on their outputs.

## License

This repository is released under the [Creative Commons Attribution 4.0 International License](LICENSE) (CC BY 4.0), a common license for research datasets. It permits reuse and redistribution with attribution. If the original publisher specifies different terms for the dataset, defer to those; this file reflects the terms chosen for this repository, not a claim about the publisher's own licensing.

## Citation

If you use this dataset, please cite the original paper (also available in machine-readable form in [`CITATION.cff`](CITATION.cff)):

```bibtex
@article{rani2025icsthreatqa,
  title={ICSThreatQA: A Knowledge-Graph Enhanced Question Answering Model for Industrial Control System Threat Intelligence},
  author={Rani, Ruby and Kumar, Mahender and Epiphaniou, Gregory and Maple, Carsten},
  journal={Expert Systems with Applications},
  pages={130180},
  year={2025},
  publisher={Elsevier},
}
```

Note: volume, issue, and DOI are not available in this repository and are omitted here rather than invented. Please verify full citation details (volume, issue, DOI) against the published version on the *Expert Systems with Applications* / Elsevier website before citing.

## Research Positioning

Most QA benchmarks, including the original evaluation of this dataset, are structured around the question: **can the model answer the question correctly?**

For QA systems intended to support security-critical decision-making, a second question matters just as much: **can we trust the process by which the model arrived at that answer?** — whether it is grounded in verifiable knowledge, robust to how the question is phrased, resistant to manipulation of its inputs, and calibrated enough to know when it should defer to a human analyst.

ICSThreatQA's combination of a fixed, human-validated benchmark with multiple grounded and ungrounded approaches evaluated against it makes it a candidate testbed for studying this second question, in addition to the first.

