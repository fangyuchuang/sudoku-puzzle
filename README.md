# Sudoku

A self-contained Sudoku game with a backtracking **generator** and **solver**. Every puzzle it deals is guaranteed to have **exactly one solution**.

One HTML file — no build step, no dependencies, no network calls.

**[Play the demo in your browser →](https://fangyuchuang.github.io/sudoku-puzzle/)**

## Play

- Open `index.html` in any modern browser, or serve the folder with `python3 -m http.server`.
- Tap a cell, then press `1`–`9`. `0` / `Backspace` erases. Arrow keys move around the grid.
- **Hint** fills one cell, **Check** reports how many cells are wrong, **Solve it** reveals the answer.
- Three difficulties (Easy / Medium / Hard) control how many cells are dug out.

Conflicting cells are highlighted as you type, so you catch mistakes immediately instead of at the end.

## How the puzzles are generated

The interesting part is not the UI — it's generating a puzzle that is *uniquely* solvable without brute-forcing forever.

**1. Build a full solved grid.** Walk the empty board in order; at each cell try the digits 1–9 in random order, keep the first that doesn't break the row/column/box constraint, and recurse. Randomising the digit order is what makes every generated grid different.

**2. Dig holes, one at a time.** Shuffle all 81 positions, then for each one: blank it out and count how many solutions the board now has. If the count is still exactly 1, leave it blank; otherwise put the digit back.

That second step is the whole trick. A naive generator that just removes N random cells produces ambiguous puzzles, which are unsatisfying to solve and impossible to check. Counting solutions on every removal is what prevents that.

**3. Stop counting early.** `countSolutions(board, limit)` aborts as soon as it finds `limit` solutions. Since we only care whether the answer is 1 or "more than 1", `limit = 2` is enough — no need to enumerate them all.

```
digging cost  = 81 removals × (solve until 2 solutions found)
observed      = 2–50 ms per puzzle on a laptop
```

## Verifying it

Generators are easy to get subtly wrong, so the core is covered by tests:

- every generated solution is a valid completed grid (all rows, columns and boxes contain 1–9)
- every generated puzzle has **exactly one** solution
- every clue agrees with the stored solution
- the solver can always recover the solution from the clues alone
- a blank board reports more than one solution (proves the counter isn't trivially returning 1)

## Project structure

```
index.html    game UI + generator + solver, no dependencies
```

## More puzzles

- [Sudoku](https://iqiqgame.com/play/play-sudoku-online-free-game) — the online version, with pencil marks and hints.
- [Killer Sudoku](https://iqiqgame.com/play/play-killer-sudoku-online-free) — sudoku plus arithmetic cages.
- [Arrow Sudoku](https://iqiqgame.com/play/play-arrow-sudoku-online-free-game) — arrows constrain the digits along their path.
- [Sudoku rules and beginner techniques](https://iqiqgame.com/blog/how-to-play-sudoku-rules-beginner-techniques) — the patterns worth learning first.
- [More free brain and puzzle games](https://iqiqgame.com/) — 24 points, 2048, lights out, and other logic puzzles.

## License

MIT.
