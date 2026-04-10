# Album Reviewer

A [QoderWork](https://docs.qoder.com/qoderwork/introduction) skill that searches album reviews from multiple sources and produces comprehensive, bias-minimized music criticism.

## What it does

Given an album name, this skill:

1. Searches 16+ review sources across three tiers (professional critics, aggregators, user communities) using a fast two-pass strategy
2. Extracts verified scores, quotes, and critical perspectives — never fabricates data
3. Analyzes source biases (editorial stance, cultural lens, temporal context) and weaves them inline
4. Generates a long-form essay (1500-3000 words) that walks through the album's emotional arc, not a dry data report

The output reads like a music essay: it opens with historical context, follows the tracklist, integrates critical perspectives with bias annotations, steelmans the strongest counterargument, and closes with the album's place in music history.

## Sources covered

| Tier | Sources |
|------|---------|
| Professional Critics | Pitchfork, AllMusic, NME, Rolling Stone, The Guardian, Consequence of Sound, Stereogum, The Quietus, Bandcamp Daily, Paste Magazine, Clash Magazine |
| Aggregators | Metacritic, Album of the Year |
| User Communities | RateYourMusic, Douban Music (豆瓣音乐), Sputnikmusic |

Each source has a detailed bias profile in `sources-reference.md` covering editorial stance, demographic skew, known scoring patterns, and cultural lens.

## File structure

```
album-reviewer/
├── SKILL.md               # Core skill instructions (132 lines)
├── sources-reference.md   # Bias profiles for 16 sources
├── output-template.md     # Output format template
└── reviews/               # Sample outputs
    ├── ok-computer/
    │   └── review.md      # Radiohead - OK Computer (1997)
    └── first-love/
        └── review.md      # Utada Hikaru - First Love (1999)
```

## Output format

Each review contains:

- **Epigraph** — the single most insightful quote about the album, placed at the top
- **Comprehensive Review** — a long-form essay following the album's emotional arc, with inline bias annotations and source attributions
- **Track Highlights** — 3-5 key tracks with brief descriptions for quick reference

No score tables, no gap analysis sections, no multi-source summary tables. All analytical insights are woven into the prose.

## Key design decisions

**Speed**: Uses a two-pass search strategy. Pass 1 grabs Wikipedia's Critical Reception section (which often contains 10+ review scores in one page). Pass 2 only targets sources where actual review prose is needed for the essay. Total searches: ~5-8 instead of 16+.

**Bias minimization**: Every source citation includes an inline bias annotation (e.g. "Pitchfork, which favors art-rock aesthetics, gave it 10/10"). Counterarguments are mandatory — every review must steelman the strongest dissenting opinion.

**No fabrication**: Scores and quotes must come from actual web searches. Training data fallback is explicitly prohibited. If a source is unreachable, the skill moves on rather than guessing.

**Minimum threshold**: Requires at least 3 verified sources before generating a review. If fewer are found, the skill stops and explains why.

## Install

### Via ClawHub

```bash
clawhub install album-reviewer
```

### Manual

Copy `SKILL.md`, `sources-reference.md`, and `output-template.md` into your QoderWork skills directory:

```bash
# macOS / Linux
cp SKILL.md sources-reference.md output-template.md ~/.qoderwork/skills/album-reviewer/

# Windows
copy SKILL.md sources-reference.md output-template.md %USERPROFILE%\.qoderwork\skills\album-reviewer\
```

## Usage

Just ask QoderWork to review an album:

> "Review OK Computer by Radiohead"

> "帮我评测宇多田光的 First Love"

> "What's the critical consensus on Fela Kuti's Zombie?"

The skill will ask which language you'd like the output in (Chinese, English, or custom), then run the full workflow automatically.

## License

MIT
