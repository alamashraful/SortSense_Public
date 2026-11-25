# SortSense — Progress

**Last updated:** 2026-11-25

> The plan lives in `docs/SPRINT_BACKLOG.md` (what each task means, acceptance criteria). **This file is the status.**
> Edit the *Actual h*, *Status* and *Notes* cells, then run `python tools/progress.py` (or the VS Code task **Progress: update snapshot**) to refresh the snapshot below.
> Keep notes free of the `|` character.

**Status values:** `⬜ TODO` · `🔵 DOING` · `✅ DONE` · `⛔ BLOCKED` · `✂️ CUT`  (CUT tasks are excluded from the totals)

## Snapshot

<!-- SNAPSHOT:START -->
| Sprint | Done / total | Planned h | Actual h | Status |
|---|---|---|---|---|
| 0 — Setup | 4 / 4 | 4 | 4 | ✅ Done |
| Sprint 1 | 15 / 15 | 42 | 31.5 | ✅ Done |
| Sprint 2 | 8 / 9 | 27 | 21 | 🔵 In progress |
| Sprint 3 | 12 / 12 | 32 | 18.75 | ✅ Done |
| Sprint 4 | 4 / 7 | 33 | 11 | 🔵 In progress |
| Sprint 5 | 0 / 8 | 27 | 0 | ⬜ Not started |
| Sprint 6 | 0 / 9 | 31 | 0 | ⬜ Not started |
| Should items | 0 / 7 | 31 | 0 | ⬜ Not started |
| **Must total** | **43 / 64** | **196** | **86.25** | |

Budget **144–180 h**. Must-only plan **196 h**; spent **86.25 h** (47% of the upper budget).
<!-- SNAPSHOT:END -->

## Now / Next / Blocked

- **Now:** S4-02...S4-03 2 model evaluation on CPU testing.
- **Next:** S5 starting.
- **Blocked:** nothing.

## Decision gates (must be cleared before the listed tasks)

| Gate | What to decide | Blocks | Status |
|---|---|---|---|
| G1 | Confirm or change the **Proposed** decisions D-05…D-12 and D-24…D-26 in `ARCH §15` | S1-04, S1-06, S1-09…S1-12 | ✅ cleared 2026-10-14 — confirmed as proposed |
| G2 | Close the effort gap (≈ 200 h planned vs 144–180 h budget) | Sprint planning from S2 on | 🔵 owner deadline 31 Dec (maybe 7 Jan): needs about 17 h/week for the full scope. **Owner decided 2026-11-19: no cuts, aims for about 20 h/week.** Checkpoints at 2 Dec (below 70 h) and 16 Dec (below 100 h) in `SPRINT_BACKLOG.md` |
| G3 | Python version on the laptop is 3.11 or 3.12 | S0-03 | ✅ cleared — 3.12.10 |
| G4 | Cloud provider and domain | S4-06 | ⬜ open: university cloud details to come from the owner; needed by about 1 Dec |
| G5 | LLM provider | S6-02 | ⬜ open (not needed yet) |
| G6 | Base-classifier data gap: TACO has 8 `organic` and 45 `carton` boxes, TrashNet none. Add own photos in `data/raw/extra/<class>/`, add another dataset, or accept weak classes and let review corrections teach them (also confirm the proposed label map) | S1-11 | Embedder + base classifier head | 3 | 2 | ✅ DONE | Base head v1 on the laptop: global eval accuracy 0.710, macro-F1 0.582 (1074 crops), ECE 0.047. At threshold 0.70: 55% of items auto at 84% accuracy; the rest 55% accurate. Weak: organic (7 train crops), carton (37), glass recall 0.44 |

## Tasks

### Sprint 0 — Environment setup

