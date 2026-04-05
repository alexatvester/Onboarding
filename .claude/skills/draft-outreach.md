# Draft Outreach

Draft personalized investor emails for Vester's fundraise.

## When to Use
When the user wants to draft an email to an investor — cold outreach, warm intro request, follow-up, meeting prep brief, or thank-you note.

## Instructions

1. **Read context first:**
   - Read `data/raise-config.json` for Vester's raise details and company context.
   - Read the specified lead file `data/leads/lead-{XXX}.json` for investor details.
   - Read the relevant template from `templates/` as a starting point.

2. **Determine outreach type** (ask the user if unclear):
   - **cold-email** — direct first outreach to an investor
   - **warm-intro-request** — email to a mutual connection asking for an intro
   - **follow-up** — nudge after no response (Variant A), after initial interest (Variant B), or after meeting (Variant C)
   - **meeting-prep** — pre-meeting brief with investor background and talking points
   - **thank-you** — post-meeting follow-up with promised materials

3. **Personalize the email:**
   - Reference something specific about the investor: a recent investment, portfolio company, talk, blog post, or tweet.
   - If you don't have enough info to personalize, do a web search for the investor and their firm.
   - Connect Vester's story to their specific interests and thesis.
   - For cold emails: keep under 150 words, no attachments — offer to send deck.
   - For warm intro requests: include the forwardable blurb.
   - For follow-ups: add new value or traction context, don't just "bump."
   - For meeting prep: research the investor thoroughly and prepare likely questions.
   - For thank-you: reference specifics from the meeting (ask the user for meeting notes if needed).

4. **Save the draft:**
   - Save to `data/outreach/drafts/{lead-id}-{type}-{YYYY-MM-DD}.md`
   - Include metadata at the top: lead ID, investor name, firm, email type, date
   - The body should be the ready-to-send email text

5. **Update the lead file:**
   - Add an entry to `outreach_history` array:
     ```json
     {
       "type": "{outreach-type}",
       "date": "{YYYY-MM-DD}",
       "status": "drafted",
       "file": "data/outreach/drafts/{filename}"
     }
     ```
   - Update `next_action` and `next_action_date` as appropriate
   - Update `updated_at` to today's date

6. **Present the draft:**
   - Show the full email to the user for review
   - Highlight what was personalized and why
   - Suggest any changes or alternatives
   - Remind the user: "This is saved as a draft. Let me know when you'd like to mark it as sent."

## Arguments
- User should specify: lead ID (or investor name), and optionally the outreach type.
- If the user says "draft emails for top leads," draft cold emails for the top-scored uncontacted leads.
- For meeting prep, the user should provide the meeting date and format.
- For thank-you, the user should provide meeting notes or key discussion points.
