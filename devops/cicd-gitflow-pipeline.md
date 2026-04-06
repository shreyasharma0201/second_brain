# CI/CD & GitFlow Pipeline

> **Category:** Software Engineering > DevOps > CI/CD & Version Control

---

## The Big Picture

Code travels from a developer's laptop to real users through two automated pipelines — **CI** (quality gates) and **CD** (deployment). GitFlow is the branching strategy that organises how that code is managed in between.

```
Write code → CI checks → Human approves → CD deploys → Users
```

---

## GitFlow Branching Strategy

| Branch | Role | Protected? |
|--------|------|-----------|
| `main` | Production-ready code, tagged releases | Yes |
| `develop` | Integration branch, working draft | Yes |
| `release/*` | Final QA before shipping | Yes |
| `feature/*` | Individual dev work | No |
| `hotfix/*` | Emergency fixes direct to main | No |

**Key rules:**
- Semantic versioning via git tags: `vX.Y.Z`
- PRs required to merge into protected branches
- Hotfixes must sync back to both `main` AND `develop`
- `CHANGELOG` maintained per release

---

## CI Pipeline (triggered on commit / pull request)

| Step | What | When |
|------|------|------|
| 1 | Linting | On commit |
| 2 | Unit testing | On commit |
| 3 | Integration testing | On PR |
| 4 | Smoke test | On PR |
| 5 | SAST scan (SonarQube) | On PR |
| 7 | Secret scan (Trufflehog) | On PR |
| 8 | **Build preview image** | On PR |
| 9 | SCA scan (Trivy) | On PR |

---

## CD Pipeline (triggered on GitHub tag / PR events)

| Step | What | When |
|------|------|------|
| 9 | Push preview image | On PR |
| 10 | Human approval | Manual gate |
| 11 | Delete preview image | On PR close |
| 12 | Build production image | On GitHub tag |
| 13 | Push production image | On GitHub tag |
| 14 | IAC / GitOps update | On GitHub tag |

---

## Preview Image vs Production Image

| | Preview | Production |
|-|---------|-----------|
| **Trigger** | PR opened | GitHub tag (e.g. `v1.2.0`) |
| **Purpose** | Test safely | Ship to real users |
| **Environment** | Staging, fake data | Live servers, real data |
| **Debug mode** | ON — verbose logs | OFF — optimised, minified |
| **Lifetime** | Deleted on PR close | Permanent, versioned |
| **Risk** | Zero — disposable | High — affects real users |

> Same code, completely different purpose and consequences.

---

## Ground-Level Complexities (Big Projects)

### Code problems
- **Merge conflicts** — two devs edit the same file; must resolve manually
- **Broken develop** — one bad PR blocks the entire team
- **Secret leaks** — API keys committed accidentally; requires key rotation + history cleanup
- **Flaky tests** — tests pass/fail randomly; erodes trust in the pipeline

### Branch problems
- **Long-lived branches** — drift too far from `develop`; merging becomes "merge hell"
- **Release branch bugs** — bugs found late require backporting to both branches
- **Hotfix chaos** — must merge to `main` AND `develop`; forgetting one reintroduces the bug
- **Version confusion** — unclear which tag is actually deployed where

### Team problems
- **Env mismatch** — works locally, fails in CI (different OS/library versions)
- **CI queue bottleneck** — everyone waits 40+ mins for pipelines
- **PR review backlog** — too many PRs, too few reviewers
- **Code ownership disputes** — who approves changes to shared modules?

---

## Key Tools Referenced

| Tool | Purpose |
|------|---------|
| GitHub Actions | CI/CD automation engine |
| SonarQube | Static code analysis (SAST) |
| Trufflehog | Secret/credential scanning |
| Trivy | Dependency vulnerability scan (SCA) |
| Docker | Container image building & registry |

---

## Mental Models to Remember

- **CI = quality firewall.** Nothing broken gets through.
- **CD = controlled launch.** Human approved, then automated.
- **Preview = sandcastle.** Safe to break, thrown away after.
- **Production = the real city.** One mistake affects everyone.
- **GitFlow = traffic rules.** Without them, 10 devs cause chaos.

---

*Learnt: {{date}} | Source: Company CI/CD pipeline diagram + GitFlow branching strategy*
