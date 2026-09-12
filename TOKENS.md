# Token usage tracking — Cantus

LLM token usage for this project, tallied session by session.

## Cumulative tally (2026-09-13)

| Metric | deepseek-v4-flash | deepseek-v4-pro | **Total** |
|---|---|---|---|
| Dev sessions (Hermes) | 2 | 1 | **3** |
| Scripted agent sessions (API) | 0 | 0 | **0** |
| Messages | 1 375 | 454 | **1 829** |
| API calls | 4 941 | 229 | **5 170** |
| Input tokens | 9 858 278 | 344 314 | **10 202 592** |
| Output tokens | 5 104 439 | 110 536 | **5 214 975** |
| **Subtotal (input + output)** | **14 962 717** | **454 850** | **15 417 567** |
| Cache read (reused at reduced price) | 1 389 711 616 | 31 611 264 | **1 421 322 880** |
| **Estimated cost** | **≈ 6.32 USD** | **≈ 0.36 USD** | **≈ 6.68 USD** |

## How to re-read the counter

The Hermes session database (SQLite) holds the exact counters
(per session × model, in `session_model_usage`, joined on the sessions
whose working directory is this project):

```bash
sqlite3 ~/.hermes/state.db "SELECT u.session_id, u.model,
  SUM(u.input_tokens), SUM(u.output_tokens), SUM(u.cache_read_tokens),
  SUM(u.api_call_count), SUM(u.estimated_cost_usd)
  FROM session_model_usage u JOIN sessions s ON s.id = u.session_id
  WHERE s.cwd LIKE '%MusicPlayer%'
  GROUP BY u.session_id, u.model;"
```

After each dev session, update the table above from a fresh query.

## Notes

- Tally taken from `~/.hermes/state.db` (table `session_model_usage`,
  filtered by session id) — real runtime counters, not an estimate.
- « Scripted agent sessions (API) » = `api-*` sessions driven by scripts
  (audits, releases, background tasks) attached to this project.
- `reasoning_tokens` is probably included in `output_tokens`
  (to be confirmed with the provider).
- Tally updated on 2026-09-13 — **2026.09.100: renamed MusicPlayer →
  Cantus** (new name, new logo, full rebrand). This tally includes the
  final flush of the 100-c1 → c8 sessions; the rename session itself
  will be added at the next tally.
