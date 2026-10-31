# Phonics Tetris

A Tetris variant where **every cell holds a phonics unit** rather than a single
letter or a plain block. Stack the pieces so that the letters across a row
spell a real English word — the row clears and you score.

Play: open `index.html` in a browser, or visit the GitHub Pages link.

## How it works

- The board is 10 x 20. The bottom **3 rows are grey bricks** — clear them all to win.
- Each tetromino is built from one of **13 shapes**. Every shape carries a set of
  *reading lines* (`grps`). Each line is filled with a phonics of the matching
  length, sharing letters with any line it crosses.
- Phonics are grouped by length:
  1. single letters — consonants + vowels
  2. digraphs and blends — `sh ch th wh ph kn wr gn bl br cl cr ...`
  3. three-letter phonics — `ack ail ain air ake ... ight` (short: `scr spr str spl squ thr shr`)
  4. four-letter phonics — `atch ight ound ouse`
- A row clears when a contiguous slice of **4 or more** cells spells a real word.
  Only rows within 1 of where your piece landed are checked.
- Slices are tested **longest first**, so a longer word always wins over a
  shorter one hiding inside it.

## Anti-repeat

The last **10 primary phonics** used are remembered (`lastphonicheads` in the
original). A new piece will not reuse a head phonics still inside that window —
candidates are tried in shuffled order, and only when the whole pool is already
recent does it fall back to the least-recently-used one.

## Controls

| Key | Action |
| --- | --- |
| `←` `→` / `a` `d` | move |
| `↓` / `s` | soft drop |
| `↑` / `w` | rotate |
| `Space` | hard drop |
| `P` | pause |

Touch buttons appear on small screens.

## Word checking

Words are verified against [dictionaryapi.dev](https://dictionaryapi.dev/). Results
are cached for the session. If the API is unreachable the row will not clear —
the game needs a connection to validate words.

## Credits

Merged from the Python prototype `vtt.py`:

- `main` branch — pygame GUI, 7 shapes, dictionary lookups near the landing
  zone, 3-row garbage start, scoring at `len(word) * 100`
- `design` branch — 13 extended shapes with per-shape letter groups so each
  piece carries genuine phonics

The web port fixes one bug from the prototype: when a reading line shared a
cell with another line, the later line used to overwrite it, corrupting the
line that was already built. Shared cells are now preserved and a line is only
accepted when its letter at the shared position actually matches.
