# Related work — Where in the Wiki

**Author:** Robert F. Thomas (publishing as Metta Thomas)
**Date:** October 2026
**Purpose:** Concise differentiation for the WOW draft. This note cites prior art. It does not report WOW empirical results.

## Honest novelty claim

WOW repositions goal-directed Wikipedia racing as a live-graph, time-stamped, category-controlled elicitation apparatus aimed at behavioral cloning of human decision traces for human-aligned embeddings. It builds on Wikispeedia’s wayfinding and semantic-distance results. It does not replace them, and it does not claim to have trained phi_h or pi_h.

## Must-cite

1. **Wikispeedia — West, Pineau, and Precup (IJCAI 2009).** Online game on a reduced Wikipedia graph. Players go from start to target by clicking links. Paths were used to infer semantic relatedness. The paper already notes that computers take shortest paths while humans use common-sense associations (hub zoom-out, then home-in).
2. **Human wayfinding — West and Leskovec (WWW 2012).** Analysis of more than 30,000 Wikispeedia paths. Humans are efficient but not shortest-path: early high-degree hubs, later content features, and a simplicity–efficiency tradeoff. This is the empirical predecessor of the divergence conjecture, which remains untested on WOW traces.
3. **SNAP Wikispeedia dataset.** https://snap.stanford.edu/data/wikispeedia.html — about 4,604 articles, about 119,882 links, 51,318 finished paths and 24,875 unfinished paths in the public release. Baseline corpus. WOW must not speak as if it already has “thousands of paths” while this release already does.
4. **West (2016),** *Human Navigation of Information Networks*, Stanford PhD. Optional broader framing.
5. **The Wiki Game / Wikiracing.** Folk and commercial genre. Wikispeedia itself notes it. WOW does not claim to invent the mechanic. The proposed contribution is a research apparatus: logging, scoring, category pools, and a planned cloning pipeline.

## How WOW differs

| Dimension | Wikispeedia / SNAP | Wiki Game (folk) | WOW (this work) |
|-----------|--------------------|------------------|-----------------|
| Graph | Frozen ~4.6k-article subgraph | Live Wikipedia (typical) | Live English Wikipedia via the Parse API |
| Research product | Semantic distance; wayfinding; target prediction | Entertainment | Planned elicitation for behavioral cloning / phi_h |
| Logging | Finished and unfinished paths | Rarely research-grade | Path, timestamps, clicks, score. Rejected links: planned, not collected |
| Scoring | Minimize clicks; backtracking allowed | Varies | Lexicographic clicks × 10000 + seconds |
| Pools | Snapshot-wide | Open | Curated category pools in the prototype |
| Empirics | Large published corpora | Large play, little open research data | Prototype only. Pilot planned (≥100–500 finished paths). No trained phi_h |
| Stance | Established findings | Game | Systems proposal + prototype + protocol |

## Adjacent work to keep in view

- Pirolli and Card (1999), information foraging: scent and patch leaving. WOW would operationalize scent under clicks-first scoring. Not yet estimated.
- Lamprecht et al. (on Wikipedia article structure and navigation): lead-section and positional bias. Independent support for a predicted signature, not a WOW result.
- DeepWalk, node2vec, GCN: topology embeddings. Planned baselines. Not a claimed win.
- Behavioral cloning, GAIL (Ho and Ermon 2016), preference RL (Christiano et al. 2017): methods intended for later use.
- Recent LLM agents on WikiGame-style tasks (2024–2025): concurrent interest. Not a uniqueness claim.
- Shneiderman (2020): design stance, not a navigation dataset. Not an existence proof for WOW.

## Do not write

- That WOW discovered humans ≠ shortest paths.
- That phi_h was reverse-engineered or that pi_h was trained.
- Any WOW path count, win rate, KL, or recovery rate.
- That a community-scale WOW dataset has been released.
