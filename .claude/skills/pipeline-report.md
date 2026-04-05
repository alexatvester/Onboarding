# Pipeline Report

Generate a summary of the fundraising pipeline with statistics and insights.

## When to Use
When the user asks for a pipeline overview, fundraise status update, or wants to see how the raise is progressing.

## Instructions

1. **Read context first:**
   - Read `data/raise-config.json` for raise target and parameters.
   - Read `data/leads/_index.json` for the full lead list.
   - Read individual lead files as needed for detail.

2. **Generate the report:**

   ### Raise Overview
   - Target raise amount (from raise-config.json)
   - Amount committed so far (sum of committed leads' check sizes)
   - Percentage of target reached
   - Number of days since raise started / days remaining (if timeline set)

   ### Pipeline Summary Table
   Show counts and total potential capital at each stage:
   | Stage | Count | Potential Capital | Avg Score |
   |-------|-------|------------------|-----------|
   | New | X | $X | X |
   | Researched | X | $X | X |
   | Contacted | X | $X | X |
   | Meeting | X | $X | X |
   | Diligence | X | $X | X |
   | Term Sheet | X | $X | X |
   | Committed | X | $X | X |
   | Passed | X | — | — |

   ### Conversion Rates
   - Contacted → Meeting: X%
   - Meeting → Diligence: X%
   - Diligence → Term Sheet: X%
   - Term Sheet → Committed: X%
   - Overall: Contacted → Committed: X%

   ### Top Leads (by score)
   - List top 5 active leads with: ID, Investor, Firm, Score, Status, Next Action

   ### Attention Needed
   - Leads with overdue follow-ups
   - Leads stale for 14+ days
   - Leads with no next action set
   - High-score leads (≥ 70) not yet contacted

   ### Activity This Week
   - New leads added
   - Outreach sent
   - Meetings held
   - Status changes

   ### Insights
   - Which lead sources are performing best
   - Average time between stages
   - Any patterns in passed leads (common reasons)
   - Recommendations for next steps

3. **Format:**
   - Use markdown tables for structured data
   - Include a brief executive summary at the top (2-3 sentences)
   - Keep it scannable — headers, bullets, bold for key numbers

## Arguments
- No arguments needed for a full report.
- User can request specific sections (e.g., "just show me conversion rates" or "who needs follow-up").
