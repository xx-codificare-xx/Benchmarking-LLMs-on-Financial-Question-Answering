# Benchmarking LLMs on Financial Sentiment Analysis

This project evaluates large language models (LLMs) on **financial sentiment analysis** and analyzes their reliability and failure modes on stock-related text such as tweets and Reddit (WallStreetBets) posts.[file:1]

We compare general-purpose LLMs (e.g., GPT-4, Claude, Gemini, Llama) against a finance-specific baseline (FinBERT) and classic machine learning models where possible.[file:1][web:38] Our focus is not only on accuracy but also on understanding **how and why** models fail, especially when they produce confident but incorrect sentiment labels in a financial context.[file:1]

## 1. Motivation

Financial sentiment extracted from social media and investor discussions is often used to anticipate market movements and support trading decisions.[file:1] However, financial text is noisy, uses domain-specific language, and often mixes hype, sarcasm, and technical terms. Prior work shows that:

- General sentiment models can struggle with financial jargon and numerics.  
- Domain-specific models like **FinBERT** improve performance but still exhibit errors and biases.[file:1][web:38]  

Our goal is to systematically benchmark LLMs on financial sentiment tasks and analyze their **error patterns and reliability**, continuing the direction described in our original proposal.[file:1]

## 2. Project Goals

1. **Benchmark multiple LLMs** (GPT-4, Claude, Gemini, Llama) on labeled financial sentiment datasets (tweets + WallStreetBets).  
2. **Compare against baselines** such as FinBERT and/or simple ML classifiers.[file:1][web:38]  
3. **Analyze failure modes**, including:
   - Confusion over “risk” / “volatility” mentions.  
   - Handling of sarcasm, memes, and mixed sentiment.  
   - Overconfident misclassifications.  

## 3. Datasets

Following our proposal, we focus on **financial social media sentiment**.[file:1]

### 3.1 Stock tweets with sentiment labels

We will use a **stock tweet sentiment dataset** (e.g., Kaggle stock-tweet sentiment or similar), containing thousands of tweets referencing stock tickers and labeled with sentiment.[file:1][web:29][web:30][web:33]  

For feasibility, we will:

- Filter to a subset focused on a single ticker (e.g., TSLA) or a small group of tickers.  
- Sample a manageable number of labeled tweets (e.g., 2–5k) for LLM evaluation.

### 3.2 WallStreetBets (WSB) posts

Our proposal also includes a **WallStreetBets sentiment dataset**, which consists of Reddit posts and/or comments annotated for sentiment.[file:1][web:43]  

In this project, we will:

- Use an existing WSB posts dataset (e.g., Kaggle Reddit WallStreetBets posts) as raw text.[web:43]  
- Manually label or weakly label a small subset, or use it primarily for qualitative error analysis rather than large-scale metrics.

### 3.3 Label scheme

We use a simple **3-class sentiment scheme**:

- `positive`  
- `negative`  
- `neutral`  

This aligns with FinBERT’s default output space and many financial sentiment datasets.[web:38]

## 4. Models

Planned models:

- **LLMs (zero-/few-shot)**  
  - GPT-4 (OpenAI)  
  - Claude 3 (Anthropic)  
  - Gemini (Google)  
  - Llama 3.x (Meta, via API or hosted endpoint)  

- **Baselines**  
  - FinBERT (pre-trained financial sentiment model).[file:1][web:38]  
  - Simple models (e.g., logistic regression on bag-of-words) where feasible.[file:1]  

Initially, we will implement a pipeline for **one LLM** (e.g., GPT-4) to obtain preliminary results; additional models will be added as time permits.

## 5. Repository Structure

```text
.
├── data/
│   ├── tweets_sample.csv        # subset of stock tweets with labels
│   └── wsb_sample.csv           # (optional) small labeled WSB subset
├── src/
│   ├── prompts.py               # sentiment prompts
│   ├── run_gpt4_sentiment.py    # LLM baseline on tweet sentiment
│   └── utils.py                 # data loading, simple metrics
├── notebooks/
│   └── 01_prelim_results.ipynb  # metrics + error analysis
├── results/
│   └── gpt4_tweets_raw.csv      # raw model outputs
└── README.md

```

---

## How To Run

```
pip install -r requirements.txt
```