| ID | Task | Est h | Actual h | Status | Notes |
|---|---|---|---|---|---|
| S0-01 | Install toolchain (Git, WSL2, Docker, Node, VS Code extensions) | 1.5 | 2 | ✅ DONE | All tools installed. Python 3.12.10, Node 24.20, Docker 29.8. `docker run hello-world` not shown in the log |
| S0-02 | Create repo, merge scaffold, first commit, GitHub remote | 0.75 | 0.5 | ✅ DONE | Repo at C:\workspace\CSDP\sortsense; private GitHub repo pushed |
| S0-03 | Run the scaffold: tests, API, frontend, database | 0.75 | 1 | ✅ DONE | Backend, database, frontend and full `docker compose up --build` all verified on the laptop |
| S0-04 | ML environment: CUDA PyTorch + Jupyter in VS Code | 1 | 0.5 | ✅ DONE | `check_env.py` all OK: torch 2.11.0+cu128, RTX 5050 GPU matmul works. A Windows Application Control block on SQLAlchemy appeared in between and went away after reinstalling |

### Sprint 1 — Foundation, auth, ML pipeline v0, Live View v1

| ID | Task | Est h | Actual h | Status | Notes |
|---|---|---|---|---|---|
| S1-01 | Repo, tooling, conventions | 1.5 | 0.5 | ✅ DONE | Lint and type checks clean; .env.example present; data/ ignored |
| S1-02 | Compose skeleton (db, api, web) | 2.5 | 2 | ✅ DONE | `docker compose up --build` verified on the laptop (dev servers stopped first) |
| S1-03 | FastAPI scaffold (logging, errors, health) | 1.5 | 0.5 | ✅ DONE | 8 tests pass: request ids, error envelope, health and version |
| S1-04 | SQLAlchemy models + Alembic baseline | 3 | 1.5 | ✅ DONE | Verified on the laptop: migration, 10 tables, 17 tests pass in about 2 s (after `localhost` → `127.0.0.1`) |
| S1-05 | Auth + RBAC | 3.5 | 2.5 | ✅ DONE | Login, logout, me, dual delivery (cookie + bearer), 429 login throttle, audit-logged, route-table test passes |
| S1-06 | Seed command | 1.5 | 1.5 | ✅ DONE | Idempotent seed runner, 20 users, 9 classes, 7 streams, facility settings, model v1 |
| S1-07 | Audit service + restricted DB role | 2.5 | 2 | ✅ DONE | Delivered 2026-10-14: migration 0002 (restricted role), AuditService, 14 tests. Verify on the laptop: `alembic upgrade head`, `python tools/db_ping.py` (app role OK), `pytest` (expect 31 passed) |
| S1-08 | Next.js: login, auth guard, role nav, API client | 3.5 | 2.5 | ✅ DONE | Login page, guard, role nav from one table (`src/lib/nav.ts`), typed client; placeholder pages for the other routes. Proxy default changed to 127.0.0.1 |
| S1-09 | Dataset preparation (TACO, TrashNet) | 3.5 | 3 | ✅ DONE | Ran on the laptop 2026-10-31 with 640 px TACO: 1500 detector images (1050/225/224), 5495 crops. 1781 boxes skipped as under 24 px. Only 4 organic and 30 carton training crops (G6: own photos later). |
| S1-10 | Detector interface + YOLOv8n fine-tune (SP-01) | 4 | 3 | ✅ DONE | Code done; 50-epoch run: TACO test mAP@0.5 0.434; 14 unseen photos 27/29 items boxed (provisional go). Further fine-tuning (full-size TACO, 100 epochs, unseen-video table) moved to Should item S-07 |
| S1-11 | Embedder + base classifier head | 3 | 2 | ✅ DONE | Base head v1 on the laptop: global eval accuracy 0.710, macro-F1 0.582 (1074 crops), ECE 0.047. At threshold 0.70: 55% of items auto at 84% accuracy; the rest 55% accurate. Weak: organic (7 train crops), carton (37), glass recall 0.44 |
| S1-12 | PipelineService | 4 | 3 | ✅ DONE | Real run on Sample_Video_01: 44 frames, 79 detections (49 AUTO, 30 PENDING_REVIEW), 148 ms per analysed frame incl. database |
| S1-13 | Videos + sessions API + WebSocket | 3 | 2.5 | ✅ DONE | Code and tests done (upload 415/422, sessions 201 + background run, WebSocket events, detections list/crop with 410, models load in the background). Owner check comes with the S1-14 UI |
| S1-14 | Live Sorting View v1 | 3.5 | 3.5 | ✅ DONE | Owner check passed: upload, start, cards with confidence bands, reload restores. Observation: food waste shows as Residual (no organic training data yet, G6) |
| S1-15 | Smoke test + README | 1.5 | 1.5 | ✅ DONE | Owner: `docker compose up --build` clean, `/` redirects to login, `smoke_test.py` passed through the web proxy with a real video |

