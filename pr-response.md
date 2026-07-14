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
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->