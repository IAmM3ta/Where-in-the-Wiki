# Where in the Wiki

**Author:** Robert F. Thomas (publishing as Metta Thomas)

**Status:** playable prototype + planned pilot. This repository does not report a collected path dataset, a trained embedding phi_h, a trained policy pi_h, or any win rate, KL, or recovery metric. Do not treat the long-form TeX as a completed dissertation.

Wikipedia racing prototype with client-side path logging, aimed at a future study of human decision traces on the live English Wikipedia link graph.

## Core Game Rules

- Players select a subject category (General, Science, History, Sports, Technology, Business).
- The player receives a random starting Wikipedia article and a random target article drawn from that category's curated pool.
- Race to navigate from Start → Target **exclusively by clicking hyperlinks** present in the article body text. No search bar, no browser back/forward as a game move, no external navigation.
- **Scoring**: combined metric prioritizing **fewest clicks** (primary) then **lowest elapsed time** (secondary).

## Scoring Formula (Prototype)

```js
score = clicks * 10000 + Math.floor(timeMs / 1000)
```

Clicks are weighted ~10,000× heavier than one second of time. A solo mode shows a simulated opponent for engagement only. That opponent is not a scientific control and its outcomes are not results.

## What logging is for

The project is designed to support later collection of goal-conditioned human trajectories. Each exported session can record:

- Sequence of articles visited
- Per-step timestamps
- Cumulative click count
- Category, start, target, score
- `consentAcknowledged` (pre-play checkbox)
- `contrastiveRejectedLinkLogging: "planned"` (not collected yet)

These traces are a proposed training signal for models of human multi-hop navigation. No such model has been trained in this repository. Future work, after a consented pilot and edge validation, would ask which links humans prefer and how they trade exploration against progress. That question is open.

## Current Implementation: Single-File HTML Prototype

`index.html` is a playable, zero-backend prototype (Tailwind CDN + live Wikipedia Parse API).

**Implemented:**

- 6 category pools with real Wikipedia titles
- Random Start/Target pair per category
- Live Wikipedia content via `action=parse` (infoboxes, navboxes, and references stripped for navigation focus)
- Link interception: internal `/wiki/` main-namespace links advance the game
- Clicks counter and mm:ss timer
- Path log (step, title, time, clicks)
- Win detection on canonical title match
- Results panel and one-click JSON export (clipboard + download)
- Consent checkbox that gates Start. The notice states that paths may be stored for research and that no IRB approval is claimed
- Mobile-responsive dark UI

**Not implemented (do not describe as done):**

- Contrastive dump of visible but unchosen links
- Server-side edge validation
- Multiplayer or shared lobbies
- Any trained phi_h / pi_h
- A public path dataset

**Limitations of v0.1:**

- Hardcoded pools (no live category-member sampling)
- Paths stay in the browser until the player exports them
- Client-trust: hops are not replayed against the API before export
- Self-selected play; no demographics

Open `index.html` in a modern browser. Internet is required for Wikipedia content. Acknowledge the consent notice before Start.

## Long-form draft

`docs/WOW-Dissertation.tex` is a long-form technical draft (June 2026, claim-surgery revision October 2026), not a degree dissertation and not a results paper. An October 2026 revision note at the front states the status: systems proposal, prototype, and planned protocol.

- LaTeX source: `docs/WOW-Dissertation.tex`
- Related work: `docs/RELATED-WORK.md` (Wikispeedia; West & Leskovec)
- Pilot plan: `docs/PILOT-PROTOCOL.md` (targets only; no collected N)
- `docs/WOW-Dissertation_RFT.pdf`, if present, is the pre-revision compile and is **stale until recompiled**

The Basketball → Moon landing walkthrough in the TeX is illustrative, not logged pilot data.

## Roadmap (future; not done)

### Phase 1 (current)

Prototype, local export, consent gate. UX and logging fidelity only.

### Phase 2 – Backend and validation

- Server that stores consented matches
- Replay of each hop against the Parse API or a cached adjacency list
- Authoritative score
- Dataset export only after anonymization and consent

### Phase 3 – Modeling (only after real traces)

- Features per decision (position, degree, lexical overlap, time)
- Behavioral cloning as a planned method, not a result
- Baselines: shortest path, random click, random-walk embeddings
- Report nulls if a model does not help

### Phase 4 – Later

- Optional public release of an anonymized set
- Broader recruitment
- No claim of community scale until that recruitment exists

## Repository structure

```
Where-in-the-Wiki/
├── index.html
├── README.md
└── docs/
    ├── WOW-Dissertation.tex
    ├── WOW-Dissertation_RFT.pdf   # stale until recompiled
    ├── RELATED-WORK.md
    └── PILOT-PROTOCOL.md
```

## Getting started

```bash
git clone https://github.com/IAmM3ta/Where-in-the-Wiki.git
# open index.html, or serve the directory with any static server
```

## Technical notes

- Wikipedia Parse API: `https://en.wikipedia.org/w/api.php?action=parse&...&origin=*`
- Identify the client and respect Wikimedia rate limits before any automated crawl. Interactive play is the intended use.
- Title normalization uses canonical titles from the API for win detection.

## License and ethics

Open for research and educational use. Path logs are behavioral data. The prototype asks for acknowledgment before play and does not claim an IRB protocol. Separate opt-in for model training, anonymization, and a documented exemption or review are specified in `docs/PILOT-PROTOCOL.md` and are not yet completed.

---

Built as a proposal for studying human navigation on a large semantic graph. A logged path is not, by itself, evidence that a human-aligned embedding exists.
