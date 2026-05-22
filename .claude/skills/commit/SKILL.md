---
name: commit
description: Generate a commit message following project conventions. Use when the user says "commit", "commit this", "make a commit", or invokes /commit.
---

# Commit

Generate and execute a commit following the project's conventional commit format.

## Step 1: Understand the changes

Use `AskUserQuestion` to confirm:

1. **Work item ID** (auto-detect from branch name if possible)
2. **Commit type** (fix, feat, refactor, chore, docs, test, perf)

If the conversation already makes the type and work item obvious, skip the question and proceed.

## Step 2: Read the diff

```bash
git status
git diff -w HEAD
git log --oneline -5
```

Derive the message from the conversation context first. Fall back to the diff only when invoked with no prior discussion.

## Step 3: Generate and commit

Present the commit message in a code block for confirmation, then stage and commit.

<CRITICAL>
Never amend a previous commit without explicit approval. Create a new commit by default.
</CRITICAL>

## Format

```
type(scope): subject line

Optional body (ONLY when the subject alone is not enough).

AB#<work-item-number>
```

See [references/conventions.md](references/conventions.md) for the full format specification and examples.
