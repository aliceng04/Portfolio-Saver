# Capture strategy

Decision, 2026-10-22: adopt the **Readwise Reader model** — multi-channel, always user-initiated
capture. No live OAuth sync of "saved/liked" items from any platform; confirmed infeasible even
for Readwise/Twitter (the most API-open of the comparable platforms), per
[Readwise Reader's own docs](https://docs.readwise.io/reader/docs/faqs/adding-new-content). See
`README.md` for why the naive "connect via API and auto-pull saved posts" premise doesn't work.

## Channels

| Channel | How it works | Trigger |
|---|---|---|
| Browser extension | User views a post/article → clicks the Saver extension icon → extension reads the **rendered page content** (not just the URL) → sends to Saver | User-initiated |
| Mobile share sheet | User taps native Share inside the source app → picks Saver from the OS share sheet | User-initiated |
| Email forwarding | Each Saver account gets a unique inbound email address; user forwards (or auto-forwards) an email to it → Saver parses the HTML into a clean article | User-initiated (or a one-time mail-rule setup) |
| RSS | User subscribes to a public feed inside Saver → new posts appear automatically | Passive, after one-time subscribe |
| URL-based save link | `https://saver.app/save?url=<url>` — Saver fetches + parses server-side | User-initiated (bookmarklet, Shortcuts, Zapier/IFTTT) |

## Per-platform channel map

| Platform | Extension | Share sheet | Email forward | RSS | URL-save link |
|---|---|---|---|---|---|
| Facebook | ✅ (web) | ✅ (app) | — | — | ⚠️ only if the post has a public URL (many don't) |
| LinkedIn | ✅ (web) | ✅ (app) | — | — | ⚠️ same caveat as Facebook |
| Threads | ✅ (web, limited) | ✅ (app) | — | — | ⚠️ same caveat |
| **Substack** | ✅ (web) | ✅ (app) | **✅ — best channel**, Substack is email-native | ✅ public feed per publication | ✅ — posts have stable public URLs |

Substack is the easiest of the four precisely because it's the only one that's email-native and
has stable public URLs + public RSS. Facebook/LinkedIn/Threads are realistically
extension-or-share-sheet only — same limitation Readwise itself has with Twitter/X.

## Separate concern: historical backfill

None of the above channels retroactively pull in years of already-saved posts. For that: each
platform's own personal-data-export feature ("Download your data") produces a file the user can
import into Saver once. Not live, not automatic, but solves the cold-start problem independently
of the ongoing-capture channels above.

## Not doing (for now)

- Passive DOM-listening for the platform's own native Save/Bookmark button (fragile, breaks on
  every UI change, closer to scraping). The explicit "Saver button/share" action is the
  intentional choice instead.
- Any live OAuth "read my saved items" integration — confirmed not offered by any of the 4
  platforms (see README.md).
