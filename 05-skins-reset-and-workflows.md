# Rewards, Money and Commands

[← Back to documentation index](../README.md)

## 19. Reward Item field

Many NPC types have a reward field.

Depending on the NPC, it may be labeled:

```text
Reward Item
```

This field is usually given after the NPC's successful/completed interaction.

Examples:

```text
minecraft:diamond
cobblemon:rare_candy
rare_candy
```

### Multiple item rewards

Separate items with commas:

```text
minecraft:diamond, cobblemon:rare_candy
```

Each item is given once.

### Item components / advanced item syntax

If the reward contains item component/NBT-style bracket syntax, the mod may use a server-side `/give` command internally.

Example style:

```text
minecraft:diamond_sword[...]
```

Use this only if you already know the correct Minecraft 1.21.1 item component syntax.

### Commands inside Reward Item

For Dialog-style NPCs, the reward field can also accept command-like input.

Examples:

```text
/give @s minecraft:diamond 1
xp add @s 5 levels
say [player] completed a dialog
```

If the command starts with `/`, the slash is stripped internally.

The command runs server-side as the interacting/completing player.

`@s` becomes the player.

### Important command warning

Avoid commas inside commands unless you know the specific field supports bracket/quote-safe splitting.

For the safest setup, use one command per command field when possible.

---

## 20. On Victory Command / On Reward Command

BattleNPC and RivalNPC use:

```text
On Victory Command
```

Dialog-style NPCs may show:

```text
On Reward Command
```

These commands are executed server-side after the correct successful event.

Examples:

```text
/give @s minecraft:diamond 1
xp add @s 3 levels
scoreboard players add @s quest_progress 1
function mypack:quest/finish_route_1
```

### How `@s` works

The command is wrapped so that:

```text
@s = the player who completed the NPC interaction
```

So this:

```text
/give @s minecraft:diamond 1
```

means:

```text
Give a diamond to the player who won/talked/traded/tutored successfully.
```

---

## 21. Money system

KantoNPCs includes a simple player money system used by MartNPC and trainer rewards.

BattleNPC and RivalNPC can give money rewards after victory.

MartNPC can charge money for products.

### Money commands

```text
/kantonpcs money get
/kantonpcs money get <player>
/kantonpcs money set <value>
/kantonpcs money set <value> <player>
/kantonpcs money add <value>
/kantonpcs money add <value> <player>
/kantonpcs money remove <value>
/kantonpcs money remove <value> <player>
/kantonpcs money reset
/kantonpcs money reset <player>
```

Admin/operator permission may be required for modifying another player's money or using set/add/remove/reset.

### Money Reward

BattleNPC/RivalNPC can use:

```text
Money Reward: 500
```

The player receives that amount after winning, subject to normal reward/rematch rules.

If the mod's Amulet Coin system is active for the battle, reward money may be doubled.

---

## 28. Quick reference

### Main config paths

```text
config/kantonpcs/dialogs/en_us_arrays.json
config/kantonpcs/dialogs/pt_br_arrays.json
config/kantonpcs/choices/en_us_choices.json
config/kantonpcs/choices/pt_br_choices.json
config/kantonpcs/descriptions/martnpc_desc_en_us.json
config/kantonpcs/descriptions/martnpc_desc_pt_br.json
config/kantonpcs/skins/
```

### Useful dialog segments

```text
start = before battle
end   = after battle victory/result
post  = later interactions after completion
main  = main Yes/No question
yes   = Yes branch
no    = No branch
cancel = cancel/back branch
```

### Common placeholders

```text
[player]
```

### Common command target

```text
@s = the player who completed the NPC interaction
```

### Common species format

```text
charizard
blastoise
pikachu
mr_mime
```

### Common move format

```text
flamethrower
thunder_punch
ice_beam
quick_attack
```

### Common item format

```text
minecraft:diamond
cobblemon:rare_candy
cobblemon:potion
```

---
