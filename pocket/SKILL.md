---
name: pocket
description: Named side spaces ("pockets") inside a chat, each with its own persistent memory, so the user can switch topics without polluting the main project conversation and later resume any pocket without re-explaining. Use whenever the user types /pocket or /back, or says things like "open a pocket for X", "switch to my linkedin pocket", "park this", "side topic", "go back to the project", or wants a topic to keep its own separate memory.
---

# Pocket

Each pocket is a separate room with its own memory. The user hops between pockets and
the main project. Nothing leaks between them, and every pocket remembers where it
was left.

## Two commands. That's all.

| Command | What it does |
|---|---|
| `/pocket <name>` | Open the pocket. If it exists, resume it. If not, create it |
| `/back` | Save the current pocket and return to the main project |

Everything else is plain language. Handle requests like these without needing a command:
- "what pockets do I have?" -> list pockets, one line each: name and where it was left
- "carry that into the project" -> bring over only that one item
- "delete the movies pocket" -> remove that pocket and its card

The name can be anything (`linkedin`, `movies`, `ideas`). The main project is the
default and needs no name.

## The pocket card (each pocket's memory)

Keep each card under about 150 words, in this shape:

```
# pocket: <name>
Goal: <one line>
Key facts: <3-6 short bullets, incl. user's preferences for this pocket>
Decisions: <what was settled>
Latest output: <the most recent draft/result, kept in full if short>
Left off: <where we stopped>
Next: <next small step>
```

Rewrite the card on leaving a pocket: merge and trim, do not just append.

## Where cards are stored

- With file tools: save each card at `.pockets/<name>.md`, read it when the pocket opens.
- Without file tools: keep cards in the chat, and on `/back` print the updated
  card in a small block with "Pocket card saved. Paste it when you reopen this pocket
  in a new chat." Be clear it won't persist across chats on its own.

## Opening a pocket

1. Load only that pocket's card. Nothing from the main project or other pockets.
2. First line: `👝 <name>`. If resuming, add: "Left off: <left-off line>".
3. If the card has a `Shares:` line (e.g. `Shares: voice guide`), the pocket may read
   that one named item from main. Default is nothing.

## Inside a pocket

- Start every reply with the one-line banner `👝 <name>`.
- Use only the pocket's card and what is said inside the pocket. Do not use or hint at
  the main project or other pockets.
- If the user mixes in project details, offer once: "That belongs to the main
  project. Switch back?"

## On /back

1. Update the pocket's card.
2. Reply with one line: `✅ Back in main. <name> saved: <3-8 word summary>`
3. Resume main from the last message before the pocket opened. Treat pocket content as
   out of bounds unless the user asks to carry something over.

Typing `/pocket <other>` from inside a pocket saves the current card first, then opens
the other one.

## Gentle auto-suggest

If a message is clearly off-project, ask once: "That looks off-project. Put it in
a pocket?" If the user declines or ignores it, don't ask again for that topic. Never
open a pocket without the user agreeing.

## Keep it simple

- One pocket open at a time.
- `/back` with no pocket open: say "You're already in main."
- Banners stay one line. No long explanations unless asked.

## Honest limit (say only if asked how isolated it is)

In a normal chat, earlier messages still exist in the model's context, so this
works by instruction: reliable, not a hard guarantee. A strict version is an app
that builds the model's context from only the active pocket's card.

## Tests

1. Leak: in main say "Codename is BLUEJAY." Open `/pocket test`, ask for the codename. Pass: it doesn't know.
2. Resume: in `/pocket linkedin` agree a style and draft a post. `/back`. Later `/pocket linkedin`. Pass: it says where you left off and still has the style and draft.
3. Isolation: after `/back`, ask "what were we discussing?" Pass: main only.
