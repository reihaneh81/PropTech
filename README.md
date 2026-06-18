# PropDeal — Multi-Agent Property Deal Qualification

A reference implementation of a two-agent system that automates property deal qualification: one agent researches and validates listings, a second runs preliminary financial analysis, and the two hand off automatically to turn a raw listing into a fully assessed go/no-go recommendation.

> **Note:** This is a sanitized demonstration built with synthetic data and generic business logic. It illustrates the architecture and agent-handoff pattern from a production engagement — no client data, proprietary logic, or credentials are included.

---

## The Problem It Solves

Qualifying a property deal usually means hours of repetitive groundwork before a human decides whether it's worth pursuing: gathering listing data, cross-checking details against multiple sources, and modeling preliminary financials. At volume, this becomes a bottleneck — qualified opportunities slip through because the team can't process them fast enough.

This system automates the groundwork so humans only step in at the decision point, where their judgment actually adds value.

---

## How It Works

The system mirrors how a real deal team operates, with two specialized agents owning distinct roles:

```
   Raw listing
        │
        ▼
┌─────────────────┐     structured      ┌─────────────────┐
│  Property Agent  │  property profile   │  Finance Agent   │
│                  │ ──────────────────▶ │                  │
│ • gather data    │                     │ • yield estimate │
│ • validate       │                     │ • cost model     │
│ • build profile  │                     │ • risk scoring   │
└─────────────────┘                     └─────────────────┘
                                                  │
                                                  ▼
                                        go / no-go + reasoning
                                                  │
                                                  ▼
                                          Human reviewer
                                        (final decision only)
```

**Property Agent** — Handles the research side. It gathers listing information, validates property details against multiple (mock) data sources, and assembles a structured profile of the opportunity.

**Finance Agent** — Takes that profile and runs the preliminary analysis: estimated yield, cost projections, and risk indicators, producing a clear recommendation with the reasoning attached.

**Automatic handoff** — The property profile feeds directly into financial modeling, so a raw listing becomes a fully assessed opportunity with no manual step in between. A human reviews only the final recommendation.

---

## Architecture

| Component | Responsibility |
|---|---|
| `agents/property_agent.py` | Listing research, validation, profile assembly |
| `agents/finance_agent.py` | Yield/cost modeling, risk scoring, recommendation |
| `orchestrator.py` | Coordinates the handoff and manages agent state |
| `data/mock_sources.py` | Synthetic listing and market data (stand-in for real sources) |
| `schemas.py` | Shared data contracts between agents |

The two agents communicate through a structured profile defined in `schemas.py`. Keeping the contract explicit means either agent can be swapped or extended without breaking the other — the property agent could pull from real estate APIs and the finance agent wouldn't need to change.

---

## Tech Stack

- **Language:** Python 3.11+
- **Agent framework:** [your framework — e.g. LangGraph / custom orchestration]
- **LLM:** [your model/provider]
- **Data validation:** Pydantic for the inter-agent schemas

*Chosen for [a sentence on why — e.g. explicit state handling for the handoff, strong typing for the data contract between agents].*

---

## Running the Demo

```bash
# Clone and install
git clone https://github.com/[your-username]/propdeal-demo.git
cd propdeal-demo
pip install -r requirements.txt

# Set your LLM credentials
cp .env.example .env
# edit .env with your API key

# Run a sample qualification
python main.py --listing data/samples/listing_01.json
```

Expected output: a structured assessment of the sample listing, showing the property profile, the financial analysis, and a final go/no-go recommendation with reasoning.

---

## Example Output

```
PROPERTY PROFILE
  Location:        [synthetic] Stockholm, Södermalm
  Type:            2-bed apartment, 68 m²
  Asking price:    4,250,000 SEK
  Validation:      ✓ 3/3 sources consistent

FINANCIAL ANALYSIS
  Est. gross yield:    4.2%
  Est. monthly costs:  6,800 SEK
  Risk score:          Low-Medium (3/10)

RECOMMENDATION
  → PROCEED to human review
  Reasoning: Yield above target threshold (3.5%); costs within
  normal range; no validation flags. Suitable for closer analysis.
```

---

## What This Demonstrates

This repo is a portfolio reference for a multi-agent automation pattern. It shows:

- Designing agents around **existing human roles** rather than abstract tasks
- A clean **handoff contract** between agents that keeps them independently swappable
- Keeping a **human in the loop** at the point where judgment matters
- Translating a repetitive, multi-step workflow into an automated pipeline

The same pattern applies well beyond property: any workflow where research feeds into analysis feeds into a decision.

---

## About

Built by [Your Name], [agentic AI automation consultant]. This is a sanitized demonstration of production work.

📫 [your-email] · [your-website] · [LinkedIn]
