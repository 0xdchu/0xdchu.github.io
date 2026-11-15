# Pilot acceptance checklist

Fill one row per deliverable, at the end. Three outcomes only: `pass` (with evidence), `fail` (with reason), `not checked` (with reason). "Looks good" is not an outcome.

## A. Existence and count

| Check | Result | Evidence |
|---|---|---|
| Every file in the scope table exists at its stated path | | |
| Every count matches exactly (10 posts = 10 files) | | |
| Every image opens and is the stated pixel size | | |
| No file is a placeholder, a stub or a copy of another file | | |

## B. Truthfulness

| Check | Result | Evidence |
|---|---|---|
| Product name, price and claims match the client's own source | | |
| Every number in the copy traces to the positioning proof list or the client | | |
| No invented statistic, testimonial, user count or revenue figure | | |
| No claim about a competitor that has no public source | | |
| Anything unverifiable is marked as such, not printed as fact | | |

## C. Fit

| Check | Result | Evidence |
|---|---|---|
| Positioning brief exists, approved, and the copy follows its hierarchy | | |
| Voice file applied; ban list respected (grep the drafts for banned words) | | |
| Each post is native to its platform per `formats/`, not one text reused | | |
| Landing copy answers the top 3 objections from the brief | | |
| Design assets use the approved palette and type scale | | |

## D. Gates

| Check | Result | Evidence |
|---|---|---|
| Usop wrote PASS/FAIL per item, rounds ≤ 2 | | |
| Owner approved each deliverable in writing, with date | | |
| Nothing was published, scheduled or sent by the team | | |
| No client credential was requested or stored | | |
| Out-of-scope requests logged in `out-of-scope.md` with date and requester | | |

## E. Delivery

| Check | Result | Evidence |
|---|---|---|
| Delivery report lists every item with its evidence path | | |
| Outstanding items and unknowns stated in one place | | |
| Handoff message to the client tells them what to approve and what is not included | | |

## Sign-off

```
Delivered by:   Tsunade
Checked by:     <owner>
Date:
Result:         <all pass / n items open, listed>
Hours spent:    <number>   (to compute the real margin)
What broke:     <one line per failure that a future project must avoid>
```

## Rule for the review after the pilot

Write down every failure with its root cause, and classify it: scope error, gate error, capability gap, or infrastructure fault. Only the first two are fixable by process. A capability gap means the deliverable leaves the offer. An infrastructure fault (a silent adapter, an expired approval, a stale cached prompt) means it gets fixed before the next client, because a client sees a missed date, not an excuse.
