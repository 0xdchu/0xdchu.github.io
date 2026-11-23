---
name: positioning-brief
description: "Use when a product needs a positioning line, ICP, message hierarchy, proof points and objection answers before any content is written. Produces one brief file other roles read; invents no facts."
metadata:
  version: 1.0.0
---

# Positioning brief

This is the file every other role reads before writing. If it is vague, every post is vague. One page, written in the client's language, reviewed by the owner.

## Non-negotiable: facts come from the client

Ask for these and stop if they are missing: product name, what it literally does, price and plan structure, who already pays, the two strongest product facts (integration, speed, output quality — whatever is checkable), what it replaces, and 3 competitors the client names itself. Anything you cannot source goes in `Assumptions` or `Unknown`, never into a sentence that reads like a claim.

## The brief

```
# Positioning — <client>

## 1. One line
<For [ICP], [product] is the [category] that [outcome], unlike [alternative].>

## 2. ICP, narrow
Primary:   <role, company size, situation, the trigger that makes them look>
Not for:   <who this is wrong for, stated plainly>
Evidence:  <where the ICP claim comes from — client data, calls, nothing>

## 3. Category and alternative
Category we compete in: <name it; an unnamed category loses>
What they use today:    <the real alternative: a person, a spreadsheet, a tool, doing nothing>

## 4. Message hierarchy
1. <headline idea — the one thing>
2. <proof 1>
3. <proof 2>
4. <what it replaces, with the cost of the old way>

## 5. Proof points (only checkable ones)
- <claim> — <source: product doc, screenshot, number the client supplied>
- Anything unverifiable is marked `unverified` and never printed as fact.

## 6. Objections and answers
| Objection | Answer | Evidence |
|---|---|---|

## 7. Words we use / words we ban
Use:   <client's own vocabulary>
Ban:   <AI tells: "revolutionary", "supercharge", "game-changing", "seamless", "it's not X, it's Y">

## 8. Unknown
<everything still missing, with who can supply it and by when>
```

## Rules

- **Say the segment narrowly enough to lose customers.** "For SaaS founders doing 20+ posts a month" beats "for businesses".
- **Name the alternative.** If the alternative is "they do it manually", say so; that is where the pain argument comes from.
- **No invented numbers.** A statistic without a source is deleted, not softened.
- **Write the banned-words list before writing content**, so the writers cannot drift into AI tells.
- **One page.** If it needs three pages, it is not a position yet.
- **Test it**: read the one line to someone who does not know the product. If they ask what the category means, rewrite it.

## Handoff

Store as `work/client/<client>/positioning.md`. Ronin, Quintaro, Lelouch and Zoro read it before their first draft; Yugi derives `voice.md` from it plus the client's real writing; Usop rejects any piece that contradicts the hierarchy or prints an unverified claim.
