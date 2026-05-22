# Commit Message Conventions

## Format

```text
type: subject line

Optional body (ONLY when the subject alone is not enough).

#<issue-number>
```

## Types

`fix`, `feat`, `refactor`, `chore`, `docs`, `test`, `perf`

## Subject Line Rules

- Imperative mood, lowercase after the colon, no period
- Max 65 characters. CI enforces this on PR titles.
- Must complete the sentence: "If applied, this commit will ___"
- Front-load the important word for `git log --oneline` scannability

## Body Rules

- Wrap at 72 chars
- Explain the PROBLEM and WHY, not the implementation (the diff shows what changed)
- Use present tense for problem statements: "Terminal drops connection after 90s" not "Terminal was dropping..."
- Inverted pyramid: most important context first
- If the subject says it all, OMIT the body entirely

## Issue References

- `#XXXXX` on separate lines at the end
- Multiple work items: each on its own line

## When Body is Needed vs Not

**Subject only:** rename, typo fix, config change, single obvious fix, dependency bump

**Include body:** non-obvious fix needing problem context, unexpected behavior change, trade-off worth recording in git history

## Examples

```text
fix: skip signature prompt for tap-to-pay refunds

#29821
```

```text
fix: handle missing payment response on Interac timeout

Interac terminals drop the connection after 90s of silence. The
cashier sees a null response with no error, and the form submits
with stale data.

#30142
```

```text
feat: implement OR clause for campaign filters

#30200
```

## Anti-patterns

- No file lists (git show does this)
- No bullet dumps listing every change
- No subject restatement in body
- No self-reference ("This commit...", "This change...")
- No passive voice

## References

- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#specification)
