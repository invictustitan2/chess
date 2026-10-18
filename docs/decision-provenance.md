# Decision provenance protocol

## Purpose

Preserve the distinction between (1) the position and facts available before a move, (2) the player's choice, (3) optional engine evidence, and (4) the result after the move. The record must be auditable without relying on a later explanation.

## Required sequence for every move

1. **Freeze the pre-move state.** Record ply number, side to move, full FEN, move history, and the exact legal-move set from the rules implementation.
2. **State the decision before validation feedback where practical.** Record the selected move in UCI and SAN, the principal reason, candidate alternatives considered, and any uncertainty. If engine output was already consulted, disclose that before presenting the choice.
3. **Validate legality.** Apply the move with a rules-compliant implementation. If it is illegal, reject it and preserve the failed attempt as an event; do not mutate the game state.
4. **Record engine evidence separately.** If used, record engine identity/version, binary or package source, UCI options, hash/thread settings, time/depth/nodes, score with side-to-move perspective, and principal variation. Save raw output when available.
5. **Apply and verify.** Record post-move FEN, SAN, check/checkmate/draw flags, and resulting legal moves. The next side must match the position state.
6. **Commit the record.** Append an immutable event or make a clearly identified correction entry. Do not rewrite history silently.

## Evidence labels

- `FACT`: directly supported by the recorded position, move history, tool output, or cited source.
- `CHOICE`: the move selected by the player; not an engine fact.
- `ENGINE`: output actually returned by a named engine under recorded settings.
- `HYPOTHESIS`: a strategic or tactical explanation that has not been proven.
- `UNKNOWN`: information not measured or not available.
- `CORRECTION`: an explicit repair to a prior record, including the reason and affected event.

Do not label an engine evaluation as a proof of a forced win. A score is an engine assessment under its search and settings. A claim of checkmate must be confirmed by the rules state; a claim of forced mate requires a stated search result or a verifiable line, with its scope and limitations.

## Falsifiability

Every material strategic claim must include what would falsify it. Examples:

- “The move is legal” is falsified if the rules implementation rejects it from the recorded pre-move FEN.
- “This move gives checkmate” is falsified if the post-move position is not checkmate under legal-move generation.
- “The engine preferred this move” is falsified if the cited raw output/settings do not show that preference.
- “The move wins material” is falsified by a legal continuation that avoids the claimed gain, unless the claim specifies a forced line and evidence for it.

## Independence and timing

A post-move engine check cannot be represented as pre-move evidence. If an engine was consulted before choosing, record that fact. If the choice was made without engine assistance, record `engine_consulted_before_choice: false`; do not later imply the choice was engine-independent if that is untrue.

## Suggested event schema

```json
{
  "game_id": "game-001",
  "event_id": "ply-001",
  "ply": 1,
  "side_to_move": "white",
  "pre_fen": "rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1",
  "legal_move_check": {
    "implementation": "UNKNOWN",
    "result": "NOT_RUN"
  },
  "decision": {
    "uci": "e2e4",
    "san": "e4",
    "reason": "Example only; not a recorded game decision.",
    "alternatives": [],
    "engine_consulted_before_choice": false,
    "uncertainties": []
  },
  "engine_evidence": [],
  "post_fen": null,
  "post_move_status": "NOT_APPLIED",
  "claims": []
}
```

The example move is illustrative only and is not an actual move in the match. Replace implementation and result fields only with observed facts.
