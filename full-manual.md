# Troubleshooting, Support and License

[← Back to documentation index](../README.md)

## 25. Troubleshooting

### My NPC has no dialog

Check:

- The NPC's Dialog ID exactly matches the JSON key.
- You edited the correct locale file.
- The JSON is valid.
- The file is in `config/kantonpcs/dialogs/`.
- You restarted/reloaded after editing external files.

### My PT-BR dialog is not showing

Check:

```text
config/kantonpcs/dialogs/pt_br_arrays.json
```

Also make sure the player's Minecraft language is set to Portuguese if you expect PT-BR text.

### The NPC uses English instead of my language

Unsupported languages fall back to English. For now, use:

```text
en_us
pt_br
```

### The skin does not appear

Check:

- The skin is a `.png` file.
- The file is inside `config/kantonpcs/skins/`.
- You clicked Refresh in the Change Model screen.
- The selected model type matches the skin: Wide or Slim.

### The shop item does not appear correctly

Check:

- Item IDs are correct.
- Use namespace when possible: `minecraft:diamond`, `cobblemon:potion`.
- For Pokémon products, use a valid Cobblemon species ID.
- For item-trade cost, check commas and pipes.

### The reward command does not work

Check:

- Do not include a slash if you are unsure; both styles may work, but plain command text is usually safer.
- Use `@s` for the player.
- Avoid commas in command text.
- Test the command manually in chat first.

### Battle starts too fast or dialog is skipped

The mod includes dialog/battle protection to avoid spam-clicking through the first dialog before the server starts the battle.

If you are editing the source code, the first-page lock is controlled in:

```text
src/client/java/net/crulim/kantonpcs/screen/custom/dialog/DialogScreen.java
```

Constant:

```java
FIRST_PAGE_ADVANCE_LOCK_TICKS
```

Lower values feel faster; higher values are safer against spam-click issues.

### A trainer can be battled only once

Check:

```text
Rematch Cooldown Seconds
```

`0` means single victory. Use a positive value for rematches.

### TraderNPC trades the wrong duplicate Pokémon

The intended/fixed behavior is that the trade uses the selected party slot.

If your build trades the first matching species in the party instead, update to the fixed KantoNPCs build/patch where TraderNPC validates the selected slot.

---

## 27. Suggested public support format

When reporting an issue, include:

```text
Minecraft version:
Fabric Loader version:
Fabric API version:
Cobblemon version:
KantoNPCs version:
Singleplayer or dedicated server:
NPC type:
What you configured:
What you expected:
What happened:
Latest log or crash report:
Screenshots/video if visual:
```

For dialog bugs, include:

```text
Dialog ID used in the NPC editor:
Relevant JSON block:
Language selected in Minecraft:
```

For MartNPC bugs, include:

```text
Buy/Sell entry fields:
Product/Item ID:
Price/Cost:
Description:
Quantity selected:
```

For TraderNPC or MoveTutorNPC bugs, include:

```text
Selected party slot:
Party order:
Configured request/offer/move:
```

---

## 29. License and project notice

KantoNPCs is part of the CobbleKanto project ecosystem and is covered by the CobbleKanto Project Custom License.

This is an unofficial fan-made Minecraft/Cobblemon project. It is not affiliated with, endorsed by, sponsored by, or approved by Nintendo, Game Freak, Creatures Inc., The Pokémon Company, Mojang Studios, Microsoft, or the Cobblemon team.
