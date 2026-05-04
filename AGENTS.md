# AGENTS.md — LLM Wiki Schema

You are maintaining a persistent, interlinked wiki on **Polish 17th century history**, grounded in primary sources (e.g. Volumina Legum, sejm records, diaries, correspondence) and secondary scholarship.

## Directory Structure
- `raw/` — Immutable source documents. Never modify.
- `wiki/` — Your output. Create, update, and maintain all markdown files here.
- `AGENTS.md` — This schema. Update it as conventions evolve.

## Wiki Conventions
- **Language**: Polish (unless quoting Latin/foreign sources).
- **Dates**: ISO 8601 when precise (`1655-07-01`), or `YYYY-MM` / `YYYY`. Note Old Style vs New Style if relevant.
- **Names**: Use Polish forms (e.g. `Jan Kazimierz`, not `John Casimir`). Include Latin variants in parentheses on first mention if the source uses them.
- **Citations**: Always cite the source file and page/section. Format: `([Source filename], s. X)`.
- **Cross-references**: Use `[[Page Name]]` Obsidian-style links liberally. Every entity, concept, and event mentioned should link to its page if one exists, or signal that one should be created.
- **Frontmatter**: Add YAML frontmatter to pages:
  ```yaml
  ---
  type: entity|concept|event|source|overview
  tags: [tag1, tag2]
  created: YYYY-MM-DD
  updated: YYYY-MM-DD
  sources: [raw/filename.pdf]
  ---
  ```

## Page Types
- **Overview** — High-level synthesis of a topic or period.
- **Entity** — Person, institution, place. Include lifespan, roles, key actions, relationships.
- **Concept** — Legal term, social structure, doctrine. Define, trace evolution, note disputes.
- **Event** — Battle, sejm, treaty, uprising. Include date, actors, outcome, significance.
- **Source** — Summary of a raw document. Key takeaways, reliability notes, connections to other pages.

## Workflows

### Ingest (new source in `raw/`)

**Rule: ingest one file at a time.** When multiple new files appear in `raw/`, process them sequentially — one source, then stop and wait for the user to confirm or add the next. Do not batch-ingest multiple sources in a single pass. This keeps each ingest focused, allows the user to review changes incrementally, and avoids errors from context overload.

Per-file workflow:
1. Read the single source thoroughly.
2. Draft a `wiki/Sources/<Source Name>.md` summary.
3. Identify all entities, concepts, events mentioned.
4. Create or update their pages in `wiki/Entities/`, `wiki/Concepts/`, `wiki/Events/`.
5. Update `wiki/index.md` with new/changed pages.
6. Append to `wiki/log.md`: `## [YYYY-MM-DD] ingest | <Source Name>`
7. Flag contradictions, uncertainties, or gaps for the user.
8. Report completion and ask if the user wants to ingest the next file.

### Query
1. Search `wiki/index.md` and relevant pages. Feel free to check as many pages as you need to gather information.
2. Try to minimize your own opinions and base on the wiki. Your job is to provide information based on wiki or to inform user, that there is no information regrading given topic.
3. Synthesize an answer with citations. Use source files listed in the pages for citations. It is crucial that user can see the source of the information.
4. If the answer is substantial, propose saving it as a new wiki page (e.g. analysis, comparison).
5. Format the answer as a paragraphs. Don't add tables. 

### Lint (periodic health check)
1. Check for contradictions between pages.
2. Find orphan pages (no inbound links).
3. Identify concepts mentioned but lacking pages.
4. Suggest missing cross-references.
5. Report stale claims superseded by newer sources.

## Output Rules
- Never modify `raw/`.
- Always update `index.md` and `log.md` after changes.
- Keep pages concise; split if they exceed ~1500 words.
- Use tables for comparisons, timelines for sequences.
- Flag uncertainty explicitly: `[?]` or `[do weryfikacji]`.
