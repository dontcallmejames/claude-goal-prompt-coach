# Proof run — coach executed end-to-end on a fresh idea

This is a captured run of the coach following the documented procedure
(`skills/goal-prompt-coach/SKILL.md`) on an idea not used elsewhere, to confirm
the output is paste-ready into a Claude `/goal`. The emitted prompt below is
**1,752 characters** — well under the 4,000-char `/goal` limit — and contains
all five required sections with evaluable success criteria.

## Input

```
/goal-prompt-coach:goal-coach a little CLI that renames my photos by the date they were taken
```

## Turn 1 — coach asks

**Your idea, restated:** A command-line tool that renames image files based on
their capture date.

**My assumptions (correct any):**
- Capture date comes from EXIF metadata, falling back to file mtime when EXIF is
  absent.
- Rename format is date-based and sortable (e.g. `2026-06-02_143501.jpg`).
- It operates on a folder you point it at, in place.
- Python is fine (ubiquitous, good EXIF libs).

**Questions — defaults in brackets:**
1. **Proof it works?** [a test suite over sample images with known dates passes]
2. **Collisions?** Two photos same second — what happens? [append `_1`, `_2`]
3. **Safety?** Dry-run first, or rename immediately? [`--dry-run` flag; real
   rename only without it]
4. **Scope?** Recurse subfolders? [top-level only unless `--recursive`]

## Your answers

> Python's good. EXIF with mtime fallback. Dry-run by default actually — make me
> pass --apply to really rename. Collisions append _1/_2. Defaults otherwise.

## Turn 2 — coach delivers

```text
/goal Build photo-rename: a Python CLI that renames image files in a folder to their capture date (EXIF, mtime fallback), dry-run by default.

## Goal
A single-file Python CLI (photo_rename.py) that renames images in a target folder to a sortable capture-date name, previewing by default and only writing files when --apply is passed.

## Success condition (verify before stopping)
Met ONLY when all are objectively true:
1. `python photo_rename.py <folder>` prints planned renames and changes nothing on disk (dry-run by default).
2. `python photo_rename.py <folder> --apply` renames files to YYYY-MM-DD_HHMMSS.<ext> using EXIF DateTimeOriginal, falling back to file mtime when EXIF is missing.
3. Same-second collisions get _1, _2 suffixes; no file is overwritten (verified by a test with two same-timestamp images).
4. A pytest suite covers: EXIF read, mtime fallback, collision suffixing, and dry-run-writes-nothing. `pytest` exits 0 with all tests passing.
5. `python photo_rename.py --help` documents the folder arg, --apply, and --recursive.

## Constraints
- Standard library plus one EXIF lib (Pillow); no other heavy deps.
- Never overwrite an existing file. Default is dry-run; only --apply mutates.
- Top-level folder only unless --recursive is passed.

## Iteration policy
- Write tests first from the criteria, then implement until they pass.
- Run pytest after each change; fix and re-run until all 5 criteria hold.

## Stop conditions
- DONE: all tests pass and a manual dry-run + --apply on sample images behaves per spec. Report the command list and test output.
- BLOCKED (stop + ask): if EXIF parsing needs a dependency beyond Pillow, or sample images aren't available to test against. State the blocker and your recommended option.
```

**Handoff:** The most likely BLOCKED trigger is missing sample images to test
against — the prompt tells the agent to surface that rather than ship untested.
Ready to paste into a new `/goal`.

## Verification of this run

- Sections present: Goal, Success condition, Constraints, Iteration policy,
  Stop conditions (DONE + BLOCKED). ✅ (criterion 4)
- Every success criterion is bound to a command, file, or test — evaluable. ✅
  (criterion 5)
- Emitted prompt length: 1,752 chars < 4,000. ✅ (paste-ready)
