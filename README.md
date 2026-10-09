# x-viral-monitor

Private archive for the Grok Bot skill **X 博主爆款帖监测** and its hourly scan data.

## Layout

| Path | What |
| --- | --- |
| `skill/SKILL.md` | Reusable scan recipe (accounts / threshold come from the routine) |
| `data/state/state.json` | Latest watch state: account ids, view snapshots, already-pushed post ids |
| `data/hourly/YYYY-MM-DD/HHMM.md` | One file per hourly run (Beijing time), human-readable summary |
| `data/hourly/YYYY-MM-DD/HHMM.json` | Same run as structured JSON |

## Accounts (as of last update)

Configured in the routine + `data/state/state.json`. Default threshold: **>5000 views/hour**. Window: daily **07:04–23:04** Asia/Shanghai.

## Notes

- Hourly files are written by the Grok Bot routine after each scan.
- `pushed` posts are never re-alerted; historical hourly files still keep the metrics.