### Sprint 2 — Review Queue, baseline mapping, overlay, multi-video

| ID | Task | Est h | Actual h | Status | Notes |
|---|---|---|---|---|---|
| S2-01 | Review API: queue, confirm/correct/skip, audit rows, corrections table | 3 | 3 | ✅ DONE | Owner tested: queue, confirm, correct, skip, audit, concurrency. Sidebar fix confirmed |
| S2-02 | Review Queue UI + item detail (crop, AI suggestion, top-3) | 5 | 3 | ✅ DONE | Owner tested. Added since: Show filter, separate counts, Session filter |
| S2-03 | QA sampling in the pipeline (qa_sample) | 1.5 | 1.5 | ✅ DONE | Owner verified with qa_sample_rate = 1.0 on a new session, then set back to 0.05 |
| S2-04 | Standards data: verify EWC codes against the official list; look up UNSPSC… | 2.5 | 2 | ✅ DONE | Standards table, verify tool, docs/STANDARDS.md. Owner to run `tools/verify_standards.py --write` and look up UNSPSC when time allows |
| S2-05 | Materials catalogue API + mapping approve/override (relational) | 3 | 2 | ✅ DONE | Owner tested |
| S2-06 | Materials UI | 3 | 2.5 | ✅ DONE | Owner tested |
| S2-07 | Bounding-box overlay synchronised with playback | 3 | 4 | ✅ DONE | Owner tested: video playback with box overlay, seek works |
| S2-08 | Curate ≥ 5 held-out demo videos; run; fill ml/DEMO_VIDEOS.md and the table… | 4 | 0 | 🔵 DOING | CONTINUES IN PARALLEL, NOT BLOCKING: owner collects 5 videos (2 phone, 3 public/licensed), runs EVAL sessions and sends updates later. Tooling is ready (`ml/DEMO_VIDEOS.md`, `tools/demo_report.py`, review Session filter) |
| S2-09 | Spike SP-02 (RF-DETR-Small CPU latency) | 2 | 3 | ✅ DONE | Laptop CPU: YOLOv8n 24.7 ms p50 (41.6 fps), RF-DETR-Small 202.4 ms (4.9 fps); end to end with DINOv2: 71.0 ms and 258.5 ms (p95 433 ms). GPU RTX 5050: 8.4 ms and 24.5 ms. RF-DETR passes 3 fps on the laptop but with little margin end to end; decide on the real cloud VM in S4-02. RTX 50-series needs cu128 PyTorch wheels |

### Sprint 3 — Model Management, Audit UI, Analytics, Preferences

