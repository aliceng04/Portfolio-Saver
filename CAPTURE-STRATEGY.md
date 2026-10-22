# Capture strategy

Decision, 2026-10-22: adopt the user-initiated capture model Readwise Reader uses (no platform
offers live OAuth sync of "saved/liked" items — confirmed, including for Readwise/Twitter, the
most API-open comparable case: [docs.readwise.io](https://docs.readwise.io/reader/docs/faqs/adding-new-content)).
See `README.md` for why "connect via API and auto-pull saved posts" doesn't work.

**Design rule, 2026-10-22 (updated — minimize user effort):** don't expose a menu of capture
channels and make the user pick one. **Exactly one path per platform**, chosen for the fewest
possible steps for that platform specifically. No channel is added just because a platform
technically supports it.

## The one path per platform

| Platform | The one path | Steps for the user | Why this one |
|---|---|---|---|
| **Substack** | **Auto-pull via RSS.** User adds a publication once (paste its name/URL) → every future post appears in Saver automatically, forever. | **1 step, one-time, per publication.** Zero steps per post after that. | Substack publishes a public RSS feed per publication — the only one of the four platforms where a true "set once, zero-touch forever" path exists. Chosen over email-forwarding because setting up a mail-client filter is itself a multi-step, technical, provider-specific task for a casual user — worse, not better. |
| **Facebook** | **Share → Saver**, from the native app or browser. | **1 tap per post** — replaces, does not add to, the tap they already do to hit "Save" today. | No API, no RSS, no email channel exists (see README.md). This is the floor; it cannot go lower given platform restrictions, not a product shortcoming. |
| **LinkedIn** | **Share → Saver** | **1 tap per post**, same as above | Same reasoning as Facebook. |
| **Threads** | **Share → Saver** | **1 tap per post**, same as above | Same reasoning as Facebook. |

Mechanism behind "Share → Saver": Saver registers as a native OS share target (iOS Share
Extension / Android Share Intent) and, for desktop, a one-click browser extension button. Both are
just the delivery mechanism for the same single action — there's no separate "channel decision"
exposed to the user; the app just works whichever way they're already holding their phone or
browser.

## Explicitly not building for v1

- Email forwarding, URL-save links, bookmarklets — real channels, but each is *an additional way
  to do the same thing*, which is exactly the "too many steps / too many choices" problem being
  avoided here. Revisit only if user research says people actually want a second path for a given
  platform.
- Passive DOM-listening for a platform's native Save button — fragile, closer to scraping.
- Any live OAuth "read my saved items" integration — not offered by any of the 4 platforms.

## Separate concern: historical backfill (still unsolved simply)

Old, already-saved posts aren't touched by any path above — they only start capturing from
whenever the user sets up Substack or installs the share/extension. Bringing in **years of past
saves** still has no one-step answer: the only route is each platform's personal-data-export
("Download your data") file, which the user must manually request from the platform, wait for,
then upload to Saver. This is inherently multi-step and slow (platform-side processing time) —
flagged here, not solved. Decide separately whether backfill is even in scope for v1, given it
can't be made simple.
