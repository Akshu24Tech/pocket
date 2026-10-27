# 👝 Pocket

Named side spaces inside a chat, each with its own memory.

You're deep in a project and a side topic shows up: a movie list, a random idea, a quick question. If you drop it in the main chat, the project context gets messy. Pocket gives each side topic its own room. You hop in, do the thing, hop out, and the main project stays clean. Next time you open that pocket, it remembers where you left off.

## Commands

Two commands. That's it.

| Command | What it does |
|---|---|
| `/pocket <name>` | Open the pocket. Resumes it if it exists, creates it if not |
| `/back` | Saves the current pocket and returns to the main project |

Everything else is plain language:

- "what pockets do I have?" lists them, one line each
- "carry that into the project" brings over just that one item
- "delete the movies pocket" removes it

## Example

```
You:    /pocket ideas
Claude: 👝 ideas

You:    Help me brainstorm names for my side project. Keep it casual.
Claude: 👝 ideas
        [brainstorm]

You:    /back
Claude: ✅ Back in main. ideas saved: side project name brainstorm

... later ...

You:    /pocket ideas
Claude: 👝 ideas
        Left off: shortlisted three names, need to pick one.
```

## How it works

Each pocket has a small card (under about 150 words) holding its goal, key facts, decisions, latest output, where you left off, and the next step. Opening a pocket loads only that card. Nothing from the main project or other pockets comes along unless you explicitly share it.

- With file tools, cards are saved at `.pockets/<name>.md`
- Without file tools, Claude prints the card on `/back` and you paste it back when you reopen the pocket in a new chat

## Install

1. Download `pocket.skill` from the [Releases](../../releases) page (or from this repo).
2. In Claude, go to the skills settings and upload the file.
3. Type `/pocket test` in any chat.

You can also copy `pocket/SKILL.md` into your own skills folder.

## Honest limit

In a normal chat, earlier messages still exist in the model's context, so isolation works by instruction. It is reliable, but not a hard guarantee. A strict version would be an app that builds the model's context from only the active pocket's card.

## Quick tests

1. **Leak:** in main say "Codename is BLUEJAY." Open `/pocket test` and ask for the codename. It should not know.
2. **Resume:** in `/pocket ideas`, agree a style and write a short draft. `/back`. Later `/pocket ideas`. It should say where you left off and still have the style and draft.
3. **Isolation:** after `/back`, ask "what were we discussing?" It should answer with main only.

## License

MIT
