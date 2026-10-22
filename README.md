# Saver

An app that pulls together the posts/articles you've **saved** across different social
platforms — Facebook, LinkedIn, Substack, Threads — into one place you can actually find things
in again.

## Problem

People save a lot of posts/articles across platforms but almost never go back and look at them,
or can't find a specific one again when they need it. Each platform's own "saved" list is a dead
end: no search across platforms, no tagging, no "what did I save about X".

## What this project is for

Aggregate saved items from multiple social platforms into one searchable, organized place.

## ⚠️ Known risk — read before building

None of the four target platforms offer a straightforward, official way for a third-party app to
read a user's **saved/bookmarked items**:

- **Facebook** — the Graph API does not expose a user's "Saved" collection to third-party apps;
  Meta deprecated most user-data read scopes for non-Meta apps years ago.
- **LinkedIn** — the public API is heavily restricted to approved partners (Marketing/Talent
  partners); there's no general "read my saved posts" scope available to an indie app.
- **Threads** — the official Threads API (via Meta) is young and currently focused on a user's
  *own* posts/insights, not their saved/liked items.
- **Substack** — no official public API at all; nothing to integrate against.

**This means the core premise — "connect to these apps and pull saved posts automatically" — is
likely not buildable as a clean API integration today**, at least not for all four platforms.
Realistic alternatives worth considering before committing to an architecture:

1. A **browser extension / share-sheet** that captures an item the moment the user saves it
   (user-initiated, no scraping, no ToS risk) instead of pulling historical "saved" lists after
   the fact.
2. **Manual/semi-manual import** (share a link into Saver, paste a URL, forward an email) as the
   v1 capture mechanism, with "auto-pull from platform X" treated as a stretch goal per platform,
   revisited once/if that platform's API allows it.
3. Scraping a platform's saved-items page — technically possible for some platforms but against
   their Terms of Service and fragile; not recommended as a foundation.

Decide this before writing more than a prototype — it changes the whole shape of the app (whether
it's an integrations product or a capture-and-organize product).

## Status

Just scaffolded. No code yet.
