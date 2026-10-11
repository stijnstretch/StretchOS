# PL Agent — freeing up growth manager time (phase 2: delivery)

Confirmed: sales and growth managers (GM) are different people, and everyone
uses Outlook. So phase 1 (lead → signed) mainly saves **sales** time. GM time
is spent after signature, in delivery. That is where phase 2 focuses.

Principle: agents take over the **prepare, collect and chase** work. The GM
keeps the judgement and the relationship: the "so what" in a report, the
strategic call, the conversation.

## Where GM time goes

Confirmed so far:
- **Client communication is the GM's job**: weekly status mails, answering
  client questions, preparing and recapping client meetings, following up on
  promised actions. This is the main target of phase 2.
- **Reporting is not done by the GM.** Owner to be named; the Reporting agent
  supports that person.
- **Daily account monitoring is needed, but not on ROAS / CPA.** The check
  watches the KPIs agreed per client (see below).

Still to verify with a one-week time log: how much time goes to monitoring,
internal coordination, project admin, onboarding and account growth.

## Agents for phase 2

### Client communication (GM)

| Agent | Takes over | GM keeps | Trigger | Systems |
|---|---|---|---|---|
| **Client Pulse** | Drafts the weekly status mail: what was done (Productive), results on the client's KPIs, next steps, open questions for the client. | Approve or edit in Slack; sent from the GM's Outlook | Weekly, per client | Outlook, Productive, ad platforms |
| **Inbox triage** | Reads incoming client mail, classifies it (question, request, approval, complaint, urgent), drafts the reply with the facts filled in, turns requests into Productive tasks. | Sending; anything sensitive | Incoming Outlook mail | Outlook, Productive |
| **Meeting** | Prep brief before every client meeting (what happened since last time, open actions, suggested agenda). Recap after, sent as the GM, with actions turned into tasks. | Running the meeting, approving the recap | Calendar, Fathom | Outlook calendar, Fathom, Productive |
| **Promise tracker** | Keeps a list of everything the GM or the team promised the client in mails and calls, with dates. Warns before a promise is late; drafts the follow-up when it's done. | Deciding priorities | Continuous | Outlook, Fathom, Productive |
| **Client memory** | One always-current summary per client: goals, KPIs, stakeholders and their preferences, decisions, open topics. The GM never has to reread old threads before a call. | — | Updated after every mail and meeting | Outlook, Fathom, HubSpot |

### Monitoring and other work

| Agent | Takes over | Owner keeps | Trigger | Systems |
|---|---|---|---|---|
| **Performance Watch** | Daily check of every account on the client's agreed KPIs, budget pacing, disapproved ads, broken tracking and campaigns that stopped delivering. Posts **only exceptions** in Slack. | Deciding the action | Daily 07:30 | Meta, Google Ads, LinkedIn, GA4 |
| **Reporting** | Builds the monthly report with numbers and a draft narrative. | Reporting owner (not the GM) edits the insight | Monthly | Ad platforms, GA4, Productive |
| **Creative Ops** | Brief → designer task → client approval → reminders. | Final OK | Request detected | Fathom, Outlook, Productive |
| **Scope & Budget Guard** | Hours burned vs plan; requests outside the agreed scope; change request draft. | Client conversation | Weekly | Productive, Outlook, Fathom |
| **Account Review** | Quarterly review draft, renewal reminders. | Strategy | Quarterly | Ad platforms, HubSpot, Productive |

### KPIs per client, not fixed metrics

ROAS and CPA are not the standard. At handover, the KPIs from the proposal are
stored on the project (for example leads, cost per lead, reach, frequency,
traffic, spend pacing). Performance Watch, Client Pulse and Reporting all read
that KPI set, with a threshold per KPI for what counts as an exception. Which
KPIs are typical per service package is still to be decided.

## Levers that are not agents

- **Live dashboard per client** (Looker Studio or similar) so "how are we
  doing?" is self-serve. Fewer mails, and the Client Pulse mail can link to it.
- **Standard service packages** with standard report templates, standard
  onboarding and standard cadences. Agents work best on standard work.
- **Fewer meetings**: replace some weekly calls with the status mail plus a
  monthly call.
- **Move work to the right role**: hand creative chasing and admin to a project
  coordinator, supported by the agents, instead of the GM.

## Where to start

1. **Client Pulse** and **Meeting** at L1 (draft and approve): they hit GM
   communication time directly and every mail is checked.
2. **Inbox triage** with reply drafts, and **Client memory** next to it.
3. **Performance Watch** on the agreed KPIs per client.
4. Promise tracker, Creative Ops, Scope Guard once the Slack approval flow and
   the action log are proven.

Measure per GM: hours of client communication per account per month before
and after, and accounts per GM.

## Impact on phase 1

- Phase 1 approvals go to **sales** (follow-ups, recap after the discovery
  call, DocuSign). The GM comes in at handover.
- Open question: does the GM join the discovery or proposal stage? If so, the
  recap and access steps route to the GM instead.
- Outlook confirmed: agents send through Microsoft Graph as the owning person
  (send-as / delegated access), so mails land in that person's Sent items and
  threads.
