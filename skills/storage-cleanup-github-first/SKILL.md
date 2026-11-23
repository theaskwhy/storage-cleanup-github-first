---
name: storage-cleanup-github-first
description: Use when a computer is low on disk space from development work — finding large project folders, making sure anything worth keeping is backed up to GitHub (or cloud storage) first, then removing what will never be used again.
---

# Storage Cleanup, GitHub First

Free disk space without losing work. Rule: if it isn't on GitHub (or cloud storage) and might be needed, back it up first; if it will never be used again, delete it. By The Ask Why Company.

## Steps
1. **Measure:** largest folders (`du -sh */ | sort -h`), plus known heavy caches (package caches, build folders, simulators/emulators, Docker VMs, old worktrees, tool app data). The OS storage panel can lag — trust `df`/`du`.
2. **Classify each folder:** (a) git repo fully pushed → safe to delete local copy; (b) git repo with unpushed commits/branches/stashes → push to a backup branch first; (c) untracked files worth keeping → copy to cloud storage or a private repo; (d) regenerable (node_modules, build output, caches, DerivedData) → delete; (e) unknown → ask the owner.
3. **Dry run:** write a plan file (path, size, class, action, why) and a script that prints what it would do. Get a human OK.
4. **Apply:** the script pushes backups first and verifies each push (compare local and remote SHAs) before deleting; logs every deletion (path, size, reason) to a TSV; exact paths only, no wildcards; never touches folders marked private/custody.
5. **Prune:** `git worktree prune` in repos whose worktrees were removed.
6. **Report:** before/after free space, what was pushed, what was deleted, what still needs a decision.

## Rules
- Simulators/emulators: keep only the device profiles actually tested on.
- Never delete secrets folders, signing keys or anything the owner hasn't approved in the dry run.
- Agents may be blocked from deleting; hand the owner the reviewed script to run.
