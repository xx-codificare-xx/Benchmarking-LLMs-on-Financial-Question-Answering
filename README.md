# Benchmarking LLMs on Financial Question Answering

This project evaluates large language models (LLMs) on **financial question answering (QA)** and analyzes their failure modes on realistic financial documents such as 10-K filings and established benchmarks.

We focus on questions that require factual lookup, numerical reasoning, and interpretation of financial text, and we study where models give confident but incorrect answers.

## 1. Motivation

LLMs are increasingly used in finance to analyze reports, filings, and news, but it is unclear how reliably they answer detailed questions about company performance, risks, and financial metrics.[file:1] Errors in this setting can be costly for practitioners.

Prior work has proposed domain-specific models like FinBERT and has evaluated LLMs on financial sentiment, showing challenges with numerical reasoning and domain-specific language.[file:1] New QA benchmarks such as **FinanceBench** and **FinTextQA** highlight that financial QA remains difficult even for strong models.[web:17][web:10][web:6][web:21]

Our project contributes a small curated QA dataset from a 10-K filing and a benchmark-style evaluation of several LLMs.

## 2. Project Goals

1. **Benchmark multiple LLMs** (GPT-4, Claude, Gemini, Llama) on financial QA.  
2. **Create a small financial QA dataset** (“Mini-10K-QA”) from a single 10-K filing.  
3. **Analyze failure modes**, especially:
   - Numerical reasoning errors.  
   - Misinterpretation of tables/sections.  
   - Hallucinated values not supported by the context.  

These goals align with our original proposal’s focus on financial NLP and model reliability, with the task shifted from sentiment classification to QA.[file:1]

## 3. Datasets

We work with two types of datasets:

### 3.1 Mini-10K-QA (our dataset)

A small curated dataset (target: 50–100 QA pairs) built from one public company’s latest 10-K filing.  
Each entry has:

- `question`: natural-language question an analyst might ask.  
- `answer`: short text or numeric answer.  
- `context`: supporting passage or paragraph from the 10-K.  
- `question_type`: e.g., `lookup`, `numeric`, `reasoning`.  

Stored as JSON in `data/mini_10k_qa.json`.

### 3.2 External financial QA benchmarks (small slices)

We may optionally evaluate on small subsets of public financial QA datasets, such as:

- **FinanceBench** – expert-annotated QA over SEC filings, designed to test retrieval and reasoning in financial documents.[web:17][web:10][web:6][web:23][web:26]  
- **FinTextQA** – long-form financial QA with cited sources from textbooks and authoritative sites.[web:21][web:24][web:19]  
- **FinanceQA** – QA pairs over financial texts, available on Hugging Face.[web:2][web:25]  

For this course project, our main focus is on Mini-10K-QA; external datasets are used for comparison on a small subset due to time and resource constraints.

## 4. Models

Planned models (subject to API access):

- GPT-4 (OpenAI)  
- Claude 3 (Anthropic)  
- Gemini (Google)  
- Llama 3.x (Meta) via hosted API or local setup  

All models are evaluated with a standardized prompt template to ensure fairness.

## 5. Repository Structure

```text
.
├── data/
│   └── mini_10k_qa.json        # our curated QA dataset
├── src/
│   ├── prompts.py              # shared prompt templates
│   ├── run_gpt4_mini10k.py     # baseline evaluation script for GPT-4
│   └── utils.py                # generic helpers (loading, scoring)
├── notebooks/
│   └── 01_prelim_results.ipynb # for exploratory analysis / plotting
├── results/
│   └── gpt4_mini10k_raw.csv    # raw model outputs
└── README.md

```

---

## How To Run

```
pip install -r requirements.txt
```
