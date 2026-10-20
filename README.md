# EdgeDefect

Planning only. No application, generator, trained model, benchmark result, or deployment is implemented here. The public repository exists; implementation has **not been activated**.

Repository: [123ssk321/cv-edgedefect](https://github.com/123ssk321/cv-edgedefect). This standalone proposal derives from *Quick-to-product computer-vision projects for a solo builder*, research dated 2026-10-16, specifically its EdgeDefect section. The execution plan corrects its dataset and noncommercial-use assumptions.

## Intended result

A local CPU tool to flag visible scratch-like and stain-like anomalies on the central face of one matte rectangular plate family in a fixed, near-frontal image. It would show a score, anomaly overlay, and one of four outcomes: no flag, review, uncertain, or retake. Every part disposition remains a human decision; a score is not a probability or a quality certificate.

The planned default is a deterministic NumPy/OpenCV template-residual baseline, with one bounded handcrafted patch-statistics comparison. Training and reproducible evaluation can finish using automatically generated images and exact masks. Existing human-annotated public datasets are permitted when their rights cover the intended use. No user collection, manual labeling, paid annotation, private images, GPU, camera purchase, or industrial hardware is required for the planned technical finish.

Excluded: general factory inspection, dimensional metrology, assembly verification, missing-fastener detection, physical dent measurement, PLC/camera integration, automatic rejection, safety-critical decisions, cloud accounts, and pretrained deep-model dependencies. Industrial metrology is a separate project about calibrated dimensions and assembly; EdgeDefect is anomaly triage.

## Schedule when selected

This is an alternative allocation of the portfolio's **16 hours/week total**, not a concurrent allocation per project. Activation is a future explicit selection, not repository creation or this planning task.

| Relative time | Planned evidence |
|---|---|
| Day one | Public planning repository; not a functional release |
| Months 1–3 | Scope, rights gate, small synthetic corpus, baseline and evaluation |
| Month 4 | First runnable public release: generator, local demo, benchmark, failure gallery and limitations |
| Months 5–6 | MVP and pilot-ready package, workflow rehearsal, operations and stop/go decision |
| By month 12 | Absolute deadline for a slipped functional result or explicit stop; no automatic extension funding |

Budget: **352 estimated work hours + 64 reserve hours = 416 hours**, equivalent to 26 weeks at 16 hours/week. The first functional public result must occur at least three and no more than twelve months after activation; month 4 is the target.

Synthetic robustness, licensed public-data performance, and actual customer validation will be reported separately. Without external use-case evidence, the deliverable can be a complete reproducible prototype and evaluation tool, but factory reliability, customer time savings, commercial readiness, and willingness to pay remain unproven. Optional feedback cannot hold the technical release hostage.

## Planning documents

- [Execution plan](docs/EXECUTION_PLAN.md): scope, module contracts, schemas, future commands, evidence gates, schedule and handoff policy.
- [Machine-actionable backlog](planning/backlog.json): 32 dependency-ordered work packages, acceptance criteria, estimates and later-phase slices.

All commands and additional paths in the plan describe future work. The parent supplies shared AGENTS.md instructions; this project does not create its own.