| ID | Task | Est h | Actual h | Status | Notes |
|---|---|---|---|---|---|
| S3-01 | RetrainService: training set, fit, evaluate, artifacts, version row | 4 | 3 | ✅ DONE | Owner check passed: real retrain on dinov2 gave versions 2 to 5 (global accuracy 69 to 70 percent against 71 percent for v1; the domain accuracy stays empty until an EVAL session exists, S3-09) |
| S3-02 | Job runner, status endpoint, rule trigger (cron optional) | 2.5 | 1 | ✅ DONE | Owner check passed: manual and rule-triggered jobs finished. Cron (FR-MODEL-04) not built |
| S3-03 | Activate / rollback: transaction + hot swap + tests | 2 | 1 | ✅ DONE | Owner check passed: activate through the API worked |
| S3-04 | Models UI: list, retrain, activate, compare with active, job status | 5 | 4 | ✅ DONE | Owner check passed: compare view works. First compare showed the active version had no domain score, so S3-12 was added |
| S3-05 | Audit UI (filters, pagination) and item trace page | 4 | 2 | ✅ DONE | Owner check passed: Audit Log works |
| S3-06 | AnalyticsService + endpoints (KPIs, throughput, mix, accuracy by version) | 4 | 2 | ✅ DONE | Owner check passed: summary, series, mix and accuracy verified; Operator sees tiles only. Appendix B choices accepted |
| S3-07 | Dashboard UI: tiles, 3–4 charts, metric tooltips (PRD Appendix B) | 4 | 2 | ✅ DONE | Owner check passed: dashboard works for Admin and Operator |
| S3-08 | Per-user preferences API + UI | 2.5 | 1 | ✅ DONE | Owner check passed: preferences apply after a fresh login |
| S3-09 | EVAL session flag and domain-eval wiring | 1 | 1 | ✅ DONE | Owner check passed: EVAL session run and reviewed; retrain then exposed the not_waste training bug, fixed 2026-11-23 |
| S3-10 | Undo a review within a time window (owner request) | 1.5 | 1 | ✅ DONE | Owner check passed: mistaken review undone |
| S3-11 | Class `not_waste` (Other, not waste) with stream NOT_WASTE (owner request) | 0.5 | 0.5 | ✅ DONE | Owner check passed: not_waste visible in the Review Queue |
| S3-12 | Score any version on the current EVAL reviews (`POST /models/{id}/evaluate`) | 1 | 0.25 | ✅ DONE | Owner check passed: v1 and v7 scored on the same 46 EVAL reviews gave 43.5% and 82.6% |

### Sprint 4 — RF-DETR-Small, webcam, cloud, privacy

| ID | Task | Est h | Actual h | Status | Notes |
|---|---|---|---|---|---|
| S4-01 | RFDETRSmallDetector adapter + single-class fine-tune | 5 | 5 | ✅ DONE | Owner check passed. Fine-tune results (single class, validation split): 10 epochs mAP50:95 0.570 (best at epoch 7), 20 epochs 0.571 (best at epoch 11, nothing better after); mAP50 about 0.73. Installed weights = the 20-epoch run (epoch-11 checkpoint). Next: test-split AP with `ml/scripts/eval_detector.py` for both backends, then CPU speed on the VM (S4-02) |
| S4-02 | CPU benchmark of both detectors; fill ARCH §12.4; decide CPU / ONNX / GPU | 3 | | ⬜ TODO |  |
| S4-03 | ONNX export path (only if S4-02 requires it) | 3 | | ⬜ TODO |  |
| S4-04 | Webcam: capture, device selector, binary frames, back-pressure | 7 | 3 | ✅ DONE | Owner check passed with a real webcam: frames sent, items boxed, camera moved between items |
| S4-05 | Pause/resume/stop; "no frames" banner | 2 | 1 | ✅ DONE | Start, pause, resume, stop verified. The no-frames banner did not show with a covered lens: frames keep arriving (black), so the server sees no gap. Added a client-side "camera image is almost black" warning after 5 s; owner to verify. The no-frames banner still covers lost frames (tab asleep, connection) |
| S4-06 | Cloud VM, domain, Caddy, prod Compose, deploy script; HTTPS webcam test | 8 |  | 🔵 DOING | Files prepared (compose override, Caddyfile, deploy script, guide), not tested. Waiting for the cloud details (G4); parked until they arrive |
| S4-07 | Privacy declaration, retention job, Privacy screen | 5 | 1 | ✅ DONE | Owner check passed: Privacy page looks right. The declaration text still has to be finalised with the supervisor (contact line, review); that is a later, separate step |

### Sprint 5 — Semantic layer and agents

