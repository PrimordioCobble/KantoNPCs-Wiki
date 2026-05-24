# NPC Types

[← Back to documentation index](../README.md)

## 13. BattleNPC

BattleNPC is a trainer-style NPC.

Main purpose:

```text
Dialog -> trainer battle -> reward/money -> post dialog
```

Typical tabs:

```text
Change Model
Dialogs
Cobblemon
Settings
Seek Player
Item
Money
NPC Reset
Save
```

### Recommended BattleNPC setup

1. Set model/name/skin in **Change Model**.
2. Set the dialog ID in **Dialogs**.
3. Configure the team in **Cobblemon**.
4. Set size/home/lock/rematch in **Settings**.
5. Configure auto-challenge in **Seek Player**, if needed.
6. Configure item/command reward in **Item**.
7. Configure money reward in **Money**.
8. Save.

### BattleNPC Cobblemon team fields

Each team entry can use:

| Field | Meaning |
|---|---|
| `nameID` | Cobblemon species ID. Example: `charizard`. |
| `Level` | Pokémon level, usually `1` to `100`. |
| `Gender` | Optional gender control. |
| Move fields | Up to 4 move IDs. |

Species examples:

```text
pikachu
charizard
blastoise
mr_mime
```

Move examples:

```text
tackle
flamethrower
thunderbolt
hydro_pump
quick_attack
```

If no moves are configured, the mod lets Cobblemon initialize/default the moveset based on the Pokémon/level.

### Gender values

Supported/common values:

```text
male
m
female
f
genderless
none
x
random
```

If the selected gender is invalid for the species, the mod falls back to an allowed gender.

### Seek Player

BattleNPC can automatically approach/challenge nearby players.

Fields:

```text
Seek Player: Yes/No
Blocks Range (limit: 16)
```

Recommended for trainer routes:

```text
Seek Player: Yes
Range: 5 to 8
```

Recommended for static bosses or NPCs that should only trigger on click:

```text
Seek Player: No
```

### Rematch behavior

In Settings:

```text
Rematch Cooldown Seconds: 0
```

means single-victory trainer.

Use a higher number to allow rematches after a delay.

---

## 14. RivalNPC

RivalNPC is a trainer NPC with multiple team branches based on the player's selected/recorded starter type.

It behaves similarly to BattleNPC, but the team editor is organized by starter routes:

```text
GRASS
FIRE
WATER
```

Typical use:

```text
If player chose Grass starter -> Rival uses one configured team.
If player chose Fire starter -> Rival uses another configured team.
If player chose Water starter -> Rival uses another configured team.
```

RivalNPC is best for story rivals, repeated rival battles, and map progression fights where the enemy team depends on the player's starter path.

### RivalNPC tabs

Typical tabs:

```text
Change Model
Dialogs
Cobblemon / Rival Teams
Settings
Seek Player
Item
Money
NPC Reset
Save
```

### Rival starter commands

The mod includes admin commands for checking and managing rival starter data.

Common commands:

```text
/kantonpcs rivalstarters view
/kantonpcs rivalstarters view <player>
/kantonpcs rivalstarters list
/kantonpcs rivalstarters scan
/kantonpcs rivalstarters scan <player>
/kantonpcs rivalstarters set <key>
/kantonpcs rivalstarters set <key> <player>
/kantonpcs rivalstarters clear
/kantonpcs rivalstarters clear <player>
/kantonpcs rivalstarters restart
```

Usually, keys are the starter routes used by the mod, such as:

```text
grass
fire
water
```

Admin/operator permission may be required depending on the command.

---

## 15. DialogNPC

DialogNPC is the general-purpose talking NPC.

Main purpose:

```text
Talk -> optional Yes/No branch -> optional reward/command/heal
```

Typical tabs:

```text
Change Model
Dialogs
Settings
Item
NPC Reset
Save
```

Use DialogNPC for:

- Story characters
- Hints
- Quest-style interactions
- NPCs that give items/commands
- Healing NPCs
- One-time rewards

### DialogNPC dialog setup

For a simple NPC, use a normal array:

```json
{
  "town_hint_npc": [
    "The forest ahead is dangerous.",
    "Make sure your team is ready."
  ]
}
```

For Yes/No behavior, use an `answers` block:

```json
[
  {
    "town_healer": [
      "Need some help?"
    ],
    "answers": {
      "main": [
        "Your team looks tired.",
        "Do you want me to heal them?"
      ],
      "yes": {
        "nextdialog": [
          "There you go!",
          "Your team should feel better now."
        ]
      },
      "no": {
        "nextdialog": [
          "All right.",
          "Stay safe out there."
        ]
      }
    }
  }
]
```

### Heal: Yes/No

DialogNPC Settings can include:

```text
Heal: Yes
```

When enabled, the NPC can heal the player's Cobblemon after the dialog flow finishes.

This is useful for nurse-style NPCs, rest spots, or checkpoints.

---

## 16. MartNPC

MartNPC is a shop NPC with buy and sell configuration.

Main purpose:

```text
Dialog -> Buy/Sell screen -> money or item-trade economy
```

Typical tabs:

```text
Change Model
Dialogs
Settings
Item
Edit Buy
Edit Sell
NPC Reset
Save
```

### MartNPC Buy list

Each buy entry has three main fields:

| Field | Meaning |
|---|---|
| Product / Item ID | What the player receives. |
| Price | Money price or item-trade cost. |
| Description | Text or `desc.*` key shown in the shop UI. |

