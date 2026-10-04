# PILOT-PROTOCOL — Where-in-the-Wiki Minimal Pilot

**Adapted for this repository:** October 2026.
**Source:** claim-surgery revision pack (`PILOT-PROTOCOL.md`, 2026-10-01 ET).
**Status:** protocol only. This file does not report a pilot. No path counts, descriptives, or model metrics have been collected or invented.
**Author framing:** Robert F. Thomas (publishing as Metta Thomas)
**Goal:** Collect enough real, consenting human paths to (a) support CHI LBW/Demo honesty, (b) unblock weak descriptives / optional tiny BC baseline, (c) **never invent numbers**.  
**Targets:** **Minimum ≥100 finished paths**; **preferred 300–500** for stabler descriptives; stretch 1–2k before full-paper claims. (Original TeX full-test estimate N ≈ 5×10³–10⁴ remains a *later* scale goal, not the LBW bar.)

---

## 1. Research questions (pilot-scoped)

**RQ1 (descriptive):** On live EN Wikipedia with WOW scoring, what are path-length, time, and hub-visitation distributions for finished races?  
**RQ2 (contrast):** Do pilot paths show hub-first then content-guided structure consistent with West & Leskovec (WWW 2012), on a *live* graph?  
**RQ3 (systems):** Is client logging + (offline) edge validation sufficient to produce a clean analysis set?  
**RQ4 (optional ML):** Can a simple goal-conditioned classifier/MLP over hand features achieve above-chance next-link prediction on holdout? — **only if N and validation allow; report nulls honestly.**

Out of scope for pilot: claiming ϕ_h geometry superiority, RAG transfer gains, or “thousands of demonstrations reverse-engineered.”

---

## 2. Participants and recruitment

| Element | Spec |
|---------|------|
| Population | Adults (≥18) fluent enough in English to read Wikipedia |
| N participants | Enough to reach path targets (e.g., 20–50 people × multiple races); report unique players |
| Recruitment | Personal network, research mailing lists, campus/lab volunteers, open call — **no deception** |
| Exclusion | Incomplete sessions; idle > threshold (e.g., 10 min no click); k > 30 optional down-weight (align with TeX filter) |
| Covariates (optional P2) | Self-reported wiki familiarity (Likert); optional domain expertise; age band; no unnecessary PII |

---

## 3. Apparatus

- Prototype: `index.html` from `IAmM3ta/Where-in-the-Wiki` (pin commit SHA in study README).  
- Categories: use all six pools or freeze a subset for balance; record pool version.  
- Pre-pilot engineering (strongly recommended before “real” N):  
  1. Consent interstitial (accept/decline; training opt-in separate).  
  2. Export schema includes `consent_research`, `consent_ml_train`, `client_sha`, `utc_iso`.  
  3. Best-effort dump of **visible candidate links** at each step (contrastive set).  
  4. Identifying User-Agent string for API calls; client-side throttle if needed.  
- Post-hoc **edge validation:** script replays each hop via Parse API or cached adjacency; drop/flag illegal edges.

---

## 4. Procedure

1. Consent form (screen or paper) → assign anonymous session ID.  
2. Short tutorial (1 page): rules, scoring, no search/back.  
3. Practice race (optional, discard or flag `practice=true`).  
4. **3–10** scored races per participant (fatigue-aware); encourage ≥1 category change.  
5. Auto JSON export + optional researcher upload to secure store.  
6. Debrief: purpose (HCI navigation research / optional ML); withdrawal contact.

Session length: ~20–40 minutes typical.

---

## 5. Consent sketch (not legal advice — adapt with counsel/IRB)

**Title:** Where in the Wiki — Path Logging Study  

**Purpose:** We study how people navigate Wikipedia by clicking links to go from one article to another. We collect the sequence of pages you visit, timing, and click counts to understand human strategies and to improve future interfaces and (if you opt in) machine-learning models.

**Procedures:** Play a browser game; no Wikipedia edits; internet required.

**Risks:** Minimal (eye strain, mild competitive stress). No sensitive topics required.

**Data:** Paths, timestamps, optional pseudonym, optional survey answers. No need for real name. Stored securely; public release only if anonymized and consented.

**ML training:** Separate yes/no: “I allow my anonymized paths to be used to train machine-learning models.” Decline = research analysis only / exclude from train split.

**Voluntary:** Stop anytime; request deletion of your session IDs where feasible.

**Contact / IRB:** [PI contact]; [IRB protocol # or exemption determination].

---

## 6. Metrics to report (descriptive first)

### Primary descriptives (required for pilot write-up)

| Metric | Definition |
|--------|------------|
| N_finished / N_unfinished | Counts |
| Path length k | Clicks (or hops) distribution: mean, median, IQR, histogram |
| Time | Total seconds; per-step ∆t distribution |
| Score | WOW composite distribution |
| Success rate | Finished / started (define start clearly) |
| Hub rate | Fraction of steps whose article out-degree (or in-degree) is in top p% of observed |
| Category breakdown | N and mean k per category |

### Comparison baselines (report without overclaim)

| Baseline | How to compute | Use |
|----------|----------------|-----|
| **Shortest-path length** | BFS on snapshot or best-effort live graph between (s,g) | Human k − SP gap (as in West & Leskovec spirit) |
| **SNAP Wikispeedia** | Published finished-path length stats | Qualitative/distributional comparison only (different graph!) |
| **Random-click null** | At each step, uniform among visible links; simulate | Sanity check that humans beat random success |
| **Lead-link heuristic** | Always click first lead-section internal link (if logged) | Simple HCI baseline |
| **Embedding-greedy (optional)** | Pick visible link maximizing cosine(emb(link), emb(g)) using off-the-shelf wiki/text emb | Adjacent “scent” baseline — report honestly |

### Optional ML metrics (only if trained)

- Next-action accuracy / MRR @1–5 on holdout  
- Action-distribution KL vs human  
- Mean clicks for rolled-out policy (with hard step cap)  
**If weak or null: say so.** Do not invent Table 1.

---

## 7. Analysis plan (pre-register lightly)

1. Filter invalid edges; report exclusion N.  
2. Descriptives + plots (path length vs SP length scatter).  
3. Test/report early-vs-late hub degree pattern (WWW 2012 replication attempt on live graph).  
4. If contrastive logs exist: feature importance for chosen vs rejected (position, degree, lexical overlap).  
5. Optional: 80/20 BC split; logistic regression over features before any neural net.  
6. **No** ϕ_h “geometry conjecture confirmed” language unless preregistered tests pass.

---

## 8. Ethics / compliance checklist

- [ ] IRB submitted or exemption documented  
- [ ] Consent + ML opt-in implemented  
- [ ] Wikimedia User-Agent + rate-limit policy written  
- [ ] No collection of passwords, emails (unless needed for compensation — prefer codes)  
- [ ] Anonymization plan for any public release  
- [ ] Competitive bias disclosed in Limitations  
- [ ] Data retention / deletion policy stated  

---

## 9. Deliverables after pilot

1. Anonymized Parquet/JSONL + schema README + commit SHA + collection date range (ET).  
2. 1–2 page Results addendum for `WOW-conference-draft.md` (descriptives only unless ML run).  
3. Update RAZOR/Gita blockers: clear P1-1; keep ML claims gated.  
4. Decision gate: LBW/Demo submit vs wait for 1–2k paths.

---

## 10. Explicit non-goals

- Do not fabricate path counts or metrics.  
- Do not claim community-scale dataset.  
- Do not merge this study with UAP / gravity / MODE packets.  
- Do not treat simulated opponent outcomes as scientific controls.
