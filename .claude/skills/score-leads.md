# Score Leads

Score and rank investor leads against Vester's raise criteria.

## When to Use
When the user wants to evaluate, score, or rank leads in the pipeline. Also useful after a sourcing run to prioritize new leads.

## Instructions

1. **Read context first:**
   - Read `data/raise-config.json` for the ideal investor profile and raise parameters.
   - Read `data/leads/_index.json` to identify leads that need scoring.

2. **Identify leads to score:**
   - By default, score all leads where `score` is `null`.
   - If the user specifies a lead ID or set of leads, score only those.
   - If the user says "re-score all," score every lead regardless of existing score.

3. **Score each lead (0-100) on these criteria:**

   | Criteria | Weight | Description |
   |----------|--------|-------------|
   | Stage fit | 20% | Do they invest at Vester's current stage? |
   | Sector fit | 20% | Do their focus areas align with Vester's sector? |
   | Check size match | 15% | Is Vester's raise within their typical check size? |
   | Portfolio synergy | 15% | Do they have relevant portfolio companies (complementary, not competitive)? |
   | Recent activity | 10% | Have they been actively investing recently? |
   | Geographic fit | 10% | Are they in a relevant geography or invest remotely? |
   | Value-add potential | 10% | Can they provide meaningful help beyond capital? |

4. **For each lead:**
   - Read the full lead file `data/leads/lead-{XXX}.json`
   - Calculate the score based on available information
   - Write clear `score_reasoning` explaining the score
   - Update the `fit_hypothesis` if new context was found
   - If information is missing to score accurately, note what's missing and do a web search to fill gaps
   - Save the updated lead file

5. **Update the index:**
   - Update `data/leads/_index.json` with new scores
   - Sort leads by score (descending) in the index

6. **Report results:**
   - Show a ranked table: ID, Investor, Firm, Score, Key Fit Reason
   - Highlight top tier (80+), strong (60-79), moderate (40-59), weak (<40)
   - Note any leads where you couldn't score confidently due to missing info
   - Suggest next actions (e.g., "Top 5 are ready for outreach")

## Arguments
- User may specify: specific lead IDs to score, "re-score all", or a modified weighting.
