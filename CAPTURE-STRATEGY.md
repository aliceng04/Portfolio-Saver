# Capture strategy

Decision, 2026-10-22: no platform offers live OAuth sync of "saved/liked" items — confirmed,
including for Readwise/Twitter, the most API-open comparable case
([docs.readwise.io](https://docs.readwise.io/reader/docs/faqs/adding-new-content)). See
`README.md` for why "connect via API and auto-pull saved posts" doesn't work. Capture has to be
user-initiated, in some form, for every platform.

**Decision, 2026-10-22 (v1 — one mechanism only):** exactly **one** capture mechanism for **all
four** platforms — **Share → Saver**. No per-platform special case, no RSS auto-pull, no email
forwarding, no URL-save link. One thing to build, one thing the user learns, full stop.

An earlier version of this doc proposed RSS auto-pull for Substack specifically (true zero-touch
after a one-time setup). **Cut from v1 on purpose** — it would mean building and maintaining two
separate capture pipelines (an RSS ingester + a share-target handler) for a gain that only applies
to one of the four platforms. Revisit post-v1 once the single mechanism is proven, if Substack
users specifically ask for less-touch capture.

## The one mechanism

**Share → Saver**, everywhere:

| Surface | What the user does |
|---|---|
| Mobile (Facebook/LinkedIn/Threads/Substack apps) | View a post → tap the native **Share** button → pick **Saver** from the OS share sheet |
| Desktop/mobile web (any of the four sites) | View a post/article → click the **Saver browser-extension icon** |

Both surfaces feed the same backend action: capture whatever the OS/browser hands over (typically
a URL + whatever title/snippet the source app includes) and, where the URL is public, fetch the
full content server-side.

**This is strictly a one-tap action, same as the "Save" button the user already uses today on
every platform** — not an added step, a replacement one.

## What this means per platform

| Platform | What "Share → Saver" captures |
|---|---|
| Facebook | URL + snippet, if the post has a public URL; otherwise title/snippet only (see README's known limitation) |
| LinkedIn | Same caveat as Facebook |
| Threads | Same caveat as Facebook |
| Substack | URL always public and stable → Saver fetches the full article server-side; best-quality capture of the four, even without a dedicated path |

## Explicitly not building for v1

- RSS auto-pull (cut, see above).
- Email forwarding, URL-save links, bookmarklets.
- Passive DOM-listening for a platform's native Save button.
- Any live OAuth "read my saved items" integration.

## Separate concern: historical backfill (still unsolved simply)

Unchanged from before: old, already-saved posts aren't touched by Share → Saver — only new posts
from install onward. Bringing in past saves still has no one-step answer (platform data-export
file, manually requested and uploaded). Decide separately whether that's in scope for v1.
