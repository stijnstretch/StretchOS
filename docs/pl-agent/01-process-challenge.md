# PL Agent — challenge of the current process

Input: the written process "how we do it today + where a PL agent can help"
(Sales → First meeting → After first meeting → Client accepts proposal).

Goal restated: free up growth managers' (GM) time by automating the operational
steps around client contact, **without the client ever feeling that the
communication is automated**. Run it as an agentic workflow in the cloud,
operated from Slack, with a webapp that shows every action the agents took per
project.

---

## 1. Big-picture challenges

### 1.1 The document is about sales, the goal is about growth managers
Almost every step described happens *before* the client signs. The stated goal
is GM time, and GM time mostly goes to delivery: kickoff, access chasing,
weekly status mails, reporting, budget pacing, renewals.

- Do a one-week time log with 2–3 GMs before building, and rank steps by hours.
- Clarify roles: is "sales" a separate person, or does the GM do sales too?
  The agent needs to know who approves what.
- Treat the flow below as **phase 1 (lead → signed → handover)** and map
  **phase 2 (delivery)** next. Phase 2 is probably the bigger win.

### 1.2 Every trigger depends on CRM data discipline
"Called but didn't pick up", "deal moved to waiting for signature", "prospect is
triggered for next meeting" — these only work if someone sets a structured
field. If a rep writes "nvm, try later" in a note, the agent can't act.

- Make call **disposition** mandatory in HubSpot (No answer / Not now – event /
  Meeting booked / Not interested / Wrong person).
- Make the agent report inconsistencies (deal in "Proposal" with no meeting
  logged, etc.) instead of silently skipping.

### 1.3 "Never feel automated" needs explicit design rules
These rules apply to every client-facing message:

1. Sent from a real person's mailbox (GM or sales), never a no-reply or a
   marketing tool layout. Plain text, normal signature.
2. Threaded into the existing conversation where one exists.
3. Sent within business hours with a random offset (no 09:00:00 sharp, no
   weekends, Belgian holidays respected).
4. **Any reply stops all automation** for that contact and routes the reply to
   the owner in Slack.
5. Content refers to something specific (the call, the download, the meeting).
6. A cap on touches per contact per week.
7. Anything a human would write uniquely (recaps, proposals) is drafted by the
   agent and approved by a human until it has earned more autonomy (see §3).

### 1.4 Not one PL agent, but a team with one orchestrator
One agent that does everything is hard to test, hard to give scoped permissions
and hard to trust. Proposed: one **PL Orchestrator per project** that owns the
project state and the action log, plus specialist agents with narrow tool
access. See `03-agent-architecture.md`.

---

## 2. Step-by-step challenges

