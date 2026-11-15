---
name: client-delivery
description: "Use when running a paid client project end to end: writing the scope, turning it into tasks with owners and dates, keeping the status file, checking acceptance with evidence, and reporting to the owner. Not for writing the client's content."
metadata:
  version: 1.0.0
---

# Client delivery

Rule that overrides everything else: **no claim without evidence**. A task is done when a file, link or number proves it, not when someone says so.

## 1. Scope file (before any work)

Write `work/client/<project>/scope.md` and get the owner's approval in writing. One page, no adjectives:

```
Client:            <name>
Product:           <one line, what it does and for whom>
Market:            <countries / languages>
In scope:          <numbered list, each item has a deliverable and a count>
Out of scope:      <numbered list: brand identity, logo, ads, publishing, credentials, ...>
Assumptions:       <what must be true, e.g. client supplies product screenshots>
Owner approver:    <one person, not a committee>
Client approver:   <one person>
Review window:     <e.g. 2 business days per round, max 2 rounds>
Start / end:       <dates>
Price / hours:     <only what was agreed>
```

Anything not in `In scope` is refused, even when asked nicely, and logged in `out-of-scope.md` with the date and who asked. That file is the defence when the invoice is questioned.

## 2. Task table

`work/client/<project>/tasks.md`, one row per task, nothing vague:

| # | Task | Owner | Input | Done when (checkable) | Due | State |
|---|---|---|---|---|---|---|

Rules:
- `Done when` must be checkable by someone who was not in the conversation: "3 banners, 1200×628 and 1080×1350, text from `brief-landing.md`, PNG in `assets/banners/`" — not "nice banners".
- One owner per row. A shared owner is no owner.
- No row without a due date. If the date is unknown, the row is not ready to be handed out.
- State is one of `todo`, `doing`, `review`, `done`, `blocked`. `blocked` needs the blocker named in one line.

## 3. Status file

`work/client/<project>/status.md`, rewritten on every change, not batched:

```
Updated: <date>
Done this period:  <task #> — <evidence path or link>
In review:         <task #> — waiting on <person> since <date>
Blocked:           <task #> — <blocker>, needs <owner decision>
Next:              <task #> — <owner>, due <date>
Risk:              <what could slip and by how much>
```

## 4. Acceptance check

Before reporting anything as delivered, walk the checklist in `acceptance.md` item by item and write the evidence next to it. Three outcomes only: `pass` with evidence, `fail` with the reason, `not checked` with the reason. Never "should be fine".

Minimum acceptance set for a marketing package:
- every deliverable exists at its stated path and opens;
- counts match the scope exactly (10 posts means 10 files, not 9 and a promise);
- the client's product facts match the client's own source (name, price, claim, URL);
- no placeholder text, no `TODO`, no lorem, no invented statistic, no invented testimonial;
- links resolve; screenshots show the real product, not a mock;
- the brand voice file was applied to every piece;
- anything the client must approve is listed in one message, not scattered.

## 5. Report to the owner

Weekly, five lines, in this order: done with evidence, in review, blocked, risk, decision needed. No praise, no narrative. If nothing moved, say nothing moved and why.

## 6. Escalate, do not absorb

Escalate to the owner, in one short message, when: scope must grow, a deadline will slip, the client asks for something the team cannot verify (a number, a guarantee, a competitor's private data), a deliverable is rejected twice, or credentials/access are needed. Absorbing a problem silently is the failure this role exists to prevent.
