# PR Response Doc — CineLog Watchlist Feature

## AI Usage
Used Claude AI  for required file and function walkthrough.

## Comment 1 — Rename
**Searched all references for 'save_to_watchlist()' and replaced it with 'add_to_watchlist() in services/watchlist_service.py andin routes/watchlist/watchlist.py**
**Ran the full test suite and verified the changes**

## Comment 2 — Deduplication
**Add deduplication logic to add_to_watchlist() in services/watchlist_service.py by referring the similar code snippet, add_to_collection() in services/collection_service.py. Also updated db.UniqueConstraint for watchlistEntry in models.py**
**Ran the full test suite and verified the changes**

## Comment 3 — Missing test
**Create a new file tests/test_watchlist.py and added new test case, test_add_to_cwatchlist_nonexistent_film_raises by referring to the similar test case in tests/test_collection.py. Also made this test file structure like imports and doc string etc.,similar to test_collection.py**
**Ran test_collection.py, full test suite and verified the changes**

## Comment 4 — Default visibility
**My position: It is better to set public=False and choose 'opt-in' for visibility**
**Reasoning: In the modern age we live, privacy principle is vital. It can also be considered as a sort of security. The user should never be surprised to discover their personal watchlist preferences got exposed to public**
**Tradeoff acknowledged: Though public visibility has its own advantages since CineLog being a social app, I weigh protecting user watchlist preferences and honor their privacy**

## Comment 5 — Sort order
**My position: I prefer to sort by most recently added watchlist preference. (Sorting by date_added in descending order)**
**Reasoning: Watchlist preference means what we want to watch next? The film we just added reflects what is our recent preference. So Sorting by date_added in descending order will be preferred by most of the users by default**
**Engagement with reviewer's point: The reviewer's point is that watchlist and collection should sort consistently. I agree that it might be preferred by selctive groups of users. But I strongly suggest sorting by date_added in descending order, because sorting by recent films are more relevant and will be welcomed by most of the users**

## Comment 6 — Rebase
**What conflicted: .gitignore has different set of entries,  models.py has another conflict. The main branch had migrated film IDs from integer to UUID **
**How I resolved it: I merged the two into a single file containing the union of ignore entries. In models.py, changed the film_id datatype from db.Integer to db.String(36) in WatchlistEntry class. Also reflected this change in the doc string of watchlist_service.py**
**How I verified no conflict remains: Ran the complete test suite and verified all test cases got passed, Ran python -c "import ast; ast.parse(...)" to confirm models.py had no leftover conflict markers and parsed cleanly. Also ran git log --oneline --graph and confirmed the feature branch history is linear with no merge commits**

## Stretch Features

### remove_from_watchlist()
**What I did:** Implemented `remove_from_watchlist(user_id, film_id)` in
`services/watchlist_service.py`, following the same pattern as
`remove_from_collection()`: query for the entry by user_id + film_id, raise
`NotInWatchlistError` if it doesn't exist, otherwise delete it, commit, and
return `True`. Added a new `NotInWatchlistError` exception mirroring
`NotInCollectionError`.
**Tests:** Added two tests in `tests/test_watchlist.py` —
`test_remove_from_watchlist_removes_entry` (adds then removes a film, asserts
`True` and that the entry is gone from the DB) and
`test_remove_from_watchlist_not_present_raises` (removing an absent film raises
`NotInWatchlistError`).
**How I verified:** `pytest tests/ -v` — all tests pass.

### Visibility toggle (public parameter)
**What I did:** Added an optional `public=True` parameter to
`add_to_watchlist()` and passed it through to the `WatchlistEntry`. Updated the
`/watchlist/<user_id>/add` route to read `public` from the request body
(`data.get("public", True)`) so callers can set visibility explicitly. The
default remains `True`, so the change is backwards-compatible — existing callers
behave exactly as before.
**Connection to Comment 4:** This is the mechanism that lets a caller override
the default visibility that I discussed in Comment 4.
**Tests:** Added `test_add_to_watchlist_respects_public_false`, which passes
`public=False` and asserts the created entry is actually private. 
**How I verified:** `pytest tests/ -v` — all tests pass.

### Additional test — deduplication edge case
**What I did:** Added `test_add_to_watchlist_duplicate_raises` — adds a film,
asserts a second add of the same film raises `AlreadyInWatchlistError`, and
confirms exactly one entry exists (`count == 1`).
**Why I chose this case:**
Dedup is the feature's core guarantee, the highest-value untested path. It directly validates Comment 2 work
**How I verified:** `pytest tests/ -v` — all tests pass.

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->