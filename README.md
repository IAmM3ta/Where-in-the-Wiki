# Where in the Wiki

**Wikipedia racing game with comprehensive path logging for human decision tree capture and ML training toward human-like navigation agents.**

## Core Game Rules

- Players select a subject category (General, Science, History, Sports, Technology, Business).
- Both players receive the **same** random starting Wikipedia article and the **same** random target article drawn from that category's curated pool.
- Race to navigate from Start → Target **exclusively by clicking hyperlinks** present in the article body text. No search bar, no browser back/forward, no external navigation.
- **Scoring**: Winner determined by combined metric prioritizing **fewest clicks** (primary key) then **lowest elapsed time** (secondary). Reaching the target in fewer clicks beats being marginally faster on higher click counts.

## Scoring Formula (Prototype)

```js
score = clicks * 10000 + Math.floor(timeMs / 1000)
```

Clicks are weighted ~10,000× heavier than one second of time. This encodes the design priority: a path with 2 fewer clicks wins even if it took 30+ minutes longer. In full multiplayer the server applies identical weighting and declares the lowest-score player the winner.

## Why Path Logging Matters

This project exists to generate high-quality datasets of *real human goal-conditioned trajectories* on the Wikipedia link graph. Each session records:

- Exact sequence of articles visited
- Per-step timestamps (revealing deliberation time)
- Cumulative click count at each decision point
- Category, start, target, final metrics

These traces enable training of models that internalize human heuristics for multi-hop reasoning and semantic navigation:

- Which links humans prefer (lead-section bias, hub articles, title length, embedding proximity to target)
- How humans balance exploration vs. exploitation when the optimal shortest path is unknown
- Failure modes and recovery strategies

Future deep learning models (graph + sequential policies) trained on this data will exhibit more human-like, robust, and interpretable behavior when performing research, tool-use planning, or open-ended inference over large knowledge bases.

## Current Implementation: Single-File HTML Prototype

`index.html` is a fully functional, zero-dependency (except Tailwind CDN + live Wikipedia Parse API) prototype.

**Implemented:**
- 6 category pools with high-signal, real Wikipedia titles
- Dynamic random Start/Target pair selection per category
- Live Wikipedia content via `action=parse` (cleaned: infoboxes/navboxes/references stripped for navigation focus; images lightly styled)
- Strict link interception: only internal `/wiki/` main-namespace links advance the game
- Real-time clicks counter + mm:ss timer
- Complete path log (step, title, timeMs, clicks_at_step)
- Win detection on canonical title match
- Results panel with score breakdown, path chain, simulated opponent for solo play feel
- One-click JSON export (clipboard + file download) containing everything needed for dataset building
- Mobile-responsive, dark, clean Tailwind UI

**Limitations (by design for v0.1):**
- Hardcoded pools (no live category member sampling) → guarantees playable, relevant articles and avoids complex PetScan/random-in-category calls
- No backend → paths logged client-side only; export for manual aggregation
- No cross-player sync → solo prototype; same start/target logic will be server-enforced later
- No path validation → client-trust for prototype (full version will replay or graph-check edges)

Open `index.html` in any modern browser. Internet required for Wikipedia content.

## Dissertation

The complete dissertation "From Human Trails to Human-Aligned Embeddings: Gamified Elicitation of Decision Trees on Semantic Graphs" is included in the repository:

- **LaTeX source**: `docs/WOW-Dissertation.tex`

It features a detailed worked example of navigating from the "Basketball" article to the "Moon landing" article. This illustrates the human decision process at each step (visible alternatives, heuristics like lead-section bias and hub exploitation, semantic priming), and shows how a single trajectory generates rich supervised training data (positive chosen links + contrastive rejected alternatives + temporal features) for training human-aligned goal-conditioned policies and embeddings.

The compiled PDF provides the full formatted academic presentation.

## Full System Vision & Roadmap

### Phase 1 (Current)
Prototype + local data export. Validate UX and logging fidelity.

### Phase 2 – Backend & Multiplayer
- FastAPI (Python) or Node/Express
- PostgreSQL for users, matches, trajectories (normalized tables: matches, steps, scores)
- WebSocket (or Supabase Realtime) for live race lobbies: generate shared (start, target) via seeded RNG or shared entropy, broadcast progress if desired (or post-hoc reveal)
- Auth (JWT or magic links) so paths are attributable yet privacy-respecting
- On submission: server validates each hop had a real link (best-effort via cached adjacency or on-demand parse), computes authoritative score, stores full trace
- REST endpoints: /matches, /submit-path, /leaderboard, /dataset-export (Parquet)

### Phase 3 – Data & Modeling Pipeline
- Article embeddings (bge-m3, wiki2vec, or fine-tuned)
- Feature vectors per decision: current_article_emb, target_emb, history_summary (mean or LSTM), link_features (position_in_text, is_lead, out_degree, title_len, emb_sim_to_target)
- Supervised dataset: (state, chosen_link, outcome) or full trajectories for sequence modeling
- Model families:
  - Transformer decoder conditioned on goal embedding + graph neighborhood
  - GNN policy network over dynamically induced wiki subgraph
  - Behavioral cloning + Inverse RL (recover latent reward: progress + human-likeness + efficiency)
- Evaluation: human win-rate vs model, path length distribution match, recovery from dead-ends

### Phase 4 – Advanced
- Integration with user's broader stack (OPEV, AR-Chive, metta-thomas-publications, BookHunter) e.g. spatial AR wiki overlays or haptic feedback for path "goodness"
- Public dataset release (with consent/anonymization) for broader research on human information foraging
- Closed-loop: model proposes "interesting" start/target pairs that maximize learning signal (high branching, ambiguous semantics)

## Repository Structure

```
Where-in-the-Wiki/
├── index.html          # Playable prototype + data exporter
├── README.md
├── docs/
│   └── WOW-Dissertation.tex  # Full dissertation source (with Basketball → Moon landing example)
└── (future) backend/   # FastAPI + DB migrations, websocket handlers
```

## Getting Started (Prototype)

```bash
git clone https://github.com/IAmM3ta/Where-in-the-Wiki.git
open index.html   # or serve via any static server
```

## Technical Notes

- Wikipedia Parse API: `https://en.wikipedia.org/w/api.php?action=parse&...&origin=*`
- CORS works in browser; rate limits generous for interactive use
- Title normalization: spaces vs underscores handled; canonical titles from API used for win detection
- Future graph validation will use adjacency lists or on-the-fly link extraction

## License & Philosophy

Open for research/educational use. Strong emphasis on precise data provenance and consent for any ML training on exported human traces. Aligns with user's broader commitment to IP control and high-signal, low-slop knowledge artifacts.

---

Built as cognitive infrastructure for modeling human-like reasoning in large semantic graphs. Each logged path is a window into how minds traverse knowledge under goal pressure.