### Selling a normal item for money

Product:

```text
cobblemon:potion
```

Price:

```text
300
```

Description:

```text
desc.potion
```

The player pays `$300` and receives one Potion.

### Selling multiple items at once

Product:

```text
cobblemon:poke_ball x5
```

Price:

```text
1000
```

The player pays `$1000` and receives 5 Poké Balls.

### Product count syntax

Supported examples:

```text
minecraft:diamond x3
minecraft:diamondx3
cobblemon:potion x2
```

### Multiple products in one purchase

Separate products with commas:

```text
cobblemon:potion x2, cobblemon:poke_ball x5
```

This gives both products from a single shop entry.

### Selling Pokémon as a Mart product

If the product is not a registered item, the MartNPC can treat it as a Cobblemon species spec.

Examples:

```text
lapras lvl=25 gender=female
cobblemon:eevee level=15 gender=random
pikachu lv=10 sex=male
```

Supported Pokémon spec options:

```text
lvl=25
level=25
lv=25
gender=male
gender=female
gender=random
gender=genderless
sex=male
```

When bought, the Pokémon is added to the player's party or PC if possible.

### Item-trade shop costs

The Price field can be used as an item cost instead of money.

Example:

```text
cobblemon:rare_candy x2
```

The player must pay 2 Rare Candies instead of money.

### Multiple required cost items

Comma means the player must pay all cost groups.

```text
cobblemon:red_apricorn x1, cobblemon:blue_apricorn x1
```

This requires both items.

### Alternative cost items

Pipe `|` means alternatives.

```text
cobblemon:red_apricorn x1 | cobblemon:blue_apricorn x1
```

The player can pay either one.

### Combined cost logic

Example:

```text
cobblemon:red_apricorn x1 | cobblemon:blue_apricorn x1, minecraft:emerald x3
```

This means:

```text
(red apricorn OR blue apricorn) AND 3 emeralds
```

### MartNPC Sell list

The Sell list controls which items players can sell to the NPC.

Each sell entry usually has:

```text
Item ID
Price
Description
```

Example:

```text
minecraft:diamond
100
A valuable gem.
```

The player can sell diamonds to this MartNPC for `$100` each.

### MartNPC description keys

Descriptions can be direct text:

```text
A basic healing item.
```

Or a key:

```text
desc.potion
```

Keys are resolved from:

```text
config/kantonpcs/descriptions/martnpc_desc_en_us.json
config/kantonpcs/descriptions/martnpc_desc_pt_br.json
```

Example description file:

```json
{
  "desc.potion": "Restores a small amount of HP.",
  "desc.poke_ball": "A basic ball used to catch wild Cobblemon."
}
```

---

## 17. TraderNPC

TraderNPC trades one selected Cobblemon from the player for another configured Cobblemon.

Main purpose:

```text
Dialog -> choose selected party Pokémon -> trade -> optional reward/command
```

Typical tabs:

```text
Change Model
Dialogs
Trade
Settings
Item
NPC Reset
Save
```

### Trade setup fields

| Field | Meaning |
|---|---|
| `Request (cobblemon id)` | Species the player must give. |
| `Offer (cobblemon id)` | Species the player receives. |
| `Offer Level` | Level of the received Pokémon. |
| `Offer Gender` | Gender of the received Pokémon. |
| `Unlimited Trade` | Whether the same player can trade repeatedly with this NPC. |

Example:

```text
Request: charizard
Offer: blastoise
Offer Level: 36
Offer Gender: random
Unlimited Trade: No
```

### Species ID syntax

Use Cobblemon species IDs:

```text
charizard
blastoise
mr_mime
farfetchd
```

The editor validates species IDs.

### Gender values

Trader offer gender accepts values such as:

```text
male
female
genderless
none
neutral
random
```

### Selected party slot behavior

The trade is intended to use the player's selected party slot.

Example:

```text
Party slot 1: Pikachu selected
Party slot 2: Charizard
Party slot 3: Charizard
Trader asks for: Charizard
```

Correct behavior:

```text
The trade should fail because the selected Pokémon is Pikachu.
```

If the player selects the Charizard in slot 2, the NPC should trade the slot 2 Charizard. If the player selects slot 3, it should trade slot 3.

This matters when the player has duplicate species but wants to trade a specific one.

---

## 18. MoveTutorNPC

MoveTutorNPC teaches one configured Cobblemon move to the selected Pokémon in the player's party.

Main purpose:

```text
Dialog -> choose selected party Pokémon -> teach move -> optional reward/command
```

Typical tabs:

```text
Change Model
Dialogs
Settings
Move Tutor
Item
NPC Reset
Save
```

### Move Tutor field

The main field is:

```text
Move ID (Cobblemon)
```

Examples:

```text
thunder_punch
flamethrower
ice_beam
quick_attack
thunderbolt
```

The tutor normalizes some input:

```text
Spaces -> underscores
Hyphens -> underscores
Namespace can be stripped
```

Examples that should resolve similarly:

```text
thunder punch
thunder-punch
cobblemon:thunder_punch
```

### Move teaching behavior

The MoveTutorNPC checks:

1. The move ID is valid.
2. The selected party slot contains a Pokémon.
3. The selected Pokémon can learn the move.
4. The Pokémon does not already know the move.

If the Pokémon has an empty move slot, the move is added directly.

If the Pokémon already has 4 moves, the move can be added to its remembered/benched moves so the player can remember it later.

---
