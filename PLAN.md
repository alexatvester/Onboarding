# Vester Fundraising Agent — Implementation Plan (v2)

## Overview
A **Claude Code-native fundraising system** that runs entirely through your Claude Max subscription. No external app — the agent lives in your repo as skills, scheduled tasks, CLAUDE.md context files, and structured markdown/JSON data on the file system. It sources investor leads, manages outreach via email, and maintains all fundraising materials.

---

## Architecture

```
fundraising-agent/
├── CLAUDE.md                          # Master context: Vester profile, raise params, agent instructions
├── PLAN.md                            # This file
│
├── .claude/
│   ├── settings.json                  # Hooks & scheduled task config
│   └── skills/
│       ├── source-leads.md            # Skill: research & source new investor leads
│       ├── score-leads.md             # Skill: score/rank leads against raise criteria
│       ├── draft-outreach.md          # Skill: draft personalized investor emails
│       ├── manage-pipeline.md         # Skill: update lead statuses, log interactions
│       ├── follow-ups.md             # Skill: check for due follow-ups, draft reminders
│       ├── manage-materials.md        # Skill: manage deck, one-pager, data room docs
│       └── pipeline-report.md         # Skill: generate pipeline summary & stats
│
├── data/
│   ├── raise-config.json              # Raise parameters (amount, stage, sector, terms)
│   ├── leads/
│   │   ├── _index.json                # Lead index with IDs, statuses, scores
│   │   ├── lead-001.json              # Individual lead files (investor + outreach history)
│   │   ├── lead-002.json
│   │   └── ...
│   ├── outreach/
│   │   ├── drafts/                    # Email drafts ready for review/send
│   │   ├── sent/                      # Sent email records
│   │   └── threads/                   # Ongoing email conversation threads
│   └── research/
│       ├── investor-lists/            # Raw research output from sourcing runs
│       └── market-intel/              # Market context, comparable deals, etc.
│
├── materials/
│   ├── deck/                          # Pitch deck versions
│   ├── one-pager/                     # One-pager / teaser
│   ├── data-room/                     # Data room documents (financials, legal, etc.)
│   ├── faq.md                         # Common investor questions & answers
│   └── talking-points.md             # Key narratives & talking points
│
└── templates/
    ├── cold-email.md                  # Email templates (cold outreach)
    ├── warm-intro-request.md          # Template for asking for warm intros
    ├── follow-up.md                   # Follow-up email template
    ├── meeting-prep.md                # Pre-meeting brief template
    └── thank-you.md                   # Post-meeting thank you template
```

---

## Step-by-step Implementation

### Step 1 — Foundation: CLAUDE.md & Raise Config
- Create `CLAUDE.md` with Vester's company context, raise parameters, and master instructions for the fundraising agent
- Create `data/raise-config.json` with structured raise details:
  - Company: name, stage, sector, location, one-liner, traction metrics
  - Raise: target amount, instrument (SAFE/priced), valuation range, use of funds
  - Ideal investor profile: stage preference, sector focus, check size, value-add priorities
- Create directory structure for data, materials, and templates

### Step 2 — Lead Sourcing Skill (`source-leads.md`)
- Skill that uses **WebSearch** and **WebFetch** to research investors
- Search strategies: VC firm portfolio pages, AngelList, Crunchbase, recent funding news, sector-specific investor lists
- For each lead found, creates a structured `lead-XXX.json` with:
  - Investor profile (name, firm, title, focus, check size, portfolio companies)
  - Source & discovery context
  - Fit hypothesis (why this investor might be good for Vester)
- Deduplicates against existing leads in `data/leads/_index.json`
- Saves raw research to `data/research/investor-lists/`

### Step 3 — Lead Scoring Skill (`score-leads.md`)
- Reads `data/raise-config.json` for scoring criteria
- Scores each unscored lead (0-100) on:
  - Stage fit, sector fit, check size match
  - Portfolio synergy (do they invest in adjacent/complementary companies?)
  - Recent activity (are they actively deploying?)
  - Geographic alignment
  - Value-add potential (board seats, operational help, network)
