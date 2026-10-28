# Project format, v1

A project lives entirely in one private git repo: what it is, where it stands, what's next, and every change made to
it. Any person, tool or agent with access that can read files and use git can pick it up and move it forward. This spec says what must be
true of the repo, never how to do the work: approach the work however suits you, as long as the repo keeps these
guarantees.

## The project repo

A repo is a project when it has a `project.json` at its root. Project repos are private: their sessions hold real
conversations. Work on public repos (open source, or someone else's repo) is tracked from a private project repo that
lists them in `repos`.


```json
{
  "format": "project/1",
  "title": "Storefront relaunch",
  "owner": "Alice",
  "repos": [
    { "repo": "acme/storefront", "about": "the customer-facing site" },
    { "repo": "acme/orders-api", "about": "the orders service the site calls" }
  ]
}
```

- `format` (required): this spec's version.
- `title` (required): the project's name.
- `owner` (optional): the person the project answers to: who decides what it's for, approves what the boundaries
  below ask about, and agrees when it's done. Without one, the owner is whoever owns the repo.
- `repos` (optional): other repos the project's work may change. Each is `owner/name`, with a short `about`.

Alongside it:

| File | | What it holds |
|---|---|---|
| `README.md` | required | What the project is, and where it stands now. |
| `CHANGELOG.md` | required | One entry per commit to this repo, newest first (README and roadmap updates aside). |
| `ROADMAP.md` | optional | The goal, deliverables, next milestone and todos. |
| `sessions/` | when used | One file per session that committed here. |
| `AGENTS.md` | optional | This project's own facts and limits (below). |

Beyond these files, the repo can hold whatever the project needs (code, docs, notes, data), organized however suits
the project.

## README.md

Starts with the project's title and what it is in a few sentences. If the project is underway, a status line comes
next, dated: `**Status (2026-03-14):** …`. Keep it a current picture, rewritten as things change, never a log. It ends
with the format line (see [Boundaries](#boundaries-for-every-project)).

## CHANGELOG.md

One entry per commit to this repo, newest first, added in the same commit it describes:

```markdown
## 2026-03-14 · Plan the checkout redesign
Set the next milestone to a one-page checkout and broke it into five todos, after reviewing drop-off in the funnel.
Session: [sessions/20260314184500-plan-checkout.md](sessions/20260314184500-plan-checkout.md)
Also: acme/storefront#42
```

- The heading is the date (UTC) and the commit's subject line.
- One to three sentences on what changed and why, for someone deciding whether to look closer.
- `Session:` links the session file, if the commit came out of a session.
- `Also:` lists commits or pull requests the same work made in the project's other repos, if any.

A commit that changes only `README.md` or `ROADMAP.md` (a todo checked off, a status line brought up to date) doesn't
need an entry: those files say where the project stands now, and `git log` keeps their history. Merge commits don't need
an entry either. A pull request lands as one squashed commit with one entry. A repo that adopts the format starts its
changelog then; its earlier history stays in `git log`.

## ROADMAP.md (optional)

The project's intent at a high level: its goal, its deliverables and the next big milestone. If the project has
well-defined todos, with deadlines or definitions of done, they can go here too. How it's laid out (sections, todo
syntax, who owns what) is up to the project's owner. For example:

```markdown
# Roadmap

## Goal
What done looks like, or "Evergreen: ongoing" for a project that doesn't end.

## Next milestone
A one-page checkout, live by April. Done when the drop-off on the shipping step is under 20%.

## Todo
- [ ] Book the user interviews (due 2026-04-01)
- [x] Pull the funnel numbers
```

## Sessions

A session is one detailed back-and-forth working on the project: a person with an AI agent, with another person, or
on their own. Its session file holds the turns of that work, not necessarily every turn of a tool's session (a
conversation that also covered other things logs only the part about this project). A session that changes the repo
leaves exactly one commit and exactly one session file. The commit holds all of
it together: the work itself, whatever it changed in the README and roadmap (a session's whole purpose may be to
define the next milestone or sharpen the todos), its changelog entry and its session file. A session that changes
nothing leaves nothing. If the work is too big for one commit, it's two sessions. What a later session needs to know
goes into the repo (README, roadmap, docs), not into a long-running conversation.

Work in the project's other repos follows each repo's own rules (branches, pull requests, its own changelog if it
has one), and is linked from the project's changelog entry and session file.

The file is `sessions/<YYYYMMDDHHMMSS>-<slug>.md`, named by the session's start time in UTC:

```markdown
---
started: 2026-03-14T18:45:00Z
ended: 2026-03-14T19:30:00Z
tool: Claude Code
model: claude-opus-5-5
---
# Plan the checkout redesign

## Alice
Let's plan the checkout redesign. The funnel numbers are in docs/funnel.md.

## Agent
I'll read the roadmap and the funnel numbers first.
> read ROADMAP.md, docs/funnel.md

Most drop-off is on the shipping step. I'd make the next milestone a one-page checkout: …
```

- Every message of the work, in full, in order, each under the speaker's name.
- Each tool call is one line (`> ran …`, `> read …`, `> edited …`), never its full output.
- **Never secrets:** API keys, tokens, passwords, connection strings, login codes and links, private keys. Write
  `[redacted: API key]` instead.
- **Other people's private details** (health, family, money, conflicts, contact details) stay out, unless the project
  exists to hold them (a personal network, say), and never cross into another project.

## AGENTS.md (optional)

Facts and limits that are true of this project and that anyone working on it needs: "merging to main deploys to
production", "ask before changing the database schema", "never contact the client's customers". It describes the
project, not a way of working.

## Boundaries for every project

These hold unless the project's AGENTS.md says otherwise:

- Nothing reaches other people (email, messages, posts, publishing, invitations) without the owner's OK for that
  action.
- Nothing that costs money, signs up for an account or can't be undone without the owner's OK.
- Never rewrite published history: no force-pushes, no amending pushed commits.
- Secrets never go in the repo. Information from one organization's project never goes into another's.

Because they matter most, every README ends with them in one line, so they reach anyone who reads the README even if
they never open this spec:

```markdown
Follows [project format v1](https://github.com/daws/project-format). Nothing goes to other people, costs money or
can't be undone without the owner's OK; no secrets in the repo; never rewrite pushed history.
```

## Versions

`project/1` is this document. A change that a v1 repo wouldn't meet is a new version (`project/2`), with how to move
a repo up to it.
