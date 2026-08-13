# Music Caption Rewriter Skill

This is a Music 3.0 Structured Caption rewriting skill that operates entirely on natural language and local text files. It does not utilize scripts, embeddings, vector databases, external APIs, or third-party dependencies.

## How It Works

The skill receives the user's `Caption` and optional `Lyrics`, then completes the rewrite through progressive disclosure:

1.  **Extraction**: Extracts genre, mood, tempo, vocals, instruments, production texture, and section instructions from the input.
2.  **Routing**: Reads a small genre router table to determine a primary style family; for fusion requests, at most one secondary family is added.
3.  **Indexing**: Reads only the compact "Style Card" index for the selected family/families.
4.  **Selection**: Selects 1–3 references to fulfill the roles of `Foundation`, `Modifier`, and `Arrangement`.
5.  **Retrieval**: Opens only the full Caption templates corresponding to these references.
6.  **Redesign**: Redesigns the song timeline around the user input and outputs a new Structured Caption.

*Full templates are used only to provide musical design language and must not be directly copied.*

## Directory Structure

```text
music-caption-rewriter/
├── SKILL.md
├── agents/openai.yaml
├── references/
│   ├── genre-router.md
│   └── index-*.md
└── templates/
    └── 1,000 full Caption templates
```

`SKILL.md` stores the core workflow, constraint priorities, output contracts, and static library maintenance rules. `genre-router.md` serves as the first-tier index, the 18 `index-*.md` files serve as the second-tier index, and `templates/` contains the final layer of full references.

A standard request only reads:
```text
SKILL.md
→ genre-router.md
→ 1 Family Index
→ 1–3 Full Templates
```
Explicit fusion genre requests read a maximum of two family indices.

## Input Example

```text
Caption: A melancholic Mandarin pop song with intimate female vocals and clean guitar.
Lyrics: [Verse] sparse piano and close vocal
[Chorus] add wide backing vocals and heavier live drums
```

**Note on Lyrics**: The lyric body text is used solely to understand the overall mood and narrative intensity; it must not be quoted, paraphrased, summarized, or rewritten. Only the structure, instrument, vocal, and dynamic instructions carried within the **square bracket tags** will enter the final arrangement as executable constraints.

## Output Structure

The result includes:

- `Global Metadata`
- `Vocal Details`
- `Arrangement`

The `Arrangement` uses a timeline based on sections such as Intro, Verse, Chorus, Bridge, and Outro, focusing on how instruments and energy enter, transition, and exit.

The default output is English. For explicit instrumental requests, the `Vocal Details` section is retained but used only to declare the work as non-vocal and to specify which instrument or texture carries the lead melody.

## Maintenance

When adding new templates:
1. Place the full Caption into `templates/`.
2. Manually add one compact Style Card to **exactly one** family index.
3. Record fusion relationships in the `Secondary routes` field; do not duplicate templates or create global indices.