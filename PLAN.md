# Vester Fundraising Agent — Implementation Plan

## Overview
A Python-based fundraising agent that sources investor leads, scores/prioritizes them, manages outreach tracking, and provides a dashboard for managing the entire fundraise pipeline.

---

## Architecture

```
fundraising-agent/
├── pyproject.toml              # Project config & dependencies
├── README.md                   # Setup & usage docs
├── .env.example                # Required env vars template
├── src/
│   └── fundraising_agent/
│       ├── __init__.py
│       ├── main.py             # CLI entrypoint
│       ├── config.py           # Settings & env loading
│       ├── models.py           # Data models (Investor, Lead, Outreach, etc.)
│       ├── db.py               # SQLite persistence layer
│       ├── agents/
│       │   ├── __init__.py
│       │   ├── lead_sourcer.py     # Finds & enriches investor leads
│       │   ├── lead_scorer.py      # Scores/ranks leads by fit
│       │   └── outreach_manager.py # Tracks outreach status & follow-ups
│       ├── sources/
│       │   ├── __init__.py
│       │   ├── crunchbase.py   # Crunchbase data fetching
│       │   ├── linkedin.py     # LinkedIn profile enrichment
│       │   └── web_search.py   # General web search for investors
│       ├── prompts/
│       │   ├── sourcing.py     # Prompts for lead sourcing agent
│       │   ├── scoring.py      # Prompts for lead scoring agent
│       │   └── outreach.py     # Prompts for outreach drafting
│       └── utils/
│           ├── __init__.py
│           └── export.py       # CSV/JSON export utilities
└── tests/
    ├── __init__.py
    ├── test_models.py
    ├── test_db.py
    └── test_agents.py
```

---

## Step-by-step Implementation

### Step 1 — Project scaffolding
- Create `pyproject.toml` with dependencies: `anthropic`, `sqlite3` (stdlib), `pydantic`, `rich` (CLI output), `python-dotenv`
- Set up package structure (`src/fundraising_agent/`)
- Create `.env.example` with `ANTHROPIC_API_KEY`
- Create `config.py` for loading settings

### Step 2 — Data models (`models.py`)
Define Pydantic models:
- **Investor** — name, firm, title, focus_areas, stage_preference, check_size_range, location, linkedin_url, email, notes
- **Lead** — investor ref, score, fit_reasons, status (new/contacted/meeting/passed/committed), source
- **OutreachRecord** — lead ref, type (email/intro/meeting), date, content_summary, response, next_action, next_action_date
- **RaiseConfig** — company details (name, stage, sector, raise_amount, deck_url, one_liner) used for scoring fit

### Step 3 — Database layer (`db.py`)
- SQLite-backed persistence (zero setup, portable)
- CRUD operations for investors, leads, outreach records
- Query helpers: leads by status, upcoming follow-ups, pipeline summary stats

### Step 4 — Lead sourcing agent (`agents/lead_sourcer.py`)
- Uses Claude API to analyze web search results and identify relevant investors
- Input: Vester's raise config (stage, sector, geography, check size)
- Sources to search: Crunchbase-style data, investor websites, portfolio pages
- Output: list of enriched Investor records saved to DB
- Deduplication against existing leads

### Step 5 — Lead scoring agent (`agents/lead_scorer.py`)
- Uses Claude API to score each investor on fit (0-100)
- Scoring criteria: stage match, sector match, check size match, geographic fit, portfolio synergy, recent activity
- Outputs ranked list with reasoning for each score
- Auto-tags top leads as priority

### Step 6 — Outreach manager (`agents/outreach_manager.py`)
- Drafts personalized outreach emails using Claude API
- Tracks outreach status per lead (sent, responded, meeting scheduled, passed)
- Generates follow-up reminders
- Logs all interactions

### Step 7 — CLI interface (`main.py`)
Commands:
- `fundraise config` — Set up raise parameters (amount, stage, sector, etc.)
- `fundraise source` — Run lead sourcing agent
- `fundraise score` — Score/rank all unscored leads
- `fundraise leads` — View lead pipeline (filterable by status/score)
- `fundraise outreach <lead_id>` — Draft outreach for a specific lead
- `fundraise follow-ups` — Show upcoming follow-ups
- `fundraise stats` — Pipeline summary (leads by stage, conversion rates)
- `fundraise export` — Export pipeline to CSV

### Step 8 — Tests
- Unit tests for models and DB layer
- Integration tests for agent workflows with mocked Claude responses

---

## Key Design Decisions

| Decision | Choice | Rationale |
|---|---|---|
| Language | Python | Best Anthropic SDK support, fast to iterate |
| LLM | Claude (Anthropic SDK) | Native tool use, strong reasoning for scoring |
| Database | SQLite | Zero config, single file, easy to back up |
| CLI framework | `argparse` + `rich` | Minimal deps, great terminal output |
| Data validation | Pydantic | Type safety, serialization, schema clarity |

---

## What's NOT in v1 (future scope)
- Web UI dashboard
- Calendar integration for scheduling meetings
- Automated email sending (v1 drafts only)
- CRM integrations (HubSpot, Affinity)
- Multi-user support