| # | Step today | Challenge | Proposed change |
|---|---|---|---|
| 1 | Inbound: first download → email 1 day later asking about podcast / content / newsletter | "First time" must exclude existing clients, open deals and contacts at a company we already talk to. A high-fit lead should go to sales, not get a newsletter question. Asking for a reply creates manual work. EU consent must be recorded. | Dedupe + enrich (Clay) + ICP score first. High fit → Slack alert to sales. Others → plain-text mail with **one-click preference links** that set HubSpot consent + lists. Any reply stops the flow. One soft reminder after 5 working days, then stop. |
| 2 | Outbound: called, no pickup → next working day email inviting to an event | Jumping to an event after one missed call skips the actual goal (a meeting) and reads like a sequence. What if no event is planned? | Lead with the meeting (booking link), mention the event only if one is planned in the next 8 weeks. Drafts batched in a daily Slack digest for approval. |
| 3 | Outbound: no meeting wanted, yes to event → email "we noted it, will let you know" | The promise "we will let you know" has no loop that delivers it. It breaks silently. | Add contact to an event-interest list. When an event is published, the agent drafts personal invites to that list + reminders. The event calendar becomes a source of truth. |
| 4 | First meeting booked → invitation sent | Already native in HubSpot Meetings; no agent needed for the invite itself. The value is in preparation and no-shows. | Keep the invite native. Add: prep brief for the GM 24h before (Clay, Meta Ad Library, site/tracking scan, past touchpoints, suggested questions). Detect no-shows and draft a reschedule mail. |
| 5 | After first meeting → AI summary sent to the prospect as recap | Highest risk to "never feel automated": wrong number, misheard name, internal remark sent straight to the prospect. | Fathom transcript → recap drafted in the GM's voice → GM approves/edits in Slack → sent from GM mailbox in the same thread. Agent also updates HubSpot (notes, next step, stage, tasks). |
| 6 | "If prospect is triggered for next meeting and access" → access flow (Meta, LinkedIn, Google) with landing page + explainer + UTMs | The trigger is implicit: who decides? A UTM click shows interest, not access. Platform list is incomplete. Full access before signing is a big ask. | Explicit GM button in Slack: *Request access / Send proposal / Nurture / Lost*. Agent sends partner/manager requests itself where possible (Meta Business Manager partner request, Google Ads manager link). Client page with per-platform checklist (GA4, GTM, Search Console, LinkedIn Campaign Manager, Merchant Center…). **Verify access via platform APIs**, tick off per platform. Reminders day 3 and 7, escalate to GM day 10. Read access for audit, full rights after signing. |
| 7 | Client accepts → deal to "waiting for signature" → DocuSign link, sales validates and sends | Fine as is. Missing: reminders and what happens when it is signed. | Keep. Add DocuSign reminders (3 and 7 working days) and use the signed webhook to move the deal to Closed won. |
| 8 | Internally: resourcing agent checks schedule, expertise, timeline → planning lead accept/decline | Checking capacity *after* signature means the proposal may promise a start date nobody can staff. | Move a **soft reservation** to proposal stage: agent proposes team + earliest realistic start date for the proposal; planning lead accepts/declines in Slack. Confirm at signature. |
| 9 | Budget created in Productive | Manual re-entry creates mismatches between quote and budget. | Create the Productive project + budget from the HubSpot quote line items; finance/PL confirms. Quote is the source of truth. |
| 10 | Sales checks that all calls/notes are connected to the deal | Manual check of everything. | Flip it: agent auto-associates Fathom calls, emails and meetings, and shows only the gaps for one-click confirm. |
| — | *(missing)* | The flow ends at signature. | Add a **handover pack** (goals, KPIs, stakeholders, promises made in calls, access status), internal Slack channel, kickoff invite draft. Add a **lost** path and **owner absence** handling (approvals reroute when the GM is on holiday). |

---

## 3. Autonomy levels (how agents earn trust)

Every action *type* (not every agent) has an autonomy level, stored as policy:

| Level | Name | What happens |
|---|---|---|
| L0 | Suggest | Agent posts a recommendation in Slack. Human does the work. |
| L1 | Draft & approve | Agent prepares; a human approves / edits / skips in Slack. |
| L2 | Act & notify | Agent acts, posts an FYI with an undo window (e.g. 15 min before send). |
| L3 | Autonomous | Agent acts; visible in the action log only. |

Promotion rule: when an action type was approved **without edits** in ≥ 95% of
the last 30 instances, the system proposes promoting it one level. The PL
decides. Any client complaint or wrong send demotes it immediately.

Start: everything client-facing at L1, internal bookkeeping at L2/L3.

---

## 4. Open questions for the team

1. ~~Is sales a separate role from the growth manager?~~ Yes, different people. See `05-gm-time.md`.
2. ~~Outlook for all senders?~~ Yes, everyone uses Outlook.
3. Where do proposals live today (HubSpot quotes, Google Docs, PandaDoc)?
4. Event calendar: who owns it and how often are events planned?
5. Is there a skills matrix today, or only Productive bookings?
6. Which platforms do we ask access for, per service line?
7. Shared Slack Connect channels with clients, or internal-only?
