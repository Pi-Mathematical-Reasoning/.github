[![Pi: irrationally good at math](https://raw.githubusercontent.com/Pi-Mathematical-Reasoning/pi-branding/main/assets/banner/pi-readme-banner.png)](https://github.com/Pi-Mathematical-Reasoning/pi)

**Pi is a small open-weight model trained deeply in mathematics, with every
proof checked by Lean 4.** It is local-first: you ask it for a proof on your
own laptop and get back a Lean 4 file that anyone can check independently.
Given a compute budget, Pi returns either a complete, `sorry`-free proof or a
record of where it stopped.

The research question is whether a model small enough to run on a personal
laptop can, after math-specific fine-tuning, solve more unseen problems than
its original checkpoint, and eventually outperform a larger model on the same
tasks.

### How Pi works

1. **Measure the baseline.** Pi runs a pinned checkpoint on pinned
   benchmarks, starting with answer accuracy on MATH-500, and records
   everything needed to reproduce the score: the weights' hash, the runtime,
   the configuration and the git revision.
2. **Write proofs.** The model writes Lean 4 proofs, and Lean is the grader:
   a proof passes only when it checks in a clean environment with no `sorry`
   and no new axioms.
3. **Train and compare.** Fine-tuning runs on AWS in time-limited jobs, each
   with a written hypothesis, cost estimate and stopping condition, and every
   result is compared against the measured baseline.

### Repositories

| Repository | What it is |
|---|---|
| [`pi`](https://github.com/Pi-Mathematical-Reasoning/pi) | The project: the checkpoint probe, the answer grader, benchmark runs, the experiment log and the project brief |
| [`pi-benchmarks`](https://github.com/Pi-Mathematical-Reasoning/pi-benchmarks) | Manifests and tooling for Pi's benchmark data, mirrored in a private S3 bucket so the baseline can be re-run if an upstream disappears |
| [`pi-branding`](https://github.com/Pi-Mathematical-Reasoning/pi-branding) | The spiral eye logo, the avatar, the README banner, the social preview and the README header standard |
| [`.github`](https://github.com/Pi-Mathematical-Reasoning/.github) | This profile, and the organization's default community health files |

### Bots

| GitHub App | What it does |
|---|---|
| [`pi-release-bot`](https://github.com/apps/pi-release-bot) | Opens the Release Please pull requests, so CI runs on them |
| [`jaws-platform-comments`](https://github.com/apps/jaws-platform-comments) | Posts the Jaws plan and preview comments on pull requests, from [Jaws](https://github.com/Jaws-Platform) |

### How every repository works

One-line Conventional Commits, every change through a pull request, squash or
rebase merges only, and a release cut by merging the Release Please pull
request. Cloud infrastructure is created and managed by
[Jaws](https://github.com/Jaws-Platform/jaws). Everything is MIT licensed.
