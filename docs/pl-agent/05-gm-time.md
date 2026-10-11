# PL Agent — freeing up growth manager time (phase 2: delivery)

Confirmed: sales and growth managers (GM) are different people, and everyone
uses Outlook. So phase 1 (lead → signed) mainly saves **sales** time. GM time
is spent after signature, in delivery. That is where phase 2 focuses.

Principle: agents take over the **prepare, collect and chase** work. The GM
keeps the judgement and the relationship: the "so what" in a report, the
strategic call, the conversation.

## Where GM time goes (hypothesis, to verify with a one-week time log)

| # | Work | What eats the time |
|---|---|---|
| 1 | Reporting | Pulling numbers from Meta, Google Ads, LinkedIn, GA4 into a monthly report, writing commentary |
| 2 | Client communication | Weekly status mails, "how is it going?" questions, prepping and recapping client meetings |
| 3 | Account monitoring | Checking budget pacing, CPA/ROAS swings, disapproved ads, broken tracking, every day across every account |
| 4 | Internal coordination | Briefing designers and copywriters, chasing creatives, getting client approval on creatives |
| 5 | Project admin | Productive tasks and time, budget burn, overservicing, change requests, invoicing input |
| 6 | Onboarding | Kickoff, access, tracking audit (partly covered in phase 1) |
| 7 | Account growth | Quarterly reviews, renewals, spotting upsell |

## Agents for phase 2

| Agent | Takes over | GM keeps | Trigger | Systems |
|---|---|---|---|---|
| **Performance Watch** | Daily check of every account: pacing vs budget, CPA/ROAS anomalies, disapprovals, conversion tracking dropping to zero, limited delivery. Posts **only exceptions** in Slack with a proposed action. | Deciding the action | Daily 07:30 | Meta, Google Ads, LinkedIn, GA4 |
| **Reporting** | Builds the monthly report with all numbers, period comparison, top movers and a draft narrative. | Editing the 3–5 lines of insight and next steps | 1st working day of the month | Ad platforms, GA4, Productive |
| **Client Pulse** | Drafts the weekly status mail from platform results, Productive tasks done/next and open questions. Drafts replies to "how is it going?" mails with current numbers. | Approve or edit in Slack; sent from GM's Outlook | Friday 10:00; incoming client mail | Outlook, Productive, ad platforms |
| **Meeting** (extended from phase 1) | Prep brief before every client meeting (results since last call, open actions, agenda); recap after, with actions turned into Productive tasks. | Running the meeting, approving the recap | Calendar, Fathom | Outlook calendar, Fathom, Productive |
| **Inbox triage** | Classifies client mails (question, request, approval, complaint), drafts replies, turns requests into tasks, flags urgent items. | Sending, handling sensitive mails | Incoming Outlook mail | Outlook, Productive |
| **Creative Ops** | Turns a request from a call or mail into a brief and a Productive task for the designer; collects the result; sends it to the client for approval; chases approval. | Briefing quality check, final OK | Request detected; task done | Fathom, Outlook, Productive |
| **Scope & Budget Guard** | Compares hours burned in Productive with the plan; detects requests outside the agreed scope in mails and transcripts; drafts a change request. | Talking to the client about scope | Weekly; request detected | Productive, Outlook, Fathom |
| **Account Review** | Drafts the quarterly review: results vs goals, what worked, proposals, upsell signals; reminds before renewal dates. | Strategy and the conversation | Quarterly; 60 days before renewal | Ad platforms, HubSpot, Productive |

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

1. One-week time log with 2–3 GMs (categories above) to confirm the ranking.
2. Start with **Performance Watch** and **Reporting**: high volume, mostly
   internal, low risk, and the data is in platform APIs.
3. Then **Client Pulse** and **Meeting** at L1 (draft and approve).
4. Inbox triage, Creative Ops and Scope Guard once the action log and Slack
   approval flow are proven.

Measure per GM: hours per account per month before and after, and accounts
per GM. That ratio is the business case.

## Impact on phase 1

- Phase 1 approvals go to **sales** (follow-ups, recap after the discovery
  call, DocuSign). The GM comes in at handover.
- Open question: does the GM join the discovery or proposal stage? If so, the
  recap and access steps route to the GM instead.
- Outlook confirmed: agents send through Microsoft Graph as the owning person
  (send-as / delegated access), so mails land in that person's Sent items and
  threads.
