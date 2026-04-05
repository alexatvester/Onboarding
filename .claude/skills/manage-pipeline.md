# Manage Pipeline

Update lead statuses, log interactions, and maintain the fundraising pipeline.

## When to Use
When the user wants to update a lead's status, log a meeting or call, record notes, mark an email as sent, track warm intro paths, or make any changes to lead records.

## Instructions

1. **Read context first:**
   - Read `data/leads/_index.json` to see the current pipeline.
   - Read the specific lead file(s) being updated.

2. **Supported actions:**

   ### Update Lead Status
   - Valid status transitions: `new → researched → contacted → meeting → diligence → term_sheet → committed → passed`
   - A lead can be marked `passed` from any stage.
   - Update the lead's `status` field in both the lead file and `_index.json`.
   - Set `updated_at` to today's date.
   - If moving to `contacted`, ensure there's an outreach record.
   - If moving to `passed`, ask for (or note) the reason.

   ### Log an Interaction
   - Add an entry to the lead's `outreach_history`:
     ```json
     {
       "type": "meeting|call|email|note",
       "date": "YYYY-MM-DD",
       "status": "completed",
       "summary": "Brief description of what happened",
       "notes": "Detailed notes if provided"
     }
     ```
   - Update `next_action` and `next_action_date` based on what was discussed.

   ### Mark Outreach as Sent
   - Move the draft from `data/outreach/drafts/` to `data/outreach/sent/`
   - Update the corresponding `outreach_history` entry status from `drafted` to `sent`
   - Update the lead's status to `contacted` if this is the first outreach

   ### Track Warm Intro Path
   - Update the lead's `warm_intro_path` field with the connection details
   - Format: "Person Name (relationship) → Investor Name"

   ### Bulk Updates
   - If the user wants to update multiple leads at once (e.g., "mark leads 5, 8, 12 as contacted"), process them all.
   - Always update both individual lead files and `_index.json`.

   ### Add/Update Tags
   - Tags are freeform strings on each lead for filtering (e.g., "fintech", "warm-intro", "high-priority")
   - Add or remove tags as requested

3. **Always maintain consistency:**
   - Every change to a lead file must also be reflected in `_index.json`
   - All dates use ISO format (YYYY-MM-DD)
   - Never delete outreach records or interaction logs — only append
   - Set `updated_at` on every modification

4. **Report the changes:**
   - Confirm what was updated with a brief summary
   - Show the lead's current status and next action
   - If multiple leads were updated, show a summary table

## Arguments
- User specifies: lead ID(s), the action to take, and relevant details (status, notes, date, etc.)
- If the user just says "update pipeline" without specifics, show current pipeline status and ask what they'd like to change.
