# PL Agent — internal first: drafts go to the people, not to the client

Decision: in the first phase **agents never send anything to a client**. Every
client-facing message is delivered internally to the person who owns it
(growth manager (GM) for client communication, sales for sales follow-ups).
That person reviews it and sends it themselves.

Why:
- Zero risk of a wrong mail reaching a client.
- GMs and sales already live in Outlook; no new tool to learn.
- No "send as" permissions needed at the start.
- We learn what the agents get wrong before giving them more autonomy.

## How the draft reaches the GM

Preferred: the agent writes the draft **directly into the GM's Outlook Drafts
folder** (Microsoft Graph), as a reply in the existing client thread where one
exists. The GM opens it, edits, presses Send. No copy-paste, the thread stays
intact, the mail goes out from the GM's own mailbox.

Next to that, one **internal email** to the GM per batch, not one per draft:

```text
From:    PL Agent <pl-agent@stretch…>
To:      GM
Subject: [PL Agent] Client X: weekly status ready to send

Draft is in your Outlook Drafts, in the thread "Re: Weekly update".
Based on: 6 tasks done in Productive, KPI results week 41, 2 open questions.
Watch out: lead volume -18% vs last week (tracking checked, works).

Not sending it this week? Reply "skip".
```

Rules for these internal mails:
- Subject always starts with `[PL Agent]` and the client name, so an Outlook
  rule can file them in one folder.
- Bundle: per GM a short morning mail with everything that is ready, plus a
  separate mail only for urgent items (complaint, tracking down).
- Every mail says what the draft is based on, so the GM can trust it quickly.
- Reply keywords for simple decisions: `skip`, `later`, `done`.

Where Outlook drafts are not possible (for example a draft for a new contact
with no thread), the draft text sits in the internal mail itself.

## How the system still knows what happened

The webapp and the action log must stay accurate without the GM doing extra
work:

| Signal | How it is detected | Logged as |
|---|---|---|
| Draft created | Agent wrote it to Drafts | `proposed` |
| Sent unchanged | Matching mail appears in the GM's Sent items, text equal | `approved` |
| Sent with edits | Matching mail in Sent items, text differs; diff stored | `edited` |
| Not sent after X days | Draft still in Drafts or deleted | `skipped` |
| GM replied "skip" | Reply on the internal mail | `skipped` |

The edit rate per type of message (status mail, recap, reply) is the evidence
for later giving the agent more autonomy.

## Rollout stages

| Stage | Client-facing messages | Internal alerts | Moves on when |
|---|---|---|---|
| **1. Internal first** (now) | Draft in Outlook Drafts + internal mail to the owner. Owner sends. | Internal mail | Edit rate per message type is low for a month and GMs trust the drafts |
| **2. One-click approve** | Same draft, approved in one click (from the mail or Slack); the system sends from the owner's mailbox | Mail or Slack, owner's choice | Approval without edits ≥ 95% over the last 30 per type |
| **3. Act and notify** | Low-risk types only (e.g. meeting confirmations, access reminders) are sent automatically, owner gets an FYI | Same | Per type, decided by the PL |

Recaps, status mails and replies to client questions stay at stage 1 or 2 for
the foreseeable future. Stage 3 is only for routine, factual messages.

## Effect on the other docs

- Autonomy level L1 ("draft and approve") is implemented as **draft in Outlook
  + internal mail** in stage 1.
- Slack stays for team-wide internal alerts (e.g. tracking broken across
  several accounts) and for the ops/PL view, but is not required for GMs in
  stage 1.
- "Send as" through Microsoft Graph is only needed from stage 2.
- Graph permissions needed in stage 1: create drafts and read Sent items for
  the GM and sales mailboxes (delegated or application access, to decide with
  IT), plus one internal sender mailbox for the agent.
