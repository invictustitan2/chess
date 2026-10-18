# Local dependency audit

The local machine cannot be inspected through this repository connection. Do not assume a dependency is installed because it was mentioned in conversation. Run the checks below on the machine that will execute the match and paste the output into the record.

## 1. Identify available commands

Linux/macOS shell:

```sh
command -v stockfish || true
stockfish --version 2>&1 | head -n 5
command -v python3 || true
python3 --version
python3 -m pip show chess 2>/dev/null || true
command -v cargo || true
cargo --version 2>/dev/null || true
```

A failed command is evidence only that the command was not found in the current PATH or that invocation failed; it does not prove the software is absent from every environment.

## 2. Verify python-chess import and rules behavior

```sh
python3 - <<'PY'
try:
    import chess
    print("python-chess version:", getattr(chess, "__version__", "unknown"))
    board = chess.Board()
    print("initial FEN:", board.fen())
    print("legal move count:", board.legal_moves.count())
    print("e2e4 legal:", chess.Move.from_uci("e2e4") in board.legal_moves)
except Exception as exc:
    print(type(exc).__name__ + ":", exc)
PY
```

Expected standard-start checks, if python-chess is installed and functioning: initial FEN corresponds to the standard starting position, 20 legal moves, and e2e4 is legal. A different result requires investigation before play. This is a smoke test, not a complete rules test.

## 3. Verify a UCI engine handshake

If `stockfish` is found, test the Universal Chess Interface:

```sh
printf 'uci\nisready\nposition startpos\ngo depth 8\nquit\n' | stockfish
```

Some builds may terminate before returning a complete search result when `quit` follows `go` immediately. For a proper test, use a UCI client that waits for `bestmove`, or interactively send `uci`, wait for `uciok`, send `isready`, wait for `readyok`, then search and wait for `bestmove`.

Record engine name/version, binary path, architecture, and options. Do not infer the version from a package name alone.

## 4. Minimum acceptance criteria

- A rules implementation accepts/rejects legal moves from FEN and detects checkmate and draws.
- A UCI engine completes a search and emits a `bestmove`.
- The event recorder captures pre/post FEN and move notation.
- Engine evidence is kept separate from the player's stated choice.
- A test position with a known legal-move count and a known checkmate position passes.
- The exact commands, versions, stdout/stderr, and failures are preserved.

## Security and reproducibility

Do not run an unknown downloaded binary merely because it claims to be a chess engine. Prefer a trusted package source or verify release checksums/signatures. Preserve the exact engine version and settings used for each recorded decision.
