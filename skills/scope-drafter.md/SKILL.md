---
name: jira-work-items
description: >
  Use this skill whenever the user wants to turn a meeting transcript, business requirements document (BRD), or a high-level request overview into draft Jira work items. Triggers include: scoping or refinement meeting write-ups, "create Jira tickets/stories/epics from this", "break this BRD into work items", turning an agreed solution into an Epic with stories and tasks, or any time engineers have discussed how to deliver a request and it needs capturing as deployable (Story) and non-deployable (Task) work items. Also trigger when a meeting ended with open questions that need a follow-up to the BA or client, and the user needs those information requests drafted. If there is a transcript, BRD, or feature request and the user wants Jira work items out of it, use this skill.
---

# Jira work items from transcripts and BRDs

This skill turns a scoping or refinement discussion into structured, review-ready Jira work items. It reads the request and the agreed solution, defines an Epic where one is warranted, breaks the work into Stories (deployable code) and Tasks (non-deployable work), and drafts follow-up information requests when the meeting left open questions for the BA or client.

The default output is Markdown drafts for your review. Creating items directly in Jira is a separate, opt-in step (see "Creating items in Jira").

---

## Inputs

This skill works from one or more of these. More inputs produce better drafts.

| Input | What it provides |
|---|---|
| Meeting transcript | The agreed solution, decisions, trade-offs discussed, owners, open questions |
| Business requirements document (BRD) | The formal request, scope, acceptance expectations, constraints |
| High-level overview | A short written summary of the request when no transcript or BRD exists |

If none of these are in the conversation yet, ask for them before going further. Request the transcript, the BRD, and a one or two line overview of what the request is. Accept any combination. Do not proceed on assumptions about what the request is.

---

## Work item taxonomy

| Type | Definition | Use it for |
|---|---|---|
| Epic | Container for a request that spans multiple work items or more than one sprint | The overall request being scoped |
| Story | A deployable code work item that ships through the CI/CD pipeline | RPG, Rails, DB2 migration, test code |
| Task | A non-deployable work item with no shipped code output | Research, documentation, config, access, coordination |

Not every request needs an Epic. Create an Epic only when the work breaks into multiple Stories or Tasks, or spans more than one sprint. A small request can be a single standalone Story or Task. Flag this rather than forcing an Epic over a one-line change.

### Story vs Task: the decision rule

Ask one question for each piece of work:

> Does completing this produce a code change that ships through the deployment pipeline?

- **Yes** to Story (deployable)
- **No** to Task (non-deployable)

Worked examples:

| Work discussed | Type | Why |
|---|---|---|
| Change to an RPG program (e.g. PN40T, PN56) | Story | Code that deploys |
| Rails migration, model, or endpoint change | Story | Code that deploys |
| New RPGUnit tests for a program | Story | Code that deploys, or folds into the parent Story |
| DB2 schema or constraint change via migration | Story | Code that deploys |
| Spike or investigation of an approach | Task | No deployable output |
| Writing or updating documentation | Task | No deployable output |
| Access review or security setup | Task | No code |
| Vendor coordination or follow-up | Task | No code |
| Manual config on the box, not pipelined | Task | Not shipped code |
| Environment setup across TTEST or TDEV | Task | Usually no deployable code |

When a single discussion item contains both (for example "investigate the approach, then build it"), split it: one Task for the spike, one Story for the build, with the Story blocked by the Task.

---

## Workflow

Follow these steps in order. Do not skip to drafting items.

### Step 1: Gather inputs

If the transcript, BRD, or overview is not already present, ask for them. Accept any combination of the three.

### Step 2: Analyze

Read everything provided. Extract:

