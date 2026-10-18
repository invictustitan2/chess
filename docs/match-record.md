# Match record and audit table

## Game metadata

| Field | Value |
|---|---|
| Game ID | `game-001` |
| Variant | Standard chess |
| White | Human opponent |
| Black | ChatGPT |
| Result | In progress / not started |
| Start position | Standard initial FEN |
| Rules validator | Not yet identified |
| Analysis engine | Not yet identified |
| Record status | Foundation created; no moves recorded |

## Turn table

This table is append-only in spirit: do not replace a prior decision with a cleaner retrospective explanation. Add a correction event if the record needs repair.

| Ply | Side | Pre-move FEN | Proposed move (UCI / SAN) | Legality | Decision rationale at time | Engine evidence at time | Post-move FEN | Status / falsifier |
|---:|---|---|---|---|---|---|---|---|
| — | — | — | — | Not checked | No moves recorded | No engine run recorded | — | Game has not started |

## Event-level evidence

Each row should link to or include a full event containing:

- Exact pre-move FEN and prior move list.
- Move in UCI and SAN.
- Legal-move validation result and validator identity/version.
- Decision rationale written at decision time.
- Candidate alternatives considered, where available.
- Engine use before or after the choice, with raw output and settings.
- Exact post-move FEN and resulting game status.
- Material claims and their falsifiers.
- Any correction, with reason and timestamp.

## Corrections

Never silently alter an accepted ply. Add a correction record that identifies the old value, replacement value, reason, evidence, and the affected event. If the move was illegal, roll back the game state explicitly and preserve the rejected attempt in the audit log.

## Current status

No chess moves have been made in this record. White has the first move. Black is assigned to ChatGPT. The actual local dependencies and engine strength remain unverified.
