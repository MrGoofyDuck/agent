# 🎬 TMDB Movies AI Agent

An end-to-end data engineering project that transforms raw TMDB movie data into an intelligent, conversational analytics system powered by Groq LLaMA 3.3 70B.

---

## 📋 Overview

This project solves a real business problem: raw movie data is unusable without technical expertise. We built a full ETL pipeline that normalizes 5,000 movies into a relational MySQL database, then layered an AI agent on top so anyone can ask business questions in plain English — no SQL required.

---

## 🗂️ Project Structure

```
tmdb-movies-ai-agent/
│
├── collect_data.py          # ETL Step 1 — merge raw CSVs
├── transform_data.py        # ETL Step 2 — parse JSON, compute metrics
├── load_to_mysql.py         # ETL Step 3 — load into MySQL
├── TMDB_Movies_Agent.ipynb  # Main notebook — full pipeline + AI agent
└── README.md
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Python | Core language |
| Pandas | Data processing |
| SQLAlchemy | Database connection |
| MySQL | Relational database |
| LangChain | AI agent framework |
| Groq | LLM API provider |
| LLaMA 3.3 70B | Language model |
| Matplotlib | Dashboard charts |
| Jupyter | Notebook environment |

---

## 🗄️ Database Schema

9 normalized tables in 3NF:

```
genres ──────────── movie_genres ──────────────┐
keywords ─────────── movie_keywords ────────── movies
production_companies ─ movie_companies ─────────┤
                                                 ├── cast_members
                                                 └── crew_members
```

**movies** table has 20 columns including computed metrics:
- `profit` = revenue − budget
- `roi` = profit ÷ budget
- `vote_weighted` = Bayesian weighted rating

---

## ⚙️ ETL Pipeline

### Step 1 — collect_data.py
- Reads `tmdb_5000_movies.csv` + `tmdb_5000_credits.csv`
- Checks DB for existing movie IDs (incremental loading)
- Merges on `movie_id` → outputs `raw_tmdb_data.csv`

### Step 2 — transform_data.py
- Parses all JSON-encoded columns (genres, cast, crew, keywords)
- Computes `profit`, `roi`, `vote_weighted`
- Splits into 9 normalized CSV files

### Step 3 — load_to_mysql.py
- Loads CSVs into MySQL in FK-safe order
- INSERT IGNORE — no duplicates ever

---

## 🤖 AI Agent

Built with LangChain + Groq. Ask any question in Russian or English:

```
You: "Which genres have the highest average ROI?"

Agent: generates SQL → executes on MySQL → returns answer
```

The agent knows the full database schema and writes optimized SQL automatically.

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/MrGoofyDuck/agent.git
cd agent
```

### 2. Install dependencies
```bash
pip install langchain langchain-groq langchain-community pymysql sqlalchemy pandas matplotlib seaborn ipywidgets
```

### 3. Set up MySQL
- Create database `tmdb_movies`
- Run the DDL from `tmdb_ddl.sql`

### 4. Add your credentials
Open the notebook and fill in **Step 2 — Configuration**:
```python
GROQ_API_KEY   = "your_groq_api_key_here"   # console.groq.com
MYSQL_HOST     = "localhost"
MYSQL_PASSWORD = "your_password"
```

### 5. Add the raw CSV files
Download from Kaggle: [TMDB 5000 Movie Dataset](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata)

Place in the project folder:
- `tmdb_5000_movies.csv`
- `tmdb_5000_credits.csv`

### 6. Run the notebook
Open `TMDB_Movies_Agent.ipynb` and run cells in order:
- **Step 1** — Install dependencies
- **Step 2** — Configuration
- **Step 3** — Verify schema
- **Step 4** — AI Agent
- **Step 5** — Dashboard
- **Step 6** — Interactive chat

---

## 📊 Key Results

| Metric | Value |
|--------|-------|
| Movies | 4,803 |
| Years covered | 1916 – 2017 |
| DB Tables | 9 |
| Cast rows | 85,000+ |
| Languages supported | Russian + English |

### Insights from the data
- Drama dominates — 48% of all films
- Horror has the highest average ROI: 3.2x
- Steven Spielberg: most prolific director (26 films)
- Warner Bros.: highest average revenue at $225M

---

## 🔄 Incremental Updates

When new data arrives, run **Step 7 (Incremental Update)** in the notebook. It automatically detects which movies are already in the database and loads only new ones — no duplicates, no manual work.

---

## ⚠️ Important

- Never commit your API keys — use `"your_groq_api_key_here"` as placeholder
- Raw CSV files are excluded from the repository (too large)
- Get a free Groq API key at [console.groq.com](https://console.groq.com)

---

## 👥 Authors

- MrGoofyDuck

---

## 📄 License

MIT License
