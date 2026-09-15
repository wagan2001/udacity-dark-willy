# Udacity Dark

Userstyles that darken Udacity. There are two in this repo, because Udacity rebuilt the site and the old one stopped working:

| File | Applies to | Status |
| --- | --- | --- |
| [`udacity-chatgpt-dark.user.css`](udacity-chatgpt-dark.user.css) | `www.udacity.com`, `learn.udacity.com` | Current |
| [`udacity-dark.css`](udacity-dark.css) | `classroom.udacity.com` | Obsolete — kept for reference |

## Install

These are userstyles, so they need a manager rather than a `link` tag:

1. Install one — [Stylus](https://add0n.com/stylus.html) for Chrome, Edge and Firefox, or [Cascadea](https://cascadea.app/) for Safari.
2. Open the manager's dashboard and choose **Import** (Stylus recognises `.user.css` files) or **Write new style**.
3. Paste in the file you want and save.

The styles carry their own `@-moz-document` scope, so they only apply to the Udacity hosts listed above.

## `udacity-chatgpt-dark.user.css` (current)

A dark theme for the site as it exists today, built on ChatGPT's dark palette.

Udacity's current front end is a Chakra UI app, which makes it unusual to theme. Every component gets an emotion class with a build-time hash (`.css-18fy3q3`) that changes on each deploy, so those are useless as selectors. What does work is that Chakra publishes its whole theme as CSS custom properties on `:root`, and Udacity's own components read them — roughly 1,000 `var(--chakra-*)` references on a single page. So the theme re-points 68 of those design tokens (semantic colours, the neutral ramps, the component hooks like `--card-bg` and `--popper-bg`) and then layers `!important` rules over Chakra's stable component classes for the surfaces that bypass tokens entirely.

### Palette

| Token | Value | Used for |
| --- | --- | --- |
| `--gpt-canvas` | `#212121` | page background |
| `--gpt-rail` | `#171717` | modal and drawer chrome |
| `--gpt-surface` | `#2f2f2f` | cards, list rows, link boxes |
| `--gpt-input` | `#303030` | fields and hover targets |
| `--gpt-well` | `#0d0d0d` | code blocks and editors |
| `--gpt-text` | `#ececec` | body copy |
| `--gpt-text-muted` | `#b4b4b4` | secondary copy |
| `--gpt-text-faint` | `#989898` | captions, placeholders |
| `--gpt-accent` | `#10a37f` | links, focus rings, selected states |

### What it deliberately leaves alone

- **`--chakra-colors-white` stays white.** Udacity uses that token for ink on its dark hero sections in 40 places *and* for the body background. Flipping it would blank out headings, so the canvas is painted explicitly instead.
- **`--chakra-colors-black` stays dark.** It carries body ink, but it is also the surface of the promo cards, where white text would disappear against a light background.
- **Mid-tone accents stay put.** Brand blue `#175CFF` and friends keep their identity, so program badges and links still read as Udacity. Only the near-white `-50`/`-100` tints are remapped, each to its own hue at 14–20% lightness, so alerts and chips stay recognisably coloured on a dark canvas.

### Status and caveats

Checked against the live site: the stylesheet parses cleanly, every token it overrides exists in Udacity's current theme, every class selector appears in the shipped CSS or markup, and every text/surface pairing clears WCAG AA (body copy 13.6:1, muted copy 6.5:1, links 5.0:1).

Not yet checked: a rendered pass on a signed-in page. The theme was derived from the public app shell plus the site's own stylesheets, so it is possible a panel somewhere still renders light. If that happens, the escape hatch comment at the bottom of the file has a single rule that neutralises every emotion-generated surface.

## `udacity-dark.css` (obsolete)

The original theme, written for `classroom.udacity.com`. That host now answers `301 Moved Permanently` to `learn.udacity.com`, and the classroom was rebuilt with entirely new class names — a different naming scheme, not just new hashes — so none of its selectors match anything any more. It is kept here as the historical version of the idea.

Where the current theme is token-driven, this one is a short list of hard-coded overrides against Udacity's old `index--some-thing--2MdcR` class names: content and footer panels, the code editor, secondary buttons, instructor notes, modal headers, and markdown body copy.

## Credit

Forked from [djwhatle/udacity-dark](https://github.com/djwhatle/udacity-dark) by Derek Whatley, who wrote the original classroom theme. His note on it still explains the goal: the themes he found online were too aggressive about darkening *everything*, so he made a restrained one instead. The ChatGPT-palette theme follows the same instinct — dark surfaces and light ink, with the page's own colours left in place.
