# Project format

A minimal, tool-neutral format for keeping a project entirely in a private git repo, so any person, tool or agent can
pick it up and move it forward: what it is, where it stands, what's next, and every change made to it.

The spec is [FORMAT.md](FORMAT.md). In short, a project repo has a `project.json` marking it as one, a README with the
current picture, a CHANGELOG with one entry per commit, an optional ROADMAP with the goal, next milestone and todos,
and a file in `sessions/` for each working session that committed. Each session leaves exactly one commit, so what the
next session needs is always in the repo.

This repo holds only the spec. It's public so any project can link to it, which is also why it isn't a project in the
format itself: project repos are private. Changes to the spec are in `git log`.
