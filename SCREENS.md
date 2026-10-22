# Screens — v1

Screen plan for the first Figma pass, drawn against the single capture mechanism decided in
`CAPTURE-STRATEGY.md` (Share → Saver, same action on every platform). Mobile-first — phone frame
390×844 — since the share sheet is primarily a mobile mechanism.

| Screen | What the user does here | Elements (top to bottom) | States |
|---|---|---|---|
| **Onboarding — How to save** | Learns the one way to save: Share → Saver | Illustration of the Share action, short explainer text, "Got it" button | default |
| **My Saves (home)** | Browses saved items, searches to find an old one again | Search bar (top), list of cards — thumbnail + title + source platform + saved date | default · empty (nothing saved yet) · loading |
| **Item Detail** | Reads the full content of a saved item | Title, source/platform, full content, "Open original" button | default · error (content couldn't be fetched) |

## Open question

Anything to add or cut before drawing — e.g. a filter-by-platform control on My Saves, a
Settings/Account screen? Not yet decided; listed here as the pending question rather than assumed.

## Status

Not drawn yet — blocked on the Figma plugin connection (Figma Desktop → Reqwise plugin panel open)
and on confirming/adjusting this screen list.