- Updates lead files with score + reasoning
- Updates `_index.json` with sorted rankings

### Step 4 — Email & Outreach Skill (`draft-outreach.md`)
- Reads lead profile + Vester context to draft personalized emails
- Supports multiple outreach types:
  - **Cold email** — direct outreach to investor
  - **Warm intro request** — email to mutual connection asking for intro
  - **Follow-up** — nudge after no response
  - **Meeting prep** — pre-meeting brief with talking points
  - **Thank you** — post-meeting follow-up
- Saves drafts to `data/outreach/drafts/` for review before sending
- Uses templates from `templates/` as starting points, personalizes heavily
- Email integration: reads/drafts emails (requires email MCP server setup)

### Step 5 — Pipeline Management Skill (`manage-pipeline.md`)
- Update lead status: `new → researched → contacted → meeting → diligence → term_sheet → committed → passed`
- Log interactions (calls, meetings, emails) with dates and notes
- Track warm intro paths (who knows whom)
- Flag stale leads (no activity in X days)

### Step 6 — Follow-up Skill (`follow-ups.md`)
- Scans all active leads for overdue follow-ups
- Drafts follow-up emails based on last interaction context
- Prioritizes by lead score and time since last contact
- **Scheduled task**: runs daily to surface what needs attention

### Step 7 — Materials Management Skill (`manage-materials.md`)
- Track versions of pitch deck, one-pager, data room docs
- Maintain `materials/faq.md` — add new Q&As after each investor meeting
- Update `materials/talking-points.md` with refined narratives
- Checklist: what's ready, what needs updating before next meeting

### Step 8 — Pipeline Report Skill (`pipeline-report.md`)
- Generate summary dashboard as markdown:
  - Total leads by status (funnel view)
  - Top 10 leads by score
  - Outreach activity this week
  - Upcoming follow-ups & meetings
  - Materials status
- Can be run on-demand or as a scheduled weekly digest

### Step 9 — Scheduled Tasks & Hooks
- **Daily**: Run follow-up check, surface overdue items
- **Weekly**: Generate pipeline report, run a sourcing sweep for new leads
- **On session start**: Show pipeline snapshot (active leads, due follow-ups)
- Configure via `.claude/settings.json`

### Step 10 — Email MCP Server Integration
- Set up email MCP server for Gmail/Outlook access
- Enable skills to read incoming investor replies
- Enable skills to draft and send emails directly
- Thread tracking: link email conversations to lead records

---

## Skill Summary

| Skill | Trigger | What it does |
|---|---|---|
| `/source-leads` | On-demand / weekly schedule | Web research to find new investor leads |
| `/score-leads` | After sourcing / on-demand | Score and rank leads by fit |
| `/draft-outreach` | On-demand | Draft personalized investor emails |
| `/manage-pipeline` | On-demand | Update lead statuses, log interactions |
| `/follow-ups` | Daily schedule / on-demand | Surface overdue follow-ups, draft reminders |
| `/manage-materials` | On-demand | Manage deck, data room, FAQ, talking points |
| `/pipeline-report` | Weekly schedule / on-demand | Generate pipeline summary & stats |

---

## Key Design Decisions

| Decision | Choice | Rationale |
|---|---|---|
| Runtime | Claude Max (Claude Code) | No infra to maintain, runs in your subscription |
| Database | File system (JSON + Markdown) | Git-trackable, human-readable, zero setup |
| Orchestration | Skills + Scheduled Tasks | Native Claude Code primitives, composable |
| Email | MCP server integration | Direct inbox access for drafting & reading |
| Templates | Markdown files | Easy to edit, version-controlled, personalizable |

---

## What's NOT in v1 (future scope)
- Calendar integration (auto-schedule meetings)
- CRM sync (HubSpot, Affinity, Attio)
- Investor update email automation
- Cap table modeling
- Term sheet comparison tool
