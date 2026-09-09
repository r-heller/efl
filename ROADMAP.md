# EFL — roadmap to completion

**Audited 2026-09-09** against `curriculum` v1.1.0 and the `pagegen` template.
The July `HANDOVER.md` remains as the historical record; this document supersedes
its open items.

**Status: EFL is the reference implementation of the suite.** Where the three
mature courses disagree on structure, EFL is right. What it lacks is not content
but the one file that makes its conformance claim checkable.

---

## 1. Measured state

| | |
|---|---|
| Unit bundles | **180** (`unitNN-slug/index.md`) |
| Exam bundles | **180** (`unitNN-slug-exam/index.md`, `page_type: exam`) |
| Other pages | 28 (schedules, legal, about, references) |
| `page_type` discriminator | on all 388 pages |
| `curriculum:` front-matter block | on all 360 units and exams |
| Naming convention | `unitNN-slug` — conformant |
| Forbidden `slug:` in front matter | none |
| Raw HTML section markup in content | none |
| Quarto remnants | none |
| Materials | 739 PDF |
| Audio | 400 files |
| `conformance.yml` | **missing** |
| `[taxonomies]` in `hugo.toml` | **missing** |
| `navTitle` | missing (still `navbarTitle`) |

## 2. What "finished" means here

`declared_conformance: core` (A1–B1) proven by `conformance_audit.py resolve`,
with B2/C1 recorded as declared gaps rather than silently absent.

## 3. Roadmap

### Phase 1 — make the conformance claim checkable (S, blocks the whole suite)

- [ ] Write `conformance.yml` at the repo root: `framework: boulingua-curriculum`,
      `framework_version: 1.1.0`, `language: en`, `declared_conformance: core`,
      `conformance_status: in-progress`.
- [ ] Populate `realizations` from the `curriculum:` blocks already present on the
      360 unit and exam pages. This is an extraction, not an authoring task —
      the descriptor IDs exist; they are simply not aggregated anywhere a machine
      can read them.
- [ ] Run `curriculum/scripts/conformance_audit.py resolve` against the manifest
      and fix every ID that does not resolve.
- [ ] Add that check to CI via `boulingua/.github`'s `course-build.yml`.
- [ ] Emit the coverage report and record which `core` cells are unpopulated.
      Per `curriculum/docs/conformance.md` an empty cell is satisfied by
      `no-official-descriptor`; a silently missing scale is a failure.

**Why first:** three courses hold descriptor IDs and none can prove anything with
them. EFL is the cheapest place to prove the mechanism end to end, and the other
two then copy a working file rather than inventing one.

### Phase 2 — close the template gaps (S)

- [ ] Declare `[taxonomies]` (tag, skill, level, topic) in `hugo.toml`. No course
      in the organisation does this today; fixing it here and in `pagegen`
      together stops the fifteen scaffolds inheriting the omission.
- [ ] Rename `navbarTitle` → `navTitle`.
- [ ] Confirm every committed PDF carries `/Author` (the
      `check_pdf_attribution.py` gate exists; verify it actually runs on all 739).

### Phase 3 — level extension (L, authoring)

- [ ] Decide whether EFL declares `full` (A1–C1). The Klassen 5–13 span implies
      B2/C1 material exists; the manifest from Phase 1 will show how much of it
      maps to B2/C1 descriptors.
- [ ] Fill the `core` cells the coverage report shows as unpopulated, or record
      them as declared gaps with a CV citation.

### Phase 4 — keep the reference honest (ongoing)

- [ ] Enable `kit-drift.yml` so a locally-copied layout or shortcode fails CI.
- [ ] Treat any structural divergence between EFL and `pagegen` as a bug in one
      of the two, and fix it in the same week it appears.

## 4. Gaps declared, not hidden

- B2/C1 coverage is unquantified until Phase 1 produces a coverage report.
- The 28 non-unit pages carry no `curriculum:` block. That is correct — schedules
  and legal pages realize no descriptor — but it should be stated in the manifest
  rather than left to inference.

## 5. Dependencies

- `curriculum` ≥ 1.1.0 for the audit script and the scale registry.
- `kit` v1.21.0 for layouts, shortcodes and CSS tokens.
- `boulingua/.github` for shared CI.
