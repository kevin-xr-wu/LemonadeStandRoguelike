---
name: coder
description: Rules for working in this codebase. Use before changing any file in this repo, and before any branch, commit, rebase, push or merge.
---

# Coder playbook

How Claude works in this repo. Several Claude sessions can run here at once, so each one keeps its work apart from the others.

## One session, one branch

- Every session works on its own branch, in its own git worktree. Never work on another session's branch or in its worktree.
- Start each new session's work from `main`: a new worktree on a new branch from `origin/main`. Use the `EnterWorktree` tool, or:
  ```sh
  git fetch -q origin main
  git worktree add .claude/worktrees/<branch> -b <branch> origin/main
  ```
  Then do all the work in that folder.
- If kevin-xr-wu gives you a PR to work on, work on that PR's branch instead, in a worktree of its own:
  ```sh
  git fetch -q origin <branch>
  git worktree add .claude/worktrees/<branch> <branch>
  ```
  If git says the branch is already checked out in another folder, another session is using it. Tell kevin-xr-wu before going further.
- Never switch branches in the main `lemonade_stand` folder. Another session may be working there.
- Before each commit, check that you're on your own branch: `git branch --show-current`.
- When your PR is merged or closed, remove your worktree: `git worktree remove .claude/worktrees/<branch>`.

## Each PR holds only its own change

- Before opening a PR, and again before merging it, check that it holds only this session's commits: `git log --oneline origin/main..HEAD`, and the PR's list of changed files.
- If you find your branch sitting on top of another branch, rebase your branch onto `origin/main` and leave the other branch alone. Only your commits come along. Then push your branch again:
  ```sh
  git fetch -q origin main
  git rebase --onto origin/main <newest commit that isn't yours> <your branch>
  git push --force-with-lease
  ```
- Leave other sessions' PRs and branches alone: don't merge, close, rebase or push to them.
