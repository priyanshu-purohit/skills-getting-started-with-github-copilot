## Plan: FastAPI Backend Tests

Add a focused pytest suite under a new top-level `tests/` directory. Exercise the FastAPI app through its in-process `TestClient`, covering current activity, signup, duplicate-registration, and unregister behavior without launching a server. Structure every test with clearly labeled Arrange, Act, and Assert sections. Isolate tests by restoring the module-level in-memory `activities` fixture state after each case.

**Steps**
1. Add `pytest` to `requirements.txt`; retain existing `httpx` dependency for the test client.
2. Add `tests/test_app.py` with a shared fixture that snapshots and restores `src.app.activities`, plus a `TestClient` fixture or local client setup.
3. Write each test using explicit `# Arrange`, `# Act`, and `# Assert` sections:
   - `GET /activities` returns the activity data.
   - Successful signup returns the expected response and adds the participant.
   - Duplicate signup returns `400` without changing participants.
   - Signup for an unknown activity returns `404`.
   - Successful unregister returns the expected response and removes the participant.
   - Unregistering a missing participant returns `404`.
   - Unregistering from an unknown activity returns `404`.
4. Keep the existing `pytest.ini` root `pythonpath` setting unless test collection demonstrates a need to adjust it. Do not add CI workflow changes in this scope.
5. Run `python -m pytest` from repository root and confirm repeat runs pass, demonstrating fixture isolation.

**Relevant files**
- `/workspaces/skills-getting-started-with-github-copilot/requirements.txt` — add the pytest test dependency; `httpx` is already listed.
- `/workspaces/skills-getting-started-with-github-copilot/tests/test_app.py` — new API-level tests and isolated in-memory state fixture.
- `/workspaces/skills-getting-started-with-github-copilot/pytest.ini` — existing root import path; inspect/adjust only if needed for collection.

**Verification**
1. Install declared dependencies as needed, then run `python -m pytest` from the repository root.
2. Run the test suite twice to catch state leaking across test cases or runs.

**Decisions**
- Use pytest with FastAPI's `TestClient`; no external server or frontend tests.
- Make AAA explicit in every test with labeled sections, asserting both response status/body and relevant state changes.
- Leave CI integration and unrelated cleanup out of scope.
