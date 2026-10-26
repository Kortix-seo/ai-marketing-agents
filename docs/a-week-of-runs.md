# A week of runs in an open-source Kortix project

Kortix is open source, and this walkthrough covers one week in this project: from the Monday trigger to a merged draft. The companies are placeholders and the commands are real.

## Monday, 09:00 UTC

The `weekly-claims-sweep` trigger starts a session for `marketing-lead`. The agent reads every list under `docs/claims/`, opens each source again, and finds one page that no longer carries the line a claim list quotes.

It opens change request 3 with one row of `docs/claims/acme-november-webinar.md` changed:

```diff
- | Join the Acme product tour on 4 November. | https://acme.example/events | 2026-09-08 |
+ | [STALE: the Acme events page no longer lists 4 November] | https://acme.example/events | 2026-09-08 |
```

Then it stops. The rewrite belongs to a person, because the source and the sentence have to agree on a date before anything goes out.

## Monday, 09:40

A brief arrives in Slack from the demand team: a webinar with Northwind on 12 November, one invitation email, the audience is the trial accounts, the deadline is Wednesday.

The session that handles it is a normal one:

```bash
kortix sessions new --prompt "Draft the Northwind webinar invitation for trial accounts, from the brief in Slack this morning"
```

## Monday, 10:15

The agent reads the brief, then `skills/marketing-voice/SKILL.md` and `skills/claim-check/SKILL.md`. It searches the repo for the last invitation to a trial audience and finds one from June. It drafts about 200 words into `drafts/2026-11-02-northwind-webinar.md` and opens change request 4, titled with the asset and the date.

Three sentences in the draft carry a claim: the webinar date, a line about what the product does in the first week, and a sentence about the plan the reader is on. Each lands in `docs/claims/northwind-november-webinar.md` with the page it came from and the day it was read. The third has no page behind it, so the draft carries `[NO SOURCE]` where the sentence would go, and the claim list row says the same.

## Tuesday, 09:10

The reviewer opens change request 4:

```bash
kortix cr ls
kortix cr diff 4
```

Three things to read in the diff: the draft, the claim list, and the fact that the agent touched nothing else. The reviewer checks the date against the calendar, opens the source page, and stops on the price sentence. That page changed after the claim list was written.

The reviewer sends it back with the reason:

```bash
kortix cr request-changes 4 --message "The plan price line came from a page that has changed. Drop it, or read the page again and date it. The date claim is correct."
```

The note wakes the session. It is a comment on the change request, so the next person to read the thread sees why the line went.

## Tuesday, 09:35

The agent re-reads the page, finds the figure moved, and replaces the sentence with `[STALE: plan price moved, rewrite needed]`. It pushes a second commit onto the same branch. The diff shows two commits and one open question, and the reviewer reads the history of that sentence instead of a flattened version of it.

A correction cost one edit, it carries a date, and the next draft of the same email starts from the corrected file.

## Wednesday, 10:00

The owner of the events page reads change request 3, replaces the November line with the new date, and merges it. That one took a day because the answer sat with a person who was travelling, which is the ordinary reason a draft waits.

## Wednesday, 11:00

The reviewer replaces the price sentence in change request 4 with a line that names the plan and leaves the price out, since the price is what the offer is about to change anyway. The claim list row goes with it.

The `[NO SOURCE]` sentence takes longer. The reviewer asks the demand team which page backs it, and the merge waits a day for the answer.

## Thursday, 09:20

The page arrives. The agent writes the sentence with its source and the day it read it, and the claim list gains a row. Then:

```bash
kortix cr merge 4
git pull
```

The merge is a person's action. The agent held `project.gitops.push`, which opened the change request, and did not hold `project.gitops.merge`.

## What the week left behind

Nobody had to argue about the voice file, because the draft followed it. The claims folder now carries a page and a date for every number in the email, and the change requests record who approved each send and which two sentences had to wait for evidence.

Next Monday the sweep runs again on the same files, and the June invitation is the one the next draft gets compared to.
