# Design directions for sam256.com

Three alternative front pages, plus the reasoning behind them.
Open `designs/index.html` for the visual side-by-side.

Nothing here replaces the live `index.html`. Each direction is a complete,
self-contained page — pick one and it becomes the new front page.

| File | Direction | In one line |
| --- | --- | --- |
| [`signal.html`](signal.html) | **A · Signal** | The current site, kept honest. Same voice, real structure. |
| [`timechain.html`](timechain.html) | **B · Timechain** | The live block height becomes the hero. |
| [`ledger.html`](ledger.html) | **C · Ledger** | Paper instead of phosphor — a printed index. |

---

## What's wrong with the page today

The voice is genuinely good: CRT teal, monospace, a live chain tip, a quote,
and ten links. That's worth keeping. These are the things working against it.

1. **The links are the product, but they read as a footnote.** Ten uniform
   uppercase labels wrapped into one row, all ranked equally. A first-time
   visitor can't tell that *Dice Roll* is an interactive tool and *Merch* is a
   shop.

2. **The descriptions hide behind `:hover`.** Each link carries a genuinely
   useful `data-hint`, rendered through `a:hover::after` only — so it never
   appears on a phone, and never for a keyboard user.

3. **The layout can't scroll.** `overflow: hidden` on `body`, plus a `.links`
   row pinned with `position: absolute; bottom: 28px`, means that on a short
   window or a landscape phone the links are clipped with no way to reach them.

4. **Contrast falls below WCAG AA.** Measured against the `#0D3643` ground:

   | Element | Measured | AA floor |
   | --- | --- | --- |
   | Link text `rgba(181,181,181,.68)` | 3.73:1 | 4.5:1 |
   | Hint line `rgba(181,181,181,.45)` | 2.44:1 | 4.5:1 |
   | Telemetry `--muted #6d6d6d` | 2.50:1 | 4.5:1 |

   The hint line is also set at `.55rem` — about 9px.

5. **Focus styles are removed.** `outline: none` on every `a` with nothing put
   back, so keyboard navigation is invisible.

6. **The overlay isn't a real dialog.** No focus trap, and it stays in the tab
   order while closed, so keyboard users tab into a panel they can't see.

7. **Nothing to share.** No meta description, no Open Graph tags, no favicon.
   Posting the URL produces a blank card titled "sam256".

8. **The best element on the page is set in 11px grey.** The live block height
   is the one thing proving this site is plugged into a running chain.

9. **Small robustness gaps.** The `fetch` has no timeout and keeps polling in a
   background tab; the <kbd>S</kbd> shortcut fires while typing anywhere on the
   page; `prefers-reduced-motion` is honoured in CSS while the glitch timer
   keeps running in JS.

---

## A · Signal — recommended

The current site with the sharp edges filed off. Lowest risk of the three.

- **Keeps** the palette, scanlines, drifting starfield, blinking caret, the
  micro-glitch, and both <kbd>?</kbd> and <kbd>S</kbd> easter eggs.
- **Restructures** the ten links into Tools / Writing / Artifacts, as cards that
  carry their description in the open — the `data-hint` text finally visible on
  touch.
- **Fixes** every item above: the page scrolls, all colour passes AA, focus
  rings are back, and the overlay is a native `<dialog>` with a real focus trap.
- **Adds** "last block *N*m ago", a chain-tip staleness indicator, a skip link,
  and a silence mode remembered in `localStorage`.

## B · Timechain

Promotes the chain tip from telemetry to hero.

- The tip height fills the screen, with blocks-to-halving computed live.
- A scrollable strip of the last eight blocks — age and transaction count each,
  linking to mempool.space.
- Tools become a numbered index; Bitcoin orange plays against the teal.
- Degrades in two steps: full block data → tip-height endpoint → offline state.

## C · Ledger

The deliberate opposite, worth seeing before deciding.

- Paper instead of phosphor: a serif index in three numbered parts, with a
  drop-capped epigraph and one live element — the chain-tip stamp.
- Follows the reader's light/dark preference, and prints cleanly.
- Best suited to the book and the newsletter; highest contrast of the three.

---

## Shared technical notes

Everything below applies to all three, and none of it exists today.

- Single self-contained HTML file, no build step, no dependencies — matching how
  the site is built now.
- Inline SVG favicon, meta description, Open Graph and `theme-color` tags.
- `fetch` guarded by an `AbortController` timeout, paused while the tab is
  hidden, and refreshed on `visibilitychange`.
- Fluid type via `clamp()`; `env(safe-area-inset-bottom)` respected on notched
  phones.
- `prefers-reduced-motion` honoured in both CSS and JS.
- Every colour pair measured against WCAG AA. Every `target="_blank"` carries
  `rel="noopener"`. All eleven original destinations preserved verbatim.

Screenshots in `../assets/` were rendered at 1440×900 and 390×844 with the chain
API stubbed at block 913,442.
