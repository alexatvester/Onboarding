# Follow-ups

Check for overdue follow-ups and draft reminder emails.

## When to Use
When the user wants to see what follow-ups are due, check on stale leads, or get a daily action list. Can also be run as a scheduled daily check.

## Instructions

1. **Read context first:**
   - Read `data/raise-config.json` for raise timeline context.
   - Read `data/leads/_index.json` to get all active leads.

2. **Scan for overdue follow-ups:**
   - Read each active lead file (status is not `passed` or `committed`).
   - Check `next_action_date` — if it's today or in the past, it's due.
   - Check `outreach_history` — if the last outreach was sent more than 5 business days ago with no response logged, flag it.
   - Check for stale leads — any lead with `updated_at` more than 14 days ago that isn't in a terminal state.

3. **Categorize and prioritize:**

   | Priority | Condition |
   |----------|-----------|
   | Urgent | `next_action_date` is past due by 3+ days, lead score ≥ 60 |
   | Due today | `next_action_date` is today |
   | Coming up | `next_action_date` is within 3 days |
   | Stale | No activity in 14+ days, not in terminal state |
   | No follow-up set | Active lead with empty `next_action_date` |

4. **Report the follow-up list:**
   - Show a prioritized table: Priority, Lead ID, Investor, Firm, Score, Last Action, Days Since, Suggested Action
   - Group by priority level
   - Highlight the most important items

5. **Draft follow-up emails (if requested or if urgent items exist):**
   - For each overdue follow-up, read the lead's outreach history to understand context
   - Select the appropriate follow-up variant from `templates/follow-up.md`:
     - Variant A: No response to cold outreach
     - Variant B: Initial interest but stalled
     - Variant C: Post-meeting, waiting on decision
   - Personalize using the lead's profile and last interaction
   - Save drafts to `data/outreach/drafts/`
   - Update the lead's `outreach_history` and `next_action`

6. **Summary stats:**
   - Total active leads: X
   - Overdue follow-ups: X
   - Due today: X
   - Coming up (next 3 days): X
   - Stale leads needing attention: X

## Arguments
- User can specify: "check follow-ups", "draft follow-ups for overdue leads", or a specific lead ID.
- Default behavior (no arguments): scan and report, ask before drafting.
