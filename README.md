# Chess: decision-provenance match

This repository records a chess match as a reproducible sequence of positions and decisions, not merely a move list.

## Match contract

- **Sides:** ChatGPT plays Black; the human opponent plays White.
- **Ruleset:** Standard chess (FIDE-style rules, standard 8×8 initial position), unless both players explicitly agree to a variant before the game.
- **Initial position:** Standard starting position, FEN:
  `rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1`
- **Move authority:** A move is accepted only after legality is checked against the current position. SAN is for display; UCI is recorded as an unambiguous move identifier.
- **Engine separation:** Engine output is evidence, not authorship. Record engine/version, settings, time/depth, score perspective, principal variation, and whether the engine was consulted before or after the decision. Never describe an engine suggestion as an independently generated choice.
- **No fabricated verification:** If no engine was actually run, record that explicitly. Do not claim a move is best, forced, or mate without supporting analysis.
- **Position integrity:** Every ply records the pre-move FEN, move, post-move FEN, and legality result. The move history is authoritative; a rendered board is a view of that state.
- **Decision provenance:** Record candidate moves, the selected move, reasons, known uncertainties, engine evidence, and what observation would falsify each material claim.
- **Fair play:** No hidden moves, retroactive edits to a decision record, or changing the position without an explicit correction entry.

## Starting board

```text
    a b c d e f g h
 8  r n b q k b n r  8
 7  p p p p p p p p  7
 6  . . . . . . . .  6
 5  . . . . . . . .  5
 4  . . . . . . . .  4
 3  . . . . . . . .  3
 2  P P P P P P P P  2
 1  R N B Q K B N R  1
    a b c d e f g h
```

Uppercase pieces are White; lowercase pieces are Black. White moves first.

## Documents

- [Decision provenance and turn protocol](docs/decision-provenance.md)
- [Match record and audit table](docs/match-record.md)
- [Local dependency audit](docs/dependency-audit.md)

## Scope and limitations

This repository is a record and protocol, not proof that a chess engine or local dependency is available. Local tools must be checked on the machine where the match is being run. The repository was found without readable files at setup time; this README establishes the initial project contract.
