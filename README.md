# ai-marketing-agents

Kortix is open source. It is the open-source CompanyOS, and this repository is the smallest working version of one argument about marketing work: an agent is only as good as the voice, the claims and the send policy a team can read. The Kortix project inside it is called `marketing-desk`, and that name lives in `kortix.yaml`.

So the rules live in files. One holds the register and the banned words. One holds the claims, each with the page it came from and the day it was read. One names the actions that wait for a person. When a send goes out wrong, the question on the table is who let it go out, and the change request that carried the sentence answers with a name and a date.

## What is here

Six files carry the rules. This README is the seventh, and a `.gitignore` keeps the junk out.

| Path | What it decides |
| --- | --- |
| `kortix.yaml` | Which agent runs, what it may reach, when it wakes |
| `agents/marketing.md` | The persona, and the one job it does |
| `skills/marketing-voice/SKILL.md` | Register and banned words |
| `skills/claim-check/SKILL.md` | What a claim needs before it ships |
| `docs/claims/acme-november-webinar.md` | A worked claim list, markers included |
| `docs/a-week-of-runs.md` | One week, from brief to a rejected line |

## Setup

```bash
curl -fsSL https://kortix.com/install | bash
kortix login
```

Clone this repository, then run the next two commands from its root:

```bash
kortix validate
kortix ship
```

`kortix validate` checks the manifest and the files around it: an agent `.md` that is missing, a skill with no `skills/<name>/SKILL.md`, or a connector the manifest does not declare is an error. `kortix ship` validates again, commits, asks for any missing secret, pushes the branch and asks you to connect the connectors the manifest declares. Two are declared here: `kortix_slack` for the review thread and `kortix_email` for drafts that a person sends.

Start a session with a brief:

```bash
kortix sessions new --prompt "Draft the Northwind webinar invitation and check every claim against skills/claim-check"
```

The agent works on its own branch and opens a change request. Review it with `kortix cr ls`, `kortix cr diff 1` and `kortix cr merge 1`, or `kortix cr request-changes 1 --message "..."` to send it back with a note. The trigger in this manifest wakes the agent every Monday at 09:00 UTC to re-read the claim lists under `docs/claims/`.

Starting from nothing instead? `kortix init marketing-desk` creates a directory from the starter, and you replace its `kortix.yaml`, `agents/`, `skills/` and `docs/` with the ones here.

To use these files in a project you already have, copy them into its root. `kortix projects set --manifest <path>` only matters if you move `kortix.yaml` off its default path.

## The send policy

`policy.default_mode: risk` holds an unmatched connector call that writes or destroys data, and lets a read run. The starter default is `allow_all`, where nothing is held. Approval gates you set. Off until you set them.

Rules on a connector use three actions, `always_run`, `require_approval` and `block`, shown as Allow, Ask and Block. A held call pauses the agent's turn until a person decides, and `approvals.ttl` says how long it waits.

Every key is documented in the [manifest reference](https://kortix.com/docs/project/manifest).

## What stays a person's job

Taste. The voice file can ask for short, plain, specific sentences. It cannot hear one land wrong.

Strategy. Which audience, which offer, which quarter. The agent drafts against the brief it is given, and a bad brief produces a clean draft of the wrong thing.

The one claim nobody verified. `claim-check` writes `[NO SOURCE]` and stops the sentence there. Somebody finds the page or drops the line, and no prompt replaces that decision.

The merge. Nothing merges itself: work reaches `main` through a change request a person approves, unless an admin granted the merge capability. The agent here holds `project.file.read` and `project.gitops.push`, so it can open a change request and cannot merge one.

If the repo has to stay inside your own network, self-hosting covers it. Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free.

## The longer write-up

The page at https://aimarketingagentsguide.com/ is the longer write-up of how these vendors compare, with each claim dated to the page it came from. This repository is the part you can run: change the voice file, and every later draft inherits the change.

The product code is at [Kortix on GitHub](https://github.com/kortix-ai/suna).
