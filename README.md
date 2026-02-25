# Benchmarking LLMs on Financial Sentiment Analysis

## Setup (do this first — ~10 min)

```bash
# 1. Clone / open your repo
git clone https://github.com/xx-codificare-xx/Benchmarking-LLMs-on-Financial-Question-Answering
cd Benchmarking-LLMs-on-Financial-Question-Answering

# 2. Install dependencies
pip install -r requirements.txt

# 3. Create a .env file with your API keys (never commit this)
echo "OPENAI_API_KEY=sk-..."      >> .env
echo "ANTHROPIC_API_KEY=sk-ant-..." >> .env
echo "GOOGLE_API_KEY=..."          >> .env
echo "TOGETHER_API_KEY=..."        >> .env

# 4. Create data directories
mkdir -p data/raw data/processed results
```

## Data Setup

Place your datasets here:
```
data/raw/
├── stock_tweets.csv    ← from Kaggle (stock tweets sentiment)
└── wsb_posts.csv       ← from moonscape95/WSBS repo
```

**⚠️ Before running anything:** Open each CSV and check the actual column names.
Then update the `TEXT_COL`, `LABEL_COL`, `TICKER_COL` variables in `dataset_loader.py`.

## Run Order

```bash
# Step 1: Check your data loads correctly
python dataset_loader.py

# Step 2: Sanity-check one model works (cheap test — 10 rows, 1 model)
python run_experiment.py --models claude --prompts zero_shot --limit 10

# Step 3: Full dry run (all prompts, 1 model, 50 rows)
python run_experiment.py --models claude --prompts zero_shot few_shot chain_of_thought --limit 50

# Step 4: Full experiment (expensive — run overnight or on the weekend)
python run_experiment.py
```

## File Structure

```
├── model_runner.py      ← unified API caller for all 4 models
├── dataset_loader.py    ← loads + normalizes both datasets
├── run_experiment.py    ← runs all combinations, saves results CSV
├── requirements.txt
├── data/
│   ├── raw/             ← your original CSVs go here
│   └── processed/       ← cleaned data (auto-generated)
└── results/             ← experiment output CSVs (auto-generated, timestamped)
```

## API Cost Estimates (rough)

| Model  | Per 1K calls (zero-shot) |
|--------|--------------------------|
| GPT-4o | ~$5–10                   |
| Claude | ~$3–6                    |
| Gemini | ~$1–3                    |
| Llama  | ~$0.50–1 (Together.ai)   |

**Tip:** Run with `--limit 200` first to get preliminary results cheaply,
then scale up once you've confirmed the pipeline works.
