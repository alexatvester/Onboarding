# Manage Materials

Manage pitch deck, one-pager, data room documents, FAQ, and talking points.

## When to Use
When the user wants to create, update, review, or organize fundraising materials like the pitch deck, one-pager, data room, investor FAQ, or talking points.

## Instructions

1. **Read context first:**
   - Read `data/raise-config.json` for current raise parameters.
   - Check `materials/` directory for existing materials.

2. **Supported actions:**

   ### Review Materials
   - Read and review existing materials in `materials/` for accuracy and consistency with `raise-config.json`.
   - Flag any outdated numbers, stale metrics, or inconsistencies.
   - Suggest improvements for clarity, persuasiveness, and completeness.

   ### Create/Update One-Pager
   - Location: `materials/one-pager.md`
   - Should include: company overview, problem/solution, market size, traction, team, raise details, contact info.
   - Keep it concise — fits on one page when printed.
   - All numbers must match `raise-config.json`.

   ### Create/Update FAQ
   - Location: `materials/faq.md`
   - Common investor questions with prepared answers.
   - Categories: Business Model, Market, Traction, Team, Fundraise Terms, Competition, Use of Funds.
   - Answers should be concise, confident, and backed by data where possible.

   ### Create/Update Talking Points
   - Location: `materials/talking-points.md`
   - Key messages for investor meetings, organized by topic.
   - Include objection handling section.
   - Keep each talking point to 2-3 sentences max.

   ### Manage Data Room
   - Location: `materials/data-room/`
   - Track what documents exist and what's needed.
   - Standard data room checklist: corporate docs, financials, cap table, team bios, product demo, customer references, market research.
   - Create a `materials/data-room/index.md` checklist if one doesn't exist.

   ### Create/Update Pitch Deck Outline
   - Location: `materials/deck-outline.md`
   - Slide-by-slide outline with key points for each slide.
   - Standard order: Cover, Problem, Solution, Market, Product, Traction, Business Model, Team, Financials, Ask, Appendix.

3. **Consistency rules:**
   - All financial numbers must match `raise-config.json`.
   - Company name, stage, and raise details must be consistent across all materials.
   - If `raise-config.json` has changed since materials were last updated, flag the discrepancies.

4. **Report what was done:**
   - Summarize changes made or materials created.
   - List any discrepancies found between materials and raise config.
   - Suggest next steps for materials that still need work.

## Arguments
- User specifies which material to work on and the action (create, update, review).
- If no specifics given, do a review of all existing materials and report status.