- **The request**: what the client or business actually wants
- **The agreed solution**: what the engineers decided to build or do
- **Decisions and trade-offs**: options weighed, what was chosen and why
- **Affected systems**: programs, files, libraries, endpoints, environments named (e.g. PN#MAST, LPRODDTA, dev01)
- **Owners**: who was assigned or volunteered, if stated
- **Open questions**: anything left unresolved or flagged for the BA or client

Separate what was decided from what is still open. Do not resolve open questions yourself.

### Step 3: Define the Epic (if warranted)

If the work spans multiple items or sprints, draft the Epic using the template below. If it does not, skip the Epic and produce a standalone Story or Task.

### Step 4: Break the work into Stories and Tasks

Apply the Story vs Task rule to each piece of work. Draft each item using the templates below. Reference the agreed solution from the meeting in each description. Do not invent work that was not discussed.

### Step 5: Draft acceptance criteria and dependencies

Write acceptance criteria for each Story. Capture dependencies between items (blocks, blocked by, relates to). Where the team did not give enough detail to write criteria, mark it "to be confirmed" rather than guessing.

### Step 6: Surface gaps and open questions

List anything that blocks the work from being fully scoped: missing acceptance detail, undecided approach, data not provided, sign-off needed.

### Step 7: Draft follow-up requests (only if open questions exist)

If the meeting ended with a plan that needs further input from the BA or client, draft an information request for each party using the template below. One request per recipient. Only include questions that genuinely block scoping or delivery. If nothing is open, skip this step.

### Step 8: Present and offer Jira creation

Present the drafts. Offer to create them in Jira through the Atlassian connector once you confirm the drafts are correct. Do not create anything without explicit confirmation.

---

## Templates

### Epic

```
# Epic: [Title]

**Type:** Epic
**Summary:** [One line]
**Goal / business outcome:** [Why this exists, tied to the BRD or request]
**Scope:** [What is in]
**Out of scope:** [What is explicitly excluded]
**Components:** [e.g. RPG, Rails, IBM i, Payroll]
**Labels:** [from the discussion, do not invent]
**Source reference:** [BRD section / transcript moment / request origin]

**Acceptance criteria (epic level):**
- [Outcome that signals the request is met]

**Child items:**
- [Story / Task titles listed below]

**Open questions:**
- [Anything unresolved, or "none"]
```

### Story (deployable)

```
## Story: [Title]

**Type:** Story (deployable)
**Parent epic:** [Epic title, or "standalone"]
**Summary:** [One line of user or system value]
**Description:** [What and why. Reference the agreed solution from the meeting.]
**Technical notes:** [Programs, files, libraries, endpoints, environments affected]

**Acceptance criteria:**
- Given [context], when [action], then [result]
- [Add criteria as needed]

**Definition of done:**
- Code reviewed
- Unit tests pass (RPGUnit where applicable)
- Deployed through the pipeline
- [Other conditions the team stated]

**Estimate:** [draft, to confirm with the team]
**Dependencies:** [blocks / blocked by / relates to, or "none"]
**Source reference:** [BRD section / transcript moment]
```

### Task (non-deployable)

```
## Task: [Title]

**Type:** Task (non-deployable)
**Parent epic:** [Epic title, or "standalone"]
**Summary:** [One line]
**Description:** [What needs doing: research, documentation, config, access, coordination.]
**Deliverable:** [What done looks like, e.g. a decision, a document, access granted]
**Owner suggestion:** [if discussed, otherwise omit]
**Dependencies:** [blocks / blocked by / relates to, or "none"]
**Source reference:** [BRD section / transcript moment]
```

### Information request to BA or client

```
**To:** [BA or client name]
**Re:** [Request or epic title]

[One or two sentences of context: what was discussed and what the team landed on.]

To finalize the work items we need the following:

1. [Specific question]. Needed because [what it blocks].
2. [Specific question]. Needed because [what it blocks].

[Optional: deadline tie-in, e.g. this gates the work for the July payroll run.]
```

---

## Creating items in Jira

Drafting is the default. Creation is opt-in.

- Only create items in Jira after the user confirms the drafts are correct.
- Use the Atlassian connector if it is available. If it is not connected, say so and leave the drafts in Markdown.
- Create the Epic first, then create Stories and Tasks linked to it, then apply the dependency links between them.
- Never invent or assume ticket numbers. Jira assigns them on creation.
- Mirror the user's existing field conventions (components, labels) only where the discussion supports them.

---

## Style rules

- Sentence case for all headings.
- Active voice.
- One line summaries. Keep descriptions tight.
- Reference the source for every item so the work stays traceable to the BRD or transcript.
- Do not fabricate names, dates, ticket numbers, programs, or decisions.
- Mark anything unclear as "to be confirmed" rather than guessing.
- No em dashes.

## Hard constraints

- Do not resolve open questions on behalf of the BA or client. Surface them and draft the request.
- Do not invent work items that were not discussed.
- Do not create anything in Jira without explicit confirmation.
- Only draft communications for the follow-up information requests in Step 7. Do not draft any other emails, messages, or communications.
- Output Markdown only unless the user asks for Jira creation.
