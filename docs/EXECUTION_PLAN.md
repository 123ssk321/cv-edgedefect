# EdgeDefect execution plan

Planning revision: 2026-10-20. Status: public plan, implementation not activated. Repository: [123ssk321/cv-edgedefect](https://github.com/123ssk321/cv-edgedefect), creation verified by the parent according to the user's publication note. No remote action is part of this task.

Source context: *Quick-to-product computer-vision projects for a solo builder*, research dated 2026-10-16, EdgeDefect section and shared data/license guidance. This document stands alone and does not require access to that report. It specifies future work; none of the future modules, commands, tests, metrics or releases is claimed to exist.

Requested architect configuration: GPT-6 Astra, high effort. Implementation handoff target: Luna medium or GPT-5.6 medium; design escalations go to Astra. This records the requested roles, not an assertion that the runtime model was changed by this task.

## 1. Outcome, boundaries and corrections

Build a local, single-user visual triage workbench for one matte, approximately rectangular plate face against a contrasting plain background. The operator imports an image, inspects an overlay and score, and chooses the disposition. The tool may prioritize review; it must never approve a manufactured part for use or actuate a reject mechanism.

Freeze v1 to one synthetic family called matte_plate_v1: near-frontal, single plate, full outline visible, no holes or fasteners, canonical 256 by 256 image, central interior inspection ROI. Source images may be 256–2048 pixels per side, PNG or JPEG, RGB or grayscale, at most 10 MiB. Require explicit profile selection. In-envelope synthetic pose is translation up to 8 canonical pixels, rotation up to 5 degrees, scale 0.9–1.1; the rectification module maps the outline into the canonical square. These are design limits, not measured camera tolerances. Inspect only an interior region eroded 12 canonical pixels from the plate boundary; do not report physical units.

Detect only visible scratch-like lines and stain-like regions. Operational synthetic targets are scratches at least 2 pixels wide and 16 pixels long, and stains at least 64 pixels in area, with absolute luminance contrast at least 0.12 on a 0–1 scale. Report failures below these sizes/contrasts separately as unsupported probes. The generator's physical realism is intentionally limited to a 2D appearance model. It cannot demonstrate real metallurgy or lighting physics.

Outputs: input thumbnail, quality/pose reasons, numeric anomaly score, non-probabilistic nominal percentile, canonical heatmap and source-coordinate overlay, versioned decision and JSON/CSV export. Outcomes are no_flag, review, uncertain, retake. Even no_flag means only no anomaly flagged within this profile, with human inspection still required. Operator observations are audit events, not automatically created training labels.

Explicit exclusions: multiple part families, reflective/transparent parts, textures requiring semantic recognition, dents as 3D geometry, cracks below image resolution, missing components, seam/assembly checks, calibrated dimensional measurement, tolerances in millimetres, defect-type classification, root-cause diagnosis, certification, real-time line speeds, PLC/robot/camera drivers, mobile/NPU optimization, multi-user hosting, billing and enterprise integrations. No GPU or new camera is required. No clinical use is proposed.

Portfolio distinction: the industrial metrology alternative owns calibrated dimensions and assembly verification. EdgeDefect owns surface-anomaly triage only. The e-invoice alternative owns rules, exceptions and reconciliation beyond quick extraction; those document workflows have no scope here. Selecting this project consumes the portfolio allocation, rather than running alongside the other alternatives.

Corrections that override the source report for this project:

1. Existing human-labeled public data is allowed. The constraint prohibits creating a new manually collected/labeled/curated corpus for the user or an undisclosed annotator; it does not disqualify existing annotations.
2. Noncommercial terms do not automatically authorize commercial evaluation, method selection, or internal product R&D. Those uses need an explicit rights decision before download/use. A research-only directory is not permission.
3. Public-benchmark performance, synthetic robustness and customer validation are three different evidence classes. None substitutes for the others.
4. A partner and private images are optional for the technical finish. Commercial claims remain conditional if appropriate external evidence is absent.

## 2. Product discovery and evidence gates

Hypothesis, currently unverified: a quality technician or small integrator benefits from local prioritization and an auditable review queue for repeated still-image checks. The buyer might pay for configuration/support; no price or demand is validated. The comparator is ordinary manual review plus a simple quality gate, not a hypothetical perfect inspector.

ED-002 defines a two-page use-case brief: input source, inspection frequency, existing review steps, operator action for each result, relative missed-anomaly/review costs, acceptable retake rate, and a script comparing the current workflow with the proposed tool. Draft three candidate operator questions. Optional conversations use synthetic examples and do not request a dataset. If no participants are available, record unavailable evidence and run a scripted self-rehearsal labeled as such. Do not invent interviews or treat a self-rehearsal as independent validation.

ED-028 budgets up to three optional 30-minute feedback sessions plus preparation and analysis. Outreach/messages require the user's authorization at execution time; preparation and technical work continue without them. The hard timebox is the issue budget. ED-029 defines a customer evaluation protocol, or executes an evidence-gap assessment if no partner/data exists. Authorized, already-annotated public or existing customer data can be considered without asking anyone to create labels. If usable ground truth is absent, workflow feedback alone cannot establish detection accuracy.

| Gate | Required evidence and decision |
|---|---|
| M1 / month 1 | Scope, license matrix, contracts and risk memo. Proceed on synthetic path even if public-data rights are unknown. Stop a commercial claim if certification or automatic rejection is required. |
| M2 / month 2 | Deterministic 64-image smoke set; exact masks; leakage checks; two CPU baselines on development only. Resolve generator bugs before scaling. |
| M3 / month 3 | Frozen thresholds, meaningful metric denominators, protected final test and failure gallery. Select template baseline unless the one comparison earns its complexity. |
| M4 / month 4 | A fresh installation generates data, fits, evaluates and serves an interactive local demo; release contains results and limitations. Passing software checks with failed performance targets permits only a clearly labeled experimental prototype. |
| M5 / month 5 | Review persistence, bounded batches, clean local deployment and workflow evidence with its source class stated. |
| M6 / month 6 | MVP/pilot-ready checklist, operations rehearsal and go/hold/stop memo. No external evidence means technical completion with commercialization on hold. |

Technical release targets, all recommendations to test rather than promises: on the in-envelope final synthetic set, supported-anomaly recall for review or uncertain at least 0.90, nominal no_flag coverage at least 0.80, and quality-failure retake recall at least 0.90. Also require nominal review rate at most 0.10 and uncertain rate at most 0.10. Retakes must not improve anomaly recall by being counted as detections. Report confidence intervals even if they cross target values. A target failure triggers a bounded correction or the prototype fallback; never silently weaken targets or select a flattering subset.

Commercial go additionally requires a named repeated use case, independently observed operator benefit (provisional target: 20% median task-time reduction without increased missed anomalies), documented customer-specific error costs, authorized domain evidence and a support-cost estimate acceptable to the prospective buyer. Three feedback sessions cannot establish industrial reliability. Customer-specific sample sizes and confidence bounds must be justified before any reliability claim; do not convert synthetic thresholds into factory guarantees. Otherwise hold commercial rollout, publish the limitations and finish the technical project.

## 3. Data and licensing

Default complete path: self-authored procedural 2D matte surfaces, background gradients, polygon plate, scratches and stains. Use NumPy for seeded sampling and OpenCV for rasterization/transforms. No external textures, logos, fonts, pretrained weights or artist assets are needed. Generate all images, masks and labels in the same operation. Human visual QA may inspect automatically selected contact sheets to debug rendering; it must not become sample selection, relabeling, or manual curation. Fix the algorithm and regenerate the entire affected version.

Generate clean/defective pairs with the same nuisance parameters. Save a binary mechanism mask before imaging corruption, warp it with the exact image transform using nearest-neighbor resampling, intersect with the visible plate/inspection region, and save a separate visibility flag. The mask is exact for the injected mechanism, not a claim of photorealistic physical defect boundaries after blur. Derive image anomaly truth from nonempty visible supported-defect masks. Quality truth comes from renderer conditions, independently of defect truth; out-of-envelope or obscured defects retain their mechanism metadata but are outside the supported accuracy denominator. Never score segmentation on invalid/fully hidden masks.

Stage sizes are caps, not instructions to download big datasets:

- Smoke: 64 generated images, fixed seeds, enough examples of each defect/quality outcome; not used for accuracy claims.
- Development: 800 images = 400 nominal train + 200 development (100 nominal, 100 defective) + 200 nominal calibration. A distinct diagnostic corruption suite is derived from development groups only. Train uses nominal samples only.
- Final: 1,200 newly generated images = 300 nominal + 300 scratches + 300 stains, all in-envelope, plus 300 quality/out-of-envelope challenge images. These 1,200 plus 800 development images give a 2,000-image cap for the main release; the 64 smoke fixtures are separate. Stress transforms are generated on demand, at most 600 additional images from protected final groups, counted as correlated variants. Split metrics use unique base groups, not inflated variant counts.

Split by base_surface_id and recipe family before transformations; seeds alone are insufficient. Keep clean/defective pairs and all nuisance variants together. Hash images, manifests and recipes; enforce disjoint groups across train/dev/calibration/test. Assign procedural texture parameter bands and stain/scratch shape families reserved for challenge testing. A second procedural shape recipe is a robustness probe, not an independent real-data distribution. ED-009 freezes a test recipe hash before model selection; ED-016 generates/unseals final data after ED-014/015 record model and policy hashes. If final results motivate changes, record contamination and use reserve for a new test version; retain all old results.

Public-data path is optional and disabled by default. MVTec AD has existing normal training images, anomalous test images and pixel annotations, but the publisher excludes commercial purposes [S1]. For this product-oriented plan, do not use it until written permission or another explicit documented rights basis covers the exact activity, including evaluation/R&D. No permission request, fee or response is assumed. ED-018 spends its time auditing rights and either records a skip or runs an authorized adapter. A skip is a completed gate decision, not a successful real-data experiment. Any approved MVTec path uses one texture category (tile) for an out-of-domain method check, not a second product family; initially read at most 16 nominal train and 32 existing test images in a deterministic sorted order. Preserve official train/test separation, reserve nominal training images for calibration, and do not adjust synthetic production thresholds from this probe. Escalate before any larger experiment; no bulk download without a subset acquisition method fitting this budget.

| Separate rights layer | Default / required evidence |
|---|---|
| Project source code and model algorithm | Propose MIT for original code at activation; record owner/license decision and notices in ED-003. This planning document does not create a LICENSE grant. |
| Runtime and packages | CPython, NumPy and opencv-python-headless; freeze exact versions/artifacts in ED-006. Upstream NumPy uses BSD terms [S4], OpenCV current source Apache-2.0 [S3]; wheel binaries and bundled libraries require their own notices/SBOM inspection. Python/toolchain and test dependencies also get entries. |
| Model parameters/weights | No pretrained weights. Locally fit template/variance arrays derive only from the approved synthetic manifest. Record parameters' provenance and intended redistribution terms separately. New checkpoints require a new rights/design gate. |
| Images/annotations/recipes | Original generated assets: proposed CC0 data/metadata release after ownership audit; original generator code follows code license. External data/annotations need individual terms and provenance even if already human-labeled. |
| Evaluation artifacts | Permission to use data does not automatically imply permission to publish thumbnails/masks. Default gallery is synthetic. Aggregate results require their own publication check. |

Maintain licenses/assets.json with entries containing id, kind (code/runtime/weights/data/annotations), version, source_url, sha256, license_id, license_text_path, attribution, rights_holder, intended_use, permissions (train/evaluate/commercial_rnd/redistribute), evidence_path, checked_at, decision (allowed/denied/unknown), and reviewer. Unknown or denied required permission fails closed before reading the asset. Own synthetic entries link generator commit/hash and the ownership/license decision. Do not infer rights for weights from an inference runtime license. No ONNX/OpenVINO/torch dependency is planned; a switch is a design escalation, not a routine implementation choice.

## 4. Concrete architecture and interfaces

Use Python 3.12, NumPy and opencv-python-headless as the default CPU stack, Python stdlib argparse for CLI, sqlite3 for local review persistence and http.server with an explicit route handler for a loopback-only UI. Use plain HTML/CSS/JavaScript with no build toolchain/CDN. Versions are pinned only after activation-time compatibility and artifact-license checks; do not invent verified current versions. The stdlib service is a local demo/workbench, not an internet production server. The only model alternative is the handcrafted patch engine below; no framework search.

All paths below are relative to the eventual standalone repository root. In this workspace that root is projects/edgedefect. Do not edit siblings or create a project AGENTS.md.

| Module / future file | Contract and invariant |
|---|---|
| contracts.py + schemas/*.schema.json | Validate JSON on every boundary; immutable records with schema_version=1; unknown required schema versions rejected. Paths are repository/run relative and cannot escape configured roots. |
| rights.py | assert_allowed(asset_ids, purpose, redistribute=False) -> RightsReceipt; no network I/O or implicit downloads; unknown rights raise RightsError before ingest. |
| synthetic.py | generate(config: GeneratorConfig, output: Path) -> DatasetManifest; deterministic seed/group allocation, aligned masks, no external asset fetches. |
| ingest.py | decode(bytes, profile) -> Frame; reject unsupported/malformed/oversize images, decode once, apply EXIF orientation consistently or reject unsupported orientation, strip metadata from derived output. |
| quality.py | assess(frame, profile) -> QualityResult with checks, metrics and reason_codes; segmentation/corner validity, blur, clipping and dynamic range. Invalid geometry never reaches scoring. |
| geometry.py | rectify(frame, profile) -> AlignedFrame(image: float32[256,256], roi: bool[256,256], source_to_canonical: float64[3,3]); mask transform uses nearest-neighbor; reject singular matrix/invalid quadrilateral. |
| engines/base.py | fit(nominal_frames, config) -> ModelBundle; score(aligned, bundle) -> ScoreMap(score: float, map: float32[256,256]); fit never sees dev defects, calibration or test images. |
| engines/template.py | Default robust template residual algorithm below. Save arrays in .npz with allow_pickle=False and manifest JSON. |
| engines/patch.py | Sole bounded competitor, handcrafted 16x16 patches at stride 8 with local mean/std/gradient-magnitude mean. Fit per-location diagonal statistics with variance floor; bilinear score-map interpolation. |
| policy.py | decide(quality, score, policy) -> Decision; pure function, no UI or database dependencies; fixed thresholds and explicit precedence. |
| evaluate.py | evaluate(dataset, bundle, policy, suite) -> Report; reject train/test overlap, capture data/model/policy hashes and denominators. Never fits or alters thresholds. |
| pipeline.py | inspect(image_bytes, profile, bundle, policy) -> InspectionResult; orchestrates quality, geometry, scoring and policy; score=None on retake. |
| store.py | append_inspection(result); append_review(inspection_id, revision, disposition, note); list_queue(filters); export_csv(); SQLite transactions, idempotent keys, immutable inspection results. |
| service.py + ui/ | GET /health, GET /profiles, POST /inspect, GET /inspections, POST /reviews, GET /exports; browser and CLI call the same pipeline. |
| cli.py | Commands below; exit 0 success, 2 invalid input, 3 rights gate, 4 evaluation gate, 5 operational failure. A valid review/retake is a successful inspection, not a CLI error. |

Default engine math: transform grayscale to [0,1], normalize using ROI median and robust spread with floor 0.05; fit per-pixel median T and median absolute deviation D on nominal train images. At inference compute R=abs(I-T)/max(1.4826*D,0.02), apply a fixed 3x3 median filter inside the ROI, and use the mean of the largest ceil(0.01*N_roi) residuals as image score. Zero outside ROI for display only; exclude those pixels from scoring/metrics. Save normalization definition and floors in model.json. The overlay is explanatory residual evidence, not a defect probability or a learned segmentation certainty.

Patch competitor: standardize each 3-component patch descriptor using nominal per-location means and diagonal variances floored at 1e-4; compute square root of the sum of normalized squared differences; overlap-average patch scores and use the same top-1% image aggregation. It is not PatchCore and uses no pretrained feature network. Limit comparison to template and patch, one predeclared default per engine plus at most one dev-set adjustment per engine. Select patch only if dev anomaly recall improves by at least 0.05 at nominal review rate <=0.10, uncertain <=0.10, and measured p95 CPU latency <=1 second. Otherwise keep template; report both. All numbers are hypotheses until measured.

Policy calibration: using nominal calibration scores only, set t_low to empirical 90th percentile and t_high to empirical 99th percentile, with a documented nearest-rank rule. quality fail -> retake; valid with score >=t_high -> review; t_low<=score<t_high -> uncertain; score<t_low -> no_flag. If both thresholds coincide, collapse the uncertain interval and document this; no random epsilon. A nominal percentile is descriptive, not P(defect). Profile mismatch, invalid alignment, absent bundle or incompatible hashes produce an explicit failure/retake, never no_flag. Unknown domains are not assumed detectable by image-quality heuristics; the scope warning persists even when checks pass.

Initial quality thresholds to test on development fixtures: blur variance <30 on canonical uint8 grayscale, more than 5% ROI pixels clipped to <=2 or >=253, ROI intensity p95-p5 <0.10, plate region missing/multiple, projected scale/rotation outside profile, or invalid corner geometry. Freeze actual selected thresholds with development evidence; no tuning on final data. Retake reason precedence is decode/geometry, profile envelope, exposure, then blur. Decode errors use an input error response rather than creating a scored inspection.

Service contracts: POST /inspect accepts multipart one image plus profile_id and idempotency_key; returns 201 InspectionResult, 200 for the same key/hash retry, 409 for conflicting key, 400 invalid format, 413 size limit, 422 profile/contract mismatch. POST /reviews accepts inspection_id, expected_revision, disposition (confirmed_flag/dismissed/needs_recapture/undecided), optional note <=500 characters; returns 201 or 409 stale revision. No arbitrary server filesystem paths in HTTP input. GET /inspections uses cursor pagination <=100; GET /exports returns CSV with formula-leading strings escaped. Bind 127.0.0.1; require per-launch random token, same-origin checks and strict Host allowlist; no wildcard CORS. Limit concurrent inspections to one and return 429 when the bounded queue of eight fills. No outbound network traffic during inference.

## 5. Versioned schema contract

ED-005 implements JSON Schema and matching Python validation for these records. Required fields are listed below; optional fields must have explicit null semantics. Arrays and numeric fields are finite, paths normalized, hashes lowercase SHA-256, enums closed. Keep file formats boring; no pickle loading.

| Record / schema path | Required fields and constraints |
|---|---|
| DatasetManifest / schemas/dataset.schema.json | schema_version, dataset_id, generator_version, config_hash, license_asset_ids[], samples[]; each sample: sample_id, base_surface_id, recipe_family, seed, split(train/dev/calibration/test), image_path, image_sha256, mask_path, mask_sha256, defect_type(none/scratch/stain), has_visible_defect, supported_defect, quality_label(valid/retake), nuisance{}, coordinate_frame(source), width, height. An empty nominal mask is required, not a missing file. |
| Profile / schemas/profile.schema.json | schema_version, profile_id, family, canonical_size=[256,256], roi_erosion_px=12, pose_bounds{}, quality_thresholds{}, max_bytes, max_pixels, version_hash. No physical units/tolerances. |
| ModelBundle / schemas/model.schema.json | schema_version, model_id, engine(template/patch), params_path, params_sha256, fit_manifest_hash, train_group_hash, profile_hash, preprocessing{}, license_asset_ids[], code_revision. Reject wrong profile/model combinations. |
| Policy / schemas/policy.schema.json | schema_version, policy_id, model_hash, calibration_manifest_hash, calibration_group_hash, quantile_method, t_low, t_high, quality_config_hash, supported_scope, version_hash; finite t_low<=t_high. |
| InspectionResult / schemas/inspection.schema.json | schema_version, inspection_id, image_sha256, profile_hash, model_hash, policy_hash, created_at_utc, quality{valid,metrics,reason_codes}, score(number or null), nominal_percentile(number or null), decision, reasons[], heatmap_path(string or null), overlay_path(string or null), source_to_canonical(matrix or null), latency_ms. Null scoring and absent maps on quality failure; immutable after creation. |
| ReviewEvent / schemas/review.schema.json | schema_version, event_id, inspection_id, expected_revision, new_revision, disposition, note, created_at_utc; no claim that a review event is a ground-truth training annotation. |
| EvalReport / schemas/evaluation.schema.json | schema_version, run_id, evidence_class(synthetic/public_dataset/customer), dataset_hash, model_hash, policy_hash, environment{}, group_counts{}, metrics{}, intervals{}, failures[], limitations[], test_exposure_log; undefined metrics are null with a reason, never zero. |

## 6. Future commands and deployable demo

These commands are specifications for later implementation, not commands to run now. Use the same command syntax in PowerShell and POSIX shells; repository-local relative paths below are intentional. ED-006 produces locked requirements and a wheelhouse recipe. Installation may need network; scoring and the sample demo must work offline after installation.

~~~text
python -m venv .venv
# Activate the environment using the platform-specific activation command documented in ED-022.
python -m pip install --require-hashes -r requirements.lock
python -m pip install --no-deps --no-build-isolation -e .
python -m edgedefect rights check --manifest licenses/assets.json --purpose commercial_rnd
python -m edgedefect generate --config configs/smoke.json --out runs/smoke
python -m edgedefect validate-data --manifest runs/smoke/manifest.json
python -m edgedefect generate --config configs/development.json --out runs/dev
python -m edgedefect fit --manifest runs/dev/manifest.json --split train --engine template --profile configs/matte_plate_v1.json --out runs/template
python -m edgedefect compare --manifest runs/dev/manifest.json --split dev --engines template,patch --out runs/comparison
python -m edgedefect calibrate --model runs/template/model.json --manifest runs/dev/manifest.json --split calibration --out runs/policy.json
python -m edgedefect generate --config configs/final.json --out runs/final --require-freeze reports/freeze.json
python -m edgedefect evaluate --manifest runs/final/manifest.json --model runs/template/model.json --policy runs/policy.json --out reports/final
python -m edgedefect inspect --image examples/synthetic_stain.png --model runs/template/model.json --policy runs/policy.json --out runs/inspection
python -m edgedefect serve --host 127.0.0.1 --port 8765 --model runs/template/model.json --policy runs/policy.json
python -m edgedefect batch --input examples/batch --limit 100 --out runs/batch
python -m edgedefect export --db runs/reviews.sqlite --format csv --out runs/reviews.csv
python -m edgedefect purge --run runs/inspection --dry-run
python -m pytest -q
~~~

If patch wins, ED-014 records the selected bundle and substitutes its path in the release script; no undisclosed refit. batch/export/purge are months 5–6 additions and must not appear as supported v0.1 commands. The month-4 deliverable is a downloadable local package with an interactive browser UI served locally, synthetic examples, generated report and short walkthrough. No hosted inference, paid service, domain purchase or website builder is needed. Publication is future authorized execution; this task makes no release, commit, push or repository mutation.

Reference hardware assumption: existing x86-64 4-core CPU, 8 GiB RAM, 2 GiB free disk, no GPU. Record actual CPU/OS/thread settings, warmup=10 and measured=100 single-image runs. Target p95 <=1 second at canonical 256 pixels and peak RSS <=1 GiB; cold startup target <=10 seconds. If unmet, retain the template baseline and cap batches to 20. Benchmark at 128 pixels only as a distinct downgraded profile with correspondingly restated supported-defect sizes; never claim unchanged accuracy. GPU/industrial-PC procurement is not a blocking dependency. This offline, file-input prototype is the scoped hardware fallback; real capture integration requires a later separately funded scope.

## 7. Evaluation and review evidence

Compare three outputs: review-all baseline, quality-only/manual workflow comparator, and template/patch detectors. Review-all achieves full referral at zero nominal no_flag coverage and therefore cannot pass the coverage gate. Always-no-flag is a negative control expected to miss all anomalies. These controls catch misleading aggregate accuracy.

Image-level metrics: anomaly referral recall (review or uncertain), review-only recall, dangerous no_flag rate among supported anomalies, nominal no_flag coverage, nominal review/uncertain/retake rates, PR-AUC and ROC-AUC on valid scored images. Report exact numerators/denominators and class prevalence. Separately report supported anomalies diverted to retake; publish an all-input outcome confusion table so abstentions cannot conceal misses. PR-AUC on balanced synthetic data does not predict deployment precision at rare-defect prevalence. Show precision under hypothetical prevalences 0.1%, 1%, 5% as a labeled calculation from measured sensitivity/FPR, with uncertainty and distribution-shift caveats.

Pixel-level metrics: pixel average precision on visible anomaly masks inside valid ROI, and IoU at a development-frozen pixel threshold; report empty-mask cases separately. Do not claim physical segmentation accuracy from procedural masks. Use tie-aware metric implementations checked against hand-computable fixtures; no new ML framework just for metrics. Image proportions receive 95% Wilson intervals; correlated robustness variants receive a base-group bootstrap with fixed resampling seed, 1,000 draws. Disclose small-sample and correlated-family limitations. Undefined AUROC (one class) is null with explanation.

Robustness: predeclare nominal variation and outside-envelope suites for blur, exposure, scale, rotation, stain contrast and scratch width. Change one factor at a time plus one combined challenge; use group-level paired comparisons. Show worst-bucket recall, coverage and retake rates, along with sample counts. Prevent mask leakage: neither engine nor quality gate may access generator defect_type, masks, seed, filenames or labels during inference. Filenames are opaque sample IDs. Negative controls include shuffled labels, empty masks and intentional group overlap, which must be detected or degrade metrics as expected.

Evidence separation: reports/final is synthetic; reports/public-data is a licensed public dataset result or explicit skip memo; reports/customer is customer evidence only when actually obtained. Synthetic-vs-public results are not paired measurements of transfer and must not be pooled. Failure gallery selection is automatic and deterministic: top false negatives, top false positives and seeded random failures; preserve counts, no hand-picked success showcase. Default gallery contains synthetic material only.

Each completed issue supplies docs/evidence/ED-NNN.md with input hashes, exact commands, expected/actual outcome, relevant test/report paths, limitations, actual hours and a review note. Future release records carry commit hashes when a repository checkout exists. Software tests validate behavior; metric gates validate usefulness; independent customer observation validates product fit. Passing one does not imply the others.

## 8. Hours and relative schedule

Activation is explicit selection of this alternative with its capacity allocation. Public repository creation and this planning work do not start the clock. Use 26 working weeks for a six-month capacity envelope; calendar-month gates are measured from the activation date recorded later in docs/activation.md. First functional public release is targeted in month 4, at least 3 months and no later than 12 months after activation. Internal prototypes may precede that; do not describe public planning as functional delivery.

| Milestone | Relative slot | Issues | Work hours | Reserve envelope | Total |
|---|---|---|---:|---:|---:|
| M1 Scope, rights, contracts | Month 1 / weeks 1–4 | ED-001–006 | 48 | 8 | 56 |
| M2 Data and CPU baselines | Month 2 / weeks 5–8 | ED-007–013 | 80 | 8 | 88 |
| M3 Frozen evaluation | Month 3 / weeks 9–13 | ED-014–019 | 60 | 8 | 68 |
| M4 Functional public v0.1 | Month 4 / weeks 14–17 | ED-020–024 | 52 | 8 | 60 |
| M5 Review workflow and deployment | Month 5 / weeks 18–22 | ED-025–028 | 60 | 16 | 76 |
| M6 Pilot readiness and decision | Month 6 / weeks 23–26 | ED-029–032 | 52 | 16 | 68 |
| Total | 26 weeks | 32 packages | 352 | 64 | 416 |

The milestone slots are target completion windows, not independent monthly capacity grants. M2 needs 88 hours, exceeding its four-week slot by 24: begin ready M2 work with M1's 8 spare hours, use M3's 12 spare hours, and use M4's 4 spare hours for the remaining spill. M1–M3 total 212 hours against 208 hours in weeks 1–13, so the M3 gate may finish four hours into week 14. ED-020 starts after ED-019; ED-021 also waits for ED-017 and ED-018. Do not start those dependent tasks early to conceal the spill. M4's own 60 hours then fit the remaining 60 hours through week 17. M6's 68 hours use 4 hours from M5's spare capacity by starting ready M6 work late in week 22: ED-030 can start after ED-027, independently of ED-028/029. Cumulative release workload is 240+32=272 hours (17 weeks); remaining workload is 112+32=144 hours (9 weeks). No weekly assignment exceeds 16 hours. Exact calendar dates are derived on activation, not guessed here.

Alternative workstream view of the same 352 hours (not extra allocations): discovery/feedback/pilot/decision ED-002/028/029/032 = 54; dedicated learning ED-004 = 8; data/provenance/rights ED-003/007/008/009/018 = 52; models/quality/policy ED-010–015 = 64; dedicated evaluation/testing ED-016/017/021 = 34; interfaces/CLI/UI/persistence/batch ED-005/019/020/025/026 = 58; activation/build/deployment/security/operations ED-001/006/022/027/030/031 = 64; standalone cards/release ED-023/024 = 18. Tests, evidence writing and review embedded in other packages are already included in their estimates; no hidden documentation/testing hours.

Reserve is 64 hours, unassigned to issues and never counted twice: 16 dependency/learning surprises, 16 data/evaluation corrections, 16 packaging/security/hardware variance, 16 product feedback/release remediation. Timing envelopes above total the same reserve, not an additional pool. Track actual usage and remaining reserve in docs/budget.md. No routine work is charged to reserve to disguise underestimation. More than 12 hours for one unplanned repair or forecast total >416 triggers Astra scope review. Remove a bounded alternative before expanding scope.

Months 7–12 are a calendar recovery window only, with no additional planned hours: complete unfinished packages using the remaining original budget, or stop. At month 6 reforecast if slipped; at month 9, if no functional release, switch to the runnable template-only synthetic prototype and publish the failure analysis. By month 12 either the functional result exists or record a failed/stopped project honestly; an empty repository or report alone cannot satisfy the functional-result requirement. No ongoing support contract or open-ended R&D is included after M6.

## 9. Later phases, operations and stop/go

ED-025 adds transactional review queue and reversible operator decisions; ED-026 adds bounded batch input/export; ED-027 validates clean Windows and Linux installation, offline use, profile compatibility, resource caps and rollback. ED-028 rehearses usability and captures optional feedback. ED-029 prepares and, only if evidence is available, performs a bounded use-case validation; the no-partner branch produces an evidence-gap memo and synthetic rehearsal, not a claimed pilot. ED-030 provides logs, backup/restore, retention and rollback procedures. ED-031 audits local service/input security and package notices. ED-032 assembles v0.2, final evidence matrix, cost/support estimate and a go/hold/stop decision. All are estimated in the backlog.

Default retention: images/overlays only for the active run; persistent image storage opt-in, capped at 200 MiB or 7 days, whichever first. Keep bounded inspection metadata and review events for 30 days; logs contain IDs, hashes, timings and reason codes, no raw bytes or free-text notes. Show retention settings and provide scoped purge with dry-run plus resolved-run-path checks. No telemetry or silent uploading. Versioned bundles and a manifest let users roll back without rewriting past inspection results. DB backups are local, restored in a disposable copy for testing. Monitoring is local aggregate latency/error/retake/score histograms; synthetic drift probes verify alert behavior but do not establish real process drift detection. Disable scoring on missing/corrupt model, rights failure or schema mismatch.

Stop immediately for scope requiring safety certification, automatic line control or unavailable visual evidence. Hold commercialization if the only evidence is synthetic, rights are unresolved, operator benefit is unproven or support costs cannot be bounded. Technical go is allowed for a candid experimental release if its functional integrity is established and metric shortcomings are visible. Do not spend contingency on endless model replacement; after two bounded baseline attempts, publish the prototype/failure study and close or seek a separately authorized redesign.

## 10. Conservative-model handoff and done/blocked policy

The parent provides shared AGENTS.md; obey it without creating a competing file. Use planning/backlog.json as the work order. Select the lowest ready ED ID with completed dependencies; do not implement future phases opportunistically. Read this plan, relevant schemas, dependency evidence and existing files. An epic >12 hours must be executed as its listed 2–8-hour child slices; these slices are internal checkpoints, not separately estimated issues. Each epic's estimate already includes every child slice. If later split into tracked child issues, replace/reduce the parent estimate so totals remain unchanged.

Luna medium or GPT-5.6 medium can implement the specified interfaces, algorithms, tests and routine compatible fixes. Before editing, record a compact handoff in docs/handoffs/ED-NNN.md: task/goal, dependency receipts, permitted paths, chosen slice, commands, expected evidence and remaining hours. At completion append changed paths, command results, input/model hashes, residual limitations, actual/remaining hours and the next ready issue. Do not claim a test passed without running it. No needless confirmation for already authorized implementation choices; this planning task itself authorizes no implementation or remote writes.

Done means all listed outputs exist, contract/acceptance checks pass, data/method pitfalls are addressed, evidence receipt is reviewable, scope/docs match actual behavior, and dependencies are satisfied. A public-data skip may be done only where the issue explicitly defines a skip output. A performance gate failure must remain visible; it cannot be marked as a successful target. Publication, outreach and releases are future actions under the executing user's applicable authorization.

Blocked means a concrete required input/right/decision or reproducible defect prevents that package's acceptance. Record reason, failing command, evidence, two bounded safe attempts (maximum 2 hours within the task estimate), and exact unblock condition. Work on independent ready packages. Missing optional customer feedback or denied optional public-data access invokes its documented fallback and is not a project blocker. Never fabricate evidence, download unknown-rights assets, request manual labeling or silently expand the task.

Escalate to Astra for altered scope/family, schema incompatibility, new engine/framework/checkpoint, revised statistical targets, license interpretation that would admit denied/unknown material, hardware integration, repeated test contamination, >12-hour unplanned repair or total-budget overrun. Supply the failing evidence, two bounded options, recommended choice and hours impact. Routine naming, styling and bug fixes within these contracts need no escalation. Astra is a design role, not legal permission; unresolved rights remain excluded. If escalation is unavailable, continue the documented synthetic/template fallback and independent tasks, leaving the decision explicit.

## 11. Checked sources and assumption register

Sources checked read-only on 2026-10-19 during preparation; plan finalized 2026-10-20. Recheck exact versions/terms at activation. These references support narrow factual claims, not future performance or revenue.

- [S1: MVTec AD official dataset page](https://www.mvtec.com/research-teaching/datasets/mvtec-ad): existing industrial images/annotations and CC BY-NC-SA 4.0 restriction. The publisher states commercial purposes are excluded. Dataset use remains gated here.
- [S2: CC BY-NC-SA 4.0 legal code](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode.en) and [deed](https://creativecommons.org/licenses/by-nc-sa/4.0/): noncommercial purpose is a use constraint; attribution/share-alike and other rights also matter. Calling work R&D does not itself establish permission.
- [S3: OpenCV upstream license](https://github.com/opencv/opencv/blob/4.x/LICENSE): current source Apache-2.0. This is not a blanket statement about all distributed wheel components.
- [S4: NumPy upstream license](https://github.com/numpy/numpy/blob/main/LICENSE.txt): BSD-style source terms with notice conditions. Recheck the selected binary distribution and bundled libraries.

Unverified assumptions: CPU availability/performance; Python/package compatibility on the eventual machine; adequacy of 2D appearance variation; transfer to photographed parts; buyer need, price and support burden; external participants; public-data permissions; efficacy of default quality thresholds. Each maps to ED-004/006/017/022/027 (hardware/runtime), ED-007–017 (data/method), ED-002/028/029/032 (product) or ED-003/018/031 (rights). None is required to become true to release a candid working synthetic prototype. Real industrial validity remains a separate evidence obligation.
