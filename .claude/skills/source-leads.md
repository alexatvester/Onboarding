# Source Leads

Research and find new investor leads for Vester's fundraise.

## When to Use
When the user wants to find new potential investors, expand the pipeline, or research a specific type of investor.

## Instructions

1. **Read context first:**
   - Read `data/raise-config.json` to understand what Vester is raising, the stage, sector, and ideal investor profile.
   - Read `data/leads/_index.json` to see existing leads and avoid duplicates.

2. **Research investors using web search:**
   - Search for VCs and angels that match the ideal investor profile (stage, sector, check size, geography).
   - Search strategies:
     - `"{sector}" seed investor 2025 2026` — find active sector investors
     - `"pre-seed" OR "seed" VC fund "{sector}" portfolio` — find firms with relevant portfolios
     - `angel investor "{sector}" "{location}"` — find angels
     - Search specific firm websites and portfolio pages for partner-level contacts
     - Look at who funded comparable/adjacent companies
   - If the user provides a specific focus (e.g., "find fintech investors in NYC"), narrow the search accordingly.

3. **For each investor found, research and capture:**
   - Full name, title, firm
   - Investment focus areas and stage preference
   - Typical check size range
   - Notable portfolio companies (especially ones relevant to Vester)
   - LinkedIn URL, email if publicly available
   - Location
   - A "fit hypothesis" — 1-2 sentences on why they'd be a good fit

4. **Create lead files:**
   - Get the `last_id` from `data/leads/_index.json`
   - For each new lead, increment the ID and create `data/leads/lead-{XXX}.json` following the schema in CLAUDE.md
   - Set `status` to `"new"`, `source` to how you found them, `source_date` to today's date
   - Update `data/leads/_index.json` with summary entries for each new lead

5. **Save raw research:**
   - Save the full research output to `data/research/investor-lists/{date}-sourcing-run.md`
   - Include search queries used, sources checked, and all investors found (even ones not added as leads)

6. **Report results:**
   - Summarize how many new leads were added
   - List the top leads by fit hypothesis
   - Note any notable findings or patterns
   - Flag any leads that might have warm intro paths

## Arguments
- The user may specify: sector focus, geography, investor type (VC/angel), check size range, or a specific firm/person to research.
- If no arguments, do a general sourcing run based on `raise-config.json`.
