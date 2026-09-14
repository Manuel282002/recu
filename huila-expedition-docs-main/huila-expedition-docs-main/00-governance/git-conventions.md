# Git Conventions - HUILA TRAVEL EXPEDITION

> **Read this document before making your first commit on the project.**

## Branch strategy




main        ← Production / Delivery. Merge from release only. Always stable.└── dev   ← Continuous integration for the ADSO team. Merge from features.└── feat/[description]    ← One branch per user story (HU-01 to HU-20)└── fix/[description]     ← One branch per bugfix (e.g., calendar overlaps)└── chore/[description]   ← Infrastructure, local environment setup, or docs└── hotfix/[description]  ← Urgent fixes directly to main (if requested by Instructor)
**Rules:**
- Nobody commits directly to `main` or `dev`.
- Every task = one branch + one Pull Request (PR).
- One branch = one task (do not mix different features or user stories).
- Branches are deleted after a successful merge.

---

## Branch naming format

[type]/[description-in-kebab-case]Examples for HTE Project:feat/agency-registration-rntfix/booking-concurrency-overbookingchore/docs-initial-frameworkhotfix/laravel-bcrypt-hashing-error
---

## Commit format (Conventional Commits)

type: [lowercase description, imperative mood, no trailing period][optional body — explain WHY, not what][optional footer — issue/user story references]
**Types:**

| Type | When to use |
|------|-------------|
| `feat` | New functionality (e.g., adding municipality search filters) |
| `fix` | Bug fix (e.g., correcting image optimization arithmetic) |
| `docs` | Documentation only (e.g., updating governance rules) |
| `style` | Formatting, whitespace, Blade layout alignment (no logic change) |
| `refactor` | Code refactoring without behavior change (optimizing Eloquent queries) |
| `test` | Add or modify PHPUnit tests |
| `chore` | Tooling, Laravel dependencies, environment configuration, database migrations |
| `perf` | Performance improvement (complying with the 3-second page load rule) |

**Examples for HTE Project:**
feat(auth): implement secure login for agency profilesfix(booking): correct calendar slot validation to prevent overbookingCloses #12docs(governance): update team agile conventions based on SRSchore(deps): upgrade Laravel framework to version 10.x
---

## Pull Request policy

- **Size:** maximum 400 lines of code (excluding automated tests). If larger, split it into smaller sub-tasks.
- **Reviewers:** minimum 1 approval from the ADSO development team (Luisa, Juan, Natalia, or Manuel) before merging.
- **Review time:** reviewer has a maximum of 24 business hours to inspect the code.
- **Template:** use the template provided at `.github/pull_request_template.md`.
- **Green Local Run:** merge only proceeds if manual validation or automated PHPUnit local environment checks pass cleanly.

---

## Merge policy

- Use **Squash and Merge** for features (keeps `dev` branch history completely clean).
- Use **Merge Commit** for official releases to `main` (preserves full history for technical audit).
- **Do not** use Rebase & Merge (creates confusion in shared git history).

---

## Tags and versioning

Follow [SemVer](https://semver.org/): `MAJOR.MINOR.PATCH`

```bash
# When releasing an official increment to main for review
git tag -a v1.0.0 -m "Release v1.0.0: core modules, agency register with RNT and booking engine"
git push origin v1.0.0
```
