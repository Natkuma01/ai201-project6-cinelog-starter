## AI Usage
- Use AI to give me suggestion on commit message
- Use AI to search a function and rename it, make sure after rename the project still run properly
- Ask AI for improvement/refactor plan


## Comment 1 — Rename
**What I did:**
- Renamed `save_to_watchlist()` to `add_to_watchlist()` in `services/watchlist_service.py` to match the project's verb_to_noun convention.
- Updated all references and call sites across the application, including the blueprint routing.
**How I verified:**
- Verified that all imports and references use the new method name `add_to_watchlist()`.
- Verified that existing tests and local server still run properly.

## Comment 2 — Deduplication
**What I did:**
- Added deduplication logic to `add_to_watchlist()` in `services/watchlist_service.py` following the pattern from `add_to_collection()`.
- Introduced `AlreadyInWatchlistError` exception which is raised if the film is already on the user's watchlist.
- Caught `AlreadyInWatchlistError` (returning 409) and `FilmNotFoundError` (returning 404) in `routes/watchlist/watchlist.py`.
**How I verified:**
- Verified that trying to add a duplicate film raises `AlreadyInWatchlistError`.
- Verified that the `/watchlist/<user_id>/add` route returns a 409 error when trying to add a film already on the watchlist.

## Comment 3 — Missing test
**What I did:**
- Created a new test file `tests/test_watchlist.py`.
- Wrote a test called `test_add_to_watchlist_nonexistent_film_raises` to verify that adding a nonexistent film raises a `FilmNotFoundError`.
- Followed the same fixtures and assertion structure as `test_add_to_collection_nonexistent_film_raises` in `tests/test_collection.py`.
**How I verified:**
- Ran the command `pytest tests/test_watchlist.py -v` and confirmed that the test successfully passed.


## Comment 4 — Default visibility
**My position:**
- Keep the `public=True` default setting for the watchlist.

**Reasoning:**
- We are optimizing for social sharing and community interaction, which are core behaviors for a community film tracking app like CineLog.
- It provides a frictionless experience for users to share their film lists with friends immediately.
- It encourages discovery within the community as users can browse each other's lists by default.

**Tradeoff acknowledged:**
- The tradeoff is that user privacy is not protected by default; users who want a private watchlist must manually toggle the visibility setting.
- The alternative (`public=False` by default) would protect user privacy first but would increase friction when sharing (e.g., users getting access errors when sharing links) and decrease list discovery across the community.


## Comment 5 — Sort order
**My position:**
- I agree with the feedback and will change the watchlist sorting to sort by date added (newest first).

**Reasoning:**
- We are optimizing for users who want to see their most recently added films at the top of their watchlist right away.
- Placing new items at the top makes the app feel dynamic and active rather than static.
- It reduces the effort required to check or manage the films the user has recently shown interest in.

**Engagement with reviewer's point:**
- I acknowledge that alphabetical sorting makes finding a specific, known movie title in a long list easier.
- However, alphabetical sorting makes a growing list feel static, and users who need to find a specific title can still use the browser's search feature (Ctrl+F).

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description