| ID | Task | Est h | Actual h | Status | Notes |
|---|---|---|---|---|---|
| S5-01 | Spike SP-04 (owlrl) | 1 | | ⬜ TODO | = SP-04 |
| S5-02 | Ontology Turtle, graph build from Postgres, OWL-RL closure, degraded mode | 4 | | ⬜ TODO |  |
| S5-03 | SPARQL endpoint + console UI | 4 | | ⬜ TODO |  |
| S5-04 | Alignment suggestions + conflict view | 4 | | ⬜ TODO |  |
| S5-05 | Agents: classes, rules R1–R3, envelope, persistence | 6 | | ⬜ TODO |  |
| S5-06 | Wire agents into pipeline and review; decision trail UI | 3 | | ⬜ TODO |  |
| S5-07 | Tests: inference, R1–R3, round limits | 3 | | ⬜ TODO |  |
| S5-08 | Inferences view in the UI | 2 | | ⬜ TODO |  |

### Sprint 6 — Ask SortSense, Impact Estimator, rehearsal

| ID | Task | Est h | Actual h | Status | Notes |
|---|---|---|---|---|---|
| S6-01 | Spike SP-03 (LLM provider, tool-calling, cost) | 1 | | ⬜ TODO | = SP-03 |
| S6-02 | LLMClient, provider adapter, FakeLLMClient | 2 | | ⬜ TODO |  |
| S6-03 | Tools, assistant loop, guardrails, logging | 6 | | ⬜ TODO |  |
| S6-04 | Chat UI with tool trace | 4 | | ⬜ TODO |  |
| S6-05 | Golden-question run and tests (ARCH §10.4) | 3 | | ⬜ TODO |  |
| S6-06 | ImpactService: pure functions + fixture tests (PRD Appendix A) | 4 | | ⬜ TODO |  |
| S6-07 | Assumptions API/UI, placeholder seed, source banner | 3 | | ⬜ TODO |  |
| S6-08 | Impact screen, endpoint, assistant tool | 3 | | ⬜ TODO |  |
| S6-09 | Full rehearsal of every workflow; fix list | 5 | | ⬜ TODO |  |

### Should items (build in this order; drop in reverse)

| ID | Task | Est h | Actual h | Status | Notes |
|---|---|---|---|---|---|
| S-01 | Alerts (in-app) | 4 | | ⬜ TODO | |
| S-02 | CSV exports | 3 | | ⬜ TODO | |
| S-03 | Billing stub (Stripe test mode) | 5 | | ⬜ TODO | |
| S-04 | Settings UI | 3 | | ⬜ TODO | |
| S-05 | Backup and restore rehearsal (recommended; consider doing early) | 2 | | ⬜ TODO | |
| S-06 | Mobile camera (last, removable) | 12 | | ⬜ TODO | |
| S-07 | Detector fine-tune: full-size TACO, 100 epochs, unseen-video check (from S1-10) | 2 | | ⬜ TODO | Do early if detector misses items in the demo videos |

## Sprint 1 exit checklist (2026-11-12)

- [x] S1-01...S1-15 meet their acceptance criteria (S1-15 verified by the owner 2026-11-12; S1-10 fine-tuning moved to S-07)
- [x] Decisions D-05...D-12 reviewed (G1 cleared 2026-10-14); D-27 and D-28 confirmed 2026-11-05
- [x] SP-01 note written in `ml/DATASETS.md` (provisional go; video table pending)
- [x] Hours tracker updated: Sprint 0 and 1 actual about 34 h of 46 h planned
- [x] Sprint 2 detail written (`SPRINT_BACKLOG.md`, "Sprint 2 detail")

## Plan change log

