# Remember original upload hashes across CWA transformations
Idea: https://ideaforge.ikoro.in/idea/1235

## Why
Repeated imports of identical original files create extra books when metadata matching fails. Stored file hashes change after conversion or metadata enforcement. Upstream issue765 raised exactly this limitation.

## How
Store original SHA-256 provenance in cwa.db, associated with library UUID, book UUID and format. Compute original bytes before transformation. Record only confirmed successful imports, never heuristic latest-book IDs. Validate live references before skipping. Preserve explicit add-format behavior, invalidate replaced/deleted relationships and transfer only surviving formats on merges. Keep legacy current-file matching and approximate duplicate management separate. Do not delete existing duplicates. Submit upstream and validate a pinned local image on an isolated library before rollout.

## Validation Commands
python -m pytest tests/unit/test_original_upload_hashes.py tests/unit/test_cwa_db.py tests/unit/test_ingest_batch_dirty.py tests/unit/test_ingest_processor_startup.py
python -m compileall -q scripts

### Task 1: Review design
- [ ] Inspect ingest and lifecycle integration points; record Claude design review and resolve findings.

### Task 2: Recognize repeated originals
- [ ] Add CWA-owned persistent provenance, safe ingest lookup and success-only recording.
- [ ] Cover transformations, changed bytes, failure/retry, deleted/replaced formats, library identity and legacy files.

### Task 3: Review and deliver
- [ ] Complete native Review Gate and Claude peer attempt, resolving valid findings.
- [ ] Validate a pinned candidate image on an isolated library copy.
- [ ] Publish an upstream PR with attribution, linked issue and validation evidence.
