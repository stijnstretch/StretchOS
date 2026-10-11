# PL Agent — recurring work (cadence engine)

The system has two kinds of triggers:

- **Event-driven**: something happens (form filled, deal moved, recording
  ready). Most of phase 1.
- **Time-driven**: the calendar says it's time (daily check, Friday status,
  monthly report, quarterly review). Most of a growth manager's (GM) delivery
  work.

Time-driven work gets its own component: the **cadence engine**.

## How it works

1. **Cadence template per service package** (Paid Social, Paid Search,
   Analytics, Full Funnel…). It lists every recurring task with its frequency,
   owner role, the agent that prepares it, how far ahead preparation starts,
   and the autonomy level.
2. **At handover** the template is copied onto the project. The GM adjusts per
   client: report due on the 5th instead of the 3rd, weekly call on Tuesday,
   no LinkedIn checks if there is no LinkedIn budget.
3. **Each occurrence** follows the same lifecycle:
   `scheduled → preparing → waiting for approval → done`, or `skipped` /
   `overdue`. The agent starts preparing ahead of time so the GM gets a
   finished draft, not a reminder.
4. **Calendar rules**: Belgian holidays and weekends shift a due date to the
   next working day; a paused project pauses its cadence; the cadence ends at
   the contract end date.
5. **Everything is logged** in the same action log as event-driven work, so the
   project timeline shows both.

## Example cadence (to validate with the GMs)

| Frequency | Task | Agent prepares | GM does | Level | Client-facing |
|---|---|---|---|---|---|
| Daily 07:30 | Account health: the client's agreed KPIs, budget pacing, disapprovals, tracking drops, stopped delivery | Performance Watch | Acts on exceptions only | L3 (alert) | No |
| Weekly, Monday 08:30 | Week plan per GM: everything due this week across all clients + open exceptions | Orchestrator | Reads it | L3 | No |
| Weekly, Friday 10:00 | Client status mail | Client Pulse | Approves / edits (GM owns client communication) | L1 | Yes |
| Weekly, Friday | Hours burned vs budget in Productive | Scope & Budget Guard | Acts if over plan | L3 (alert) | No |
| Monthly, last working day | Media spend reconciliation: actual spend vs client budget, next month's budgets proposed | Reporting | Approves budgets; agent never changes platform budgets on its own | L0 | No |
| Monthly, 1st–3rd working day | Invoice input to finance: media spend + fees per client | Deal Desk | Checks | L1 | No |
| Monthly, due 5th working day (prep starts day 1) | Monthly report | Reporting | Not the GM: reporting owner writes the insight lines | L1 | Yes |
| Monthly | Client call: prep brief before, recap after | Meeting | Runs the call | L1 | Yes |
| Monthly | Tracking health audit (conversions firing, consent mode, GA4 vs platform gaps) | Performance Watch | Fixes or briefs a specialist | L0 | No |
| Quarterly | Quarterly review deck draft + upsell signals | Account Review | Strategy, presents | L1 | Yes |
| Quarterly | Access review: who has access to client accounts, remove leavers | Access | Confirms removals | L1 | No |
| 60 and 30 days before contract end | Renewal reminder + renewal brief | Account Review | Talks to the client | L0 | No |
| Yearly | Annual plan and budget proposal | Account Review | Strategy | L0 | Yes |

## Data model additions

```text
cadence_templates   id, service_package, name, rrule, due_rule (e.g. "5th working day"),
                    owner_role, agent, prep_lead_days, autonomy_level,
                    client_facing, minutes_manual_estimate
recurring_tasks     id, project_id, template_id, rrule_override, due_rule_override,
                    owner_user_id, active, starts_on, ends_on
task_occurrences    id, recurring_task_id, due_at, prep_started_at,
                    status (scheduled|preparing|waiting_approval|done|skipped|overdue),
                    action_id, productive_task_id
```

## Where people see it

- **Slack**: Monday week plan per GM, a draft when it's ready, an overdue alert
  if a client-facing item passes its due date.
- **Webapp**:
  - *Project → Cadence*: upcoming and past occurrences, on time / late / skipped.
  - *GM workload*: a week view of every due item across all of a GM's
    clients. This shows capacity, i.e. how many accounts one GM can carry.
  - *Template editor*: per service package, owned by ops.
- **Productive**: each occurrence is mirrored as a Productive task on the
  project, so it sits in people's normal task list and time can be booked on
  it. The cadence engine stays the source of truth for the schedule.

## Open questions

1. Which recurring tasks do GMs actually have today, per service package?
2. Do you already use recurring tasks or templates in Productive?
3. Report due date and format: one standard, or per client? Who owns reporting?
4. Which KPIs are standard per service package (ROAS / CPA are not)?