| Date | Change | Reason |
|---|---|---|
| 2026-10-13 | Added Sprint 0 (environment setup, ≈ 4 h) and this progress file; moved the hours tracker here | Start of development |
| 2026-10-13 | Daily dev loop changed to hybrid: database in Docker, API and frontend run on the laptop with hot reload; full `docker compose up --build` is used to verify and deploy | Faster feedback; Docker rebuilds per code change are slow |
| 2026-10-13 | Frontend scaffold: Next.js 16.4 `cacheComponents` and `partialPrefetching` flags removed; Google Fonts replaced by a system font stack | Simpler client-rendered app; no build-time dependency on an external font service |
| 2026-10-13 | Dev dependency `httpx` replaced by `httpx2` | Starlette 1.x deprecates `httpx` in its TestClient |
| 2026-10-13 | Sprint 0 verified on the laptop (S0-01, S0-02 done; S0-03 pending the container check); S1-01 and S1-03 accepted | Setup log received |
| 2026-10-13 | DEV_SETUP: added "stop the dev servers before `docker compose up`" and the npm audit note | The compose port clash in the setup log was caused by uvicorn still running on port 8000, not by the database |
| 2026-10-14 | G1 cleared (decisions D-05…D-12, D-24…D-26 confirmed); G2 deferred to the end of Sprint 1 | Owner decision |
| 2026-10-14 | Sprint 1 order changed to S1-04 → S1-07 → S1-06 → S1-05; S1-12 will add migration 0003 for `sorting_decisions` (0002 is the app role; corrected 2026-10-24); test line length 120; `httpx2`/Alembic config tidy-ups | The API must run as the restricted app role, which S1-07 creates; ruff line length raised for readability |
| 2026-10-14 | Tests: migrate once per session and reuse one engine (30 → 5 DB connections); `connect_timeout`; `pytest-timeout` 60 s; `.env.example` uses 127.0.0.1; added `tools/db_ping.py` | Database tests took 20+ minutes on the laptop, most likely from slow `localhost` connections; a hung test must fail fast with a stack trace |
| 2026-10-14 | S1-07: found and fixed a bug where a `%` in `APP_DB_PASSWORD` broke the migration (regression test added); compose now requires `APP_DB_PASSWORD` instead of a silent default | Tested with a deliberately hostile password |
| 2026-10-24 | S0-04 and S1-08 closed (hours 0.5 and 2.5). S1-09 scripts: `download_datasets.py`, `prepare_datasets.py`, `ml/configs/label_map.yaml`, `ml/tests`; `pyyaml` added to the `ml` extra; AGENTS.md sprint line updated; G6 added | Image-level splits so no image feeds two sets; detector-test images are forced into the classifier eval side |
| 2026-10-31 | S1-10 code delivered: `app/ml/detector/` (Box, Detector, shared box rules, FakeDetector, YOLOv8nDetector, factory), `artifact_dir` setting, mypy override for ultralytics, `train_detector.py`, `check_detector.py` | Ultralytics is imported lazily so the API and its tests do not need PyTorch |
| 2026-11-05 | S1-11/S1-12 code delivered: embedder (pinned DINOv2, local files), classifier head, registry, evaluation, `sorting_decisions` (migration 0003, `compliance_approved` nullable), `PipelineService`, ML runtime loaded in the API lifespan, `tools/run_pipeline.py`; D-27 and D-28 proposed | Offline-capable startup; honest NULL instead of a fake compliance approval |
| 2026-11-05 | S1-11 and S1-12 closed (2 h, 3 h); D-27 and D-28 confirmed (revisit if the server needs a change) | Laptop checks passed |
| 2026-11-11 | S1-13 code delivered: `/videos`, `/sessions`, `/detections` (+ crop), `/ws/sessions/:id`, event hub, session runner (one at a time), interrupted sessions marked FAILED at start, models load in the background; migration 0004 (`media_assets.original_name`); Detection JSON aligned to ARCH 11.4 (`class`, `top3.code/confidence`, `stream`, `crop_url`, `image_expired`) | One API worker and one GPU; clients can restore state with `GET /detections` |
| 2026-11-12 | S1-14 delivered: Live Sorting page; backend `GET /facility` (threshold for the confidence bands); WebSocket refuses a foreign Origin (cookie-based WebSocket hijacking); `frontend/tests` logic tests (`npm run test:logic`) | Bands must use the facility threshold; a cookie-authenticated WebSocket must check the page origin |
| 2026-11-12 | Sprint 1 close-out: S1-13 and S1-14 closed (2.5 h, 3.5 h); G2 re-baselined from actuals; Sprint 2 detail written (S2-07 now includes a video file endpoint with Range support, which the plan had missed); SP-01 note filled; S1-15 delivered; old scaffold home page guarded by `npm run check:routes` | Planned re-baseline after Sprint 1 |
| 2026-11-13 | Sprint 1 closed. S2-01 delivered (corrections table, review queue and actions, `detection.updated` event, `review` field on the Detection object); S-07 added (detector fine-tune); sidebar fixed to window height | Owner request; S1-10 follow-up |
| 2026-11-19 | S2-01 closed (3 h). S2-02 and S2-07 delivered; `/classes` and `frame_fps_limit` in `/facility` added. Re-plan for the 31 Dec / 7 Jan demo written (proposal) | Owner deadline |
| 2026-11-20 | Owner decision: no cuts, about 20 h/week, cloud details later. S2-03, S2-04, S2-05, S2-06 delivered (code done; owner to verify and to run the standards verification) | Owner decision |
| 2026-11-21 | Hours S2-01...S2-07 recorded (18 h). Review Queue: Show filter (everything / needs review / quality checks), separate counts, Session filter (`GET /review/sessions`). S2-08 tooling (`evaluation.py`, `tools/demo_report.py`, `ml/DEMO_VIDEOS.md`), S2-09 script (`spike_latency.py`) | Owner could not find QA items; S2-08 and S2-09 requested |
| 2026-11-22 | Sprint 2 closed except S2-08, which continues in parallel and does not block Sprint 3. S2-03 and S2-09 done (1.5 h, 3 h). `docs/HANDOFF.md` added for starting a fresh chat | Context size of the working chat; owner request |
| 2026-11-22 | Sprint 3 detail written (hours at 0.8, about 29 h; order S3-01, -02, -03, -09, -04, -08, -05, -06, -07). S3-01...S3-03 delivered: `RetrainService`, `RetrainRunner`, `/models` API, activate / rollback with hot swap, rule trigger after Confirm / Correct; no migration (a job is a `model_versions` row); `docker-compose.yml` mounts `data/artifacts` read-write | Owner request; pitfall in HANDOFF |
| 2026-11-22 | S3-10 (undo a review) and S3-11 (class not_waste) added on owner request; S3-01 and S3-02 owner-checked | Table marks and similar are not waste; a mistaken click must be reversible |
| 2026-11-22 | S3-09, S3-04, S3-05 built: EVAL checkbox (Admin) in Live Sorting; Models UI with `/models/status` and `/models/{id}/metrics`; `GET /audit`, `/audit/actions`, `/audit/trace/detection/{id}` and their pages | Owner request to continue with the UI tasks |
| 2026-11-23 | Bug: retrain failed with `labels not in class_codes: ['not_waste']`. The classifier head has a fixed class list (`CLASS_CODES`) and `not_waste` was not on it. Added at the END of the list (older versions keep their own order in meta.json); tests added for a training review and for an EVAL review of `not_waste` | Found by owner at the first retrain after S3-11 |
| 2026-11-23 | S3-12 added: a version only had a domain score for the EVAL reviews that existed when it was trained, so v1 could not be compared with v7 | Found in the first compare screen (owner screenshot) |
| 2026-11-23 | S3-06, S3-07, S3-08 built in one delivery (analytics API and dashboard, preferences API and page); `/analytics/sessions` added for the session filter | Owner request |
| 2026-11-24 | Sprint 4 detail written; S4-07, S4-04, S4-05, S4-01 and the S4-06 files delivered. Also: `record_detector_backend` keeps the facility row equal to `DETECTOR_BACKEND`; sessions API now accepts `WEBCAM`; WebSocket takes binary frames and `pause`/`resume` controls; the API image takes `BACKEND_EXTRAS` | Continuation after Sprint 3 |
| 2026-11-25 | Owner reports S4-01, S4-04, S4-07 checked; S4-05 gap found (covered lens). Added: dark-camera warning, `ml/scripts/eval_detector.py` (test-split AP for either detector), RF-DETR adapter now passes `num_classes=1` and has an optional `RFDETR_OPTIMIZE` switch | Owner test feedback and training logs |