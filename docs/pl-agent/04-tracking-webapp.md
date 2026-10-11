# PL Agent — tracking webapp

Purpose: one place to see, per project, what the agents did, what waits on a
human, and whether the system actually saves GM time.

## Screens

1. **Portfolio** — all projects with current stage, owner, pending approvals,
   last agent action, health flag (stuck > N days in a stage, access missing,
   unsigned after reminders).
2. **Project timeline** — stage progress (Inbound → … → Handover), access status
   per platform, and the action log: time, agent, action, autonomy level,
   status, approver, link to the sent text / CRM record.
3. **Approval inbox** — the same items as Slack, for people who prefer the web.
   Edits are captured as diffs (input for the promotion rule).
4. **Agent performance** — per action type: volume, approval-without-edit rate,
   median time-to-approve, current autonomy level, promotion proposals.
   Estimated hours saved = volume × agreed minutes per manual action.
5. **Policies** — autonomy level per action type, touch caps, business hours,
   holiday calendar, kill switches.

## Data model (first cut)

```text
projects        id, hubspot_deal_id, client_name, owner_user_id, stage,
                productive_project_id, slack_thread_ts, paused, created_at
contacts        id, project_id, hubspot_contact_id, role, automation_paused
events          id, project_id, source, type, payload_ref, received_at
agent_actions   id, project_id, agent, action_type, step_id, trigger_event_id,
                autonomy_level, status (proposed|approved|edited|skipped|
                executed|failed|undone), output_ref, final_text,
                approver_user_id, approved_at, executed_at, error
approvals       id, action_id, slack_ts, decided_by, decision, diff, decided_at
policies        action_type, autonomy_level, touch_cap, minutes_saved_estimate,
                updated_by, updated_at
access_checks   id, project_id, platform, requested_at, verified_at, status
```

`agent_actions` is append-only for status changes (store transitions), so the
log is auditable.

## Metrics that matter

- GM hours saved per week (estimate, see above) — the headline.
- Approval-without-edit rate per action type — trust indicator.
- Lead → first meeting conversion, time from meeting to recap sent.
- Time from "request access" to all platforms verified.
- Time from signature to kickoff.
- Client replies to automated mails flagged as "feels automated" (manual tag) —
  should stay at zero.
