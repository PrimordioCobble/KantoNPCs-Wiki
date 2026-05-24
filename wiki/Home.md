# KantoNPCs Manual

Advanced NPC tools for Cobblemon adventure maps, multiplayer servers, and CobbleKanto-style projects.

This manual explains the practical setup workflow for KantoNPCs: how to create each NPC type, what the important fields mean, how to write dialog JSON files, how rewards/commands work, and how the external config folders are organized.

> This is a community/map-maker manual. It is not meant to replace experimentation inside the editor screens, but it should give you enough context to configure NPCs without guessing every field.

---

## 1. Basic requirements

KantoNPCs is designed for:

- Minecraft `1.21.1`
- Fabric
- Java `21`
- Cobblemon `1.7.1+1.21.1` or newer in the `1.7.x` line
- Fabric API

Recommended setup for servers and map makers:

1. Install the mod on both server and client.
2. Start the game/server once.
3. Let the mod generate its default folders under `config/kantonpcs/`.
4. Edit dialogs, skins, descriptions, and NPCs from there.
5. Restart/reload the world if you edited files outside the in-game editor.

---

## 2. Runtime folders

After first launch, the mod creates editable files under:

```text
.minecraft/config/kantonpcs/
```

Main subfolders:

```text
config/kantonpcs/dialogs/
config/kantonpcs/choices/
config/kantonpcs/descriptions/
config/kantonpcs/skins/
```

### `config/kantonpcs/dialogs/`

Stores the main dialog array files:

```text
en_us_arrays.json
pt_br_arrays.json
```

These files contain most NPC dialog IDs and their text arrays.

### `config/kantonpcs/choices/`

Optional folder for standalone choice dialog files:

```text
en_us_choices.json
pt_br_choices.json
```

You can use this folder for Yes/No style dialogs, although the mod also supports `answers` blocks directly inside the arrays file.

### `config/kantonpcs/descriptions/`

Stores MartNPC item descriptions:

```text
martnpc_desc_en_us.json
martnpc_desc_pt_br.json
```

These are mainly used when MartNPC product descriptions use keys such as:

```text
desc.potion
```

### `config/kantonpcs/skins/`

Stores external NPC skins.

The mod can copy default bootstrap skins here on first launch. Custom skins can also be added manually as `.png` files.

Use the in-game **Change Model** screen and click:

```text
Open Folder
Refresh
```

Then select the skin from the list.

### Rival data folder

Rival starter/team data is world/server save data, not global config. It may be stored under the world save root, for example:

```text
<world>/kantonpcs/rival/starter.json
<world>/kantonpcs/rival/teams.json
```

Do not treat this as the same thing as `config/kantonpcs/`. The config folder is for reusable external resources; rival storage is per-world/per-server data.

---

## 3. Creating, editing, removing, and copying NPCs

### Spawn items

The mod provides spawn items for the main NPC types:

```text
Battle NPC Spawn Item
Rival NPC Spawn Item
Dialog NPC Spawn Item
Mart NPC Spawn Item
Trader NPC Spawn Item
Move Tutor NPC Spawn Item
```

Use the spawn item on a block to create the NPC.

### Editing an NPC

To edit an NPC:

1. Enter Creative Mode.
2. Sneak.
3. Right-click the NPC.

This opens the editor screen for that NPC type.

### Removing an NPC

Use the Remove NPC tool/item, or use the built-in creative/sneak removal behavior depending on the NPC type.

Recommended safe workflow:

1. Enter Creative Mode.
2. Sneak.
3. Use the Remove NPC item on the target NPC.

### NPC Preset item

The NPC Preset item can copy an existing NPC and spawn another copy later.

Typical workflow:

- Use the preset item on an NPC to copy its data.
- Use the preset item on a block to spawn a copy.
- Sneak behavior may overwrite the held preset instead of creating a new one.

This is useful for map makers who want to clone configured NPCs without redoing every field.

---

## 4. Common editor fields

Many NPC types share the same basic settings. The field may appear in slightly different tabs depending on the NPC.

### NPC Name

The display/name field shown by the NPC renderer and editor.

Can be left empty.

### NPC Size

Controls the visual scale of the NPC.

Examples:

```text
0.9375
1.0
1.2
0.8
```

Use decimals. The editor usually accepts small adjustments through buttons.

### Model Type: Wide / Slim

Controls the skin geometry:

```text
Wide = classic/default player arms
Slim = slim/Alex-style arms
```

Use the model type that matches the skin you selected.

### Skin

The selected skin file/texture.

External skins should be placed in:

```text
config/kantonpcs/skins/
```

After adding skins manually, click **Refresh** in the Change Model screen.

### Home Pos

The NPC's home/anchor position.

Used by reset behavior, position locking, and some movement logic.

### Entity Pos

The NPC's current physical position.

This is useful when moving the NPC precisely after it already exists.

### Home Yaw

The NPC's default facing direction.

This is a numeric yaw value. Use it to make the NPC face the correct direction after reset or when locked.

### Sitting: Yes/No

Controls whether the NPC is rendered in a sitting pose.

### Follow: Yes/No

For NPCs that expose this field, this controls whether the NPC should look toward/follow the interacting player visually.

### Lock Pos: Yes/No

Locks the NPC in place.

Recommended for adventure maps and servers where NPCs should never drift, fall, be pushed, or move away from their intended spot.

### Rematch Cooldown Seconds

Used by BattleNPC and RivalNPC.

```text
0 = single victory / no rematch after the player wins
>0 = player can rematch after this many seconds
```

Examples:

```text
0
300
86400
```

`300` = 5 minutes.  
`86400` = 24 hours.

---

## 5. Dialog ID field

Most NPCs use a `Dialog ID` field.

That field must match a key inside your dialog JSON file.

Example editor field:

```text
route_01_trainer_1
```

Matching JSON key:

```json
{
  "route_01_trainer_1": [
    "Hey! I saw you in the tall grass!",
    "Let's battle!"
  ]
}
```

If the Dialog ID does not exist, the NPC may open with empty dialog, skip dialog, or go directly to its next behavior depending on the NPC type.

---

## 6. Dialog files and locales

Main files:

```text
config/kantonpcs/dialogs/en_us_arrays.json
config/kantonpcs/dialogs/pt_br_arrays.json
```

Optional choice files:

```text
config/kantonpcs/choices/en_us_choices.json
config/kantonpcs/choices/pt_br_choices.json
```

The mod normalizes languages like this:

```text
en       -> en_us
pt       -> pt_br
en-us    -> en_us
pt-br    -> pt_br
```

For unsupported languages, English is used as fallback.

### Config overrides bundled defaults

The mod ships bundled default dialogs inside the jar. On first launch, it can copy default files into `config/kantonpcs/dialogs/`.

Config files override bundled files. This means you can customize dialogs without editing the jar.

### JSON style note

The mod is somewhat tolerant of trailing commas in arrays/objects, but you should still try to keep valid JSON.

Recommended:

- Use double quotes.
- Do not forget commas between lines.
- Keep each dialog ID unique.
- Keep each text line reasonably short.

---

## 7. Writing simple dialogs

The simplest format is one dialog ID mapped to an array of strings.

```json
{
  "pallet_town_npc_1": [
    "Technology is incredible!",
    "You can now store Cobblemon data",
    "in many different ways."
  ]
}
```

You can also use the array-of-objects format:

```json
[
  {
    "pallet_town_npc_1": [
      "Technology is incredible!",
      "You can now store Cobblemon data",
      "in many different ways."
    ]
  }
]
```

Both formats are supported.

---

## 8. Battle-style dialogs: `start`, `end`, and `post`

BattleNPC and RivalNPC usually use three dialog segments:

```json
{
  "route_03_trainer_1": {
    "start": [
      "I like shorts!",
      "They're comfy and easy to wear!"
    ],
    "end": [
      "I don't believe it!"
    ],
    "post": [
      "Are shorts really that good?",
      "I still think they are."
    ]
  }
}
```

### Segment meaning

| Segment | Meaning |
|---|---|
| `start` | Shown before the battle starts. |
| `end` | Shown after the player wins/finishes the battle result flow. |
| `post` | Shown on later interactions after the NPC has already been defeated/completed. |

For BattleNPC and RivalNPC, this is the recommended format.

---

## 9. DialogNPC / Yes-No style dialogs

DialogNPC, MartNPC, TraderNPC, and MoveTutorNPC can use choice-style dialogs with `main`, `yes`, and `no` branches.

Example inside `en_us_arrays.json`:

```json
[
  {
    "viridian_healer_1": [
      "Your Cobblemon look tired."
    ],
    "answers": {
      "main": [
        "Your Cobblemon look tired.",
        "Would you like me to heal them?"
      ],
      "yes": {
        "nextdialog": [
          "All right.",
          "Let me take care of them."
        ]
      },
      "no": {
        "nextdialog": [
          "No problem.",
          "Come back anytime."
        ]
      }
    }
  }
]
```

The `answers` block supports:

```text
main
yes
no
cancel
```

The branch can be written directly as an array:

```json
"yes": [
  "Great!"
]
```

Or as an object using `nextdialog`:

```json
"yes": {
  "nextdialog": [
    "Great!"
  ]
}
```

Supported names for nested branch text:

```text
nextdialog
nextDialog
next_dialog
dialog
lines
```

Recommended style: use `nextdialog` everywhere for consistency.

---

## 10. Standalone choice file format

You can also write choices in:

```text
config/kantonpcs/choices/en_us_choices.json
```

Example:

```json
{
  "move_tutor_dream_eater": {
    "main": [
      "I can teach DREAM EATER.",
      "Do you want one of your Cobblemon to learn it?"
    ],
    "yes": [
      "Choose the Cobblemon that should learn it."
    ],
    "no": [
      "Maybe another time."
    ],
    "cancel": [
      "Come back if you change your mind."
    ]
  }
}
```

For most map makers, keeping the `answers` block inside `*_arrays.json` is easier because the dialog ID and its branches stay together.

---

## 11. Dialog variables

The main built-in variable is:

```text
[player]
```

It is replaced by the player's name.

Example:

```json
{
  "welcome_player": [
    "Hey, [player]!",
    "Welcome to this town."
  ]
}
```

Result:

```text
Hey, Steve!
Welcome to this town.
```

The replacement is case-insensitive, so `[Player]`, `[PLAYER]`, and `[player]` should all work.

---

## 12. Dialog writing tips

Recommended line length:

```text
Around 30-40 characters per line
```

This depends on your UI scale and language, but shorter lines usually look better.

Good:

```json
[
  "This cave has been unstable",
  "since the anomaly appeared.",
  "Please be careful inside."
]
```

Avoid very long single strings:

```json
[
  "This cave has been unstable since the anomaly appeared and several people have already gone missing inside."
]
```

Use multiple strings to control pacing and readability.

---

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

## 22. Skins and bootstrap skins

Default skins can be bundled inside the mod under:

```text
src/main/resources/assets/kantonpcs/bootstrap/
```

The index file is:

```text
src/main/resources/assets/kantonpcs/bootstrap/skins_index.json
```

Example:

```json
[
  "oak.png",
  "misty.png",
  "brock.png"
]
```

On first launch, the mod copies missing listed skins into:

```text
config/kantonpcs/skins/
```

Important behavior:

```text
Bootstrap only copies missing files.
It does not overwrite existing skins in config/kantonpcs/skins/.
```

If you update a bundled skin and want to test the copied version, delete or replace the old file in:

```text
config/kantonpcs/skins/
```

Then relaunch or refresh the folder.

---

## 23. NPC Reset

Most editor menus include:

```text
NPC Reset
```

This resets the NPC's state/data depending on its type.

Common uses:

- Clear player completion state.
- Allow a defeated trainer to be fought again.
- Clear trade/tutor reward state.
- Restore NPC interaction flow during testing.

When testing quests, rewards, and post-dialog behavior, reset the NPC between tests.

---

## 24. Recommended configuration examples

### Example: simple trainer

Dialog ID:

```text
route_01_trainer_1
```

Dialog JSON:

```json
{
  "route_01_trainer_1": {
    "start": [
      "You're a new trainer, right?",
      "Let's see what you can do!"
    ],
    "end": [
      "Wow, you're strong!"
    ],
    "post": [
      "Keep training.",
      "The road ahead gets harder."
    ]
  }
}
```

Cobblemon team:

```text
pidgey level 7
grass-type or route-themed Pokémon as needed
```

Settings:

```text
Seek Player: Yes
Range: 6
Rematch Cooldown Seconds: 0
Lock Pos: Yes
```

Reward:

```text
Reward Item: cobblemon:poke_ball
Money Reward: 120
```

### Example: healer DialogNPC

Dialog ID:

```text
town_healer
```

Dialog JSON:

```json
[
  {
    "town_healer": [
      "Your team looks tired."
    ],
    "answers": {
      "main": [
        "Your team looks tired.",
        "Would you like me to heal them?"
      ],
      "yes": {
        "nextdialog": [
          "There you go!",
          "Your Cobblemon are ready."
        ]
      },
      "no": {
        "nextdialog": [
          "All right.",
          "Come back if you need help."
        ]
      }
    }
  }
]
```

Settings:

```text
Heal: Yes
Lock Pos: Yes
```

### Example: MartNPC selling a Pokémon

Buy entry:

```text
Product: lapras lvl=25 gender=random
Price: 5000
Description: A rare water-type partner.
```

### Example: MartNPC item-trade shop

Buy entry:

```text
Product: cobblemon:rare_candy
Price: cobblemon:red_apricorn x3 | cobblemon:blue_apricorn x3
Description: Trade apricorns for a rare candy.
```

### Example: TraderNPC

Trade tab:

```text
Request: haunter
Offer: gengar
Offer Level: 36
Offer Gender: random
Unlimited Trade: No
```

Dialog:

```json
[
  {
    "trade_haunter_gengar": [
      "I love spooky Cobblemon."
    ],
    "answers": {
      "main": [
        "Do you have a Haunter?",
        "I can trade you something special."
      ],
      "yes": {
        "nextdialog": [
          "Great! Choose the Haunter",
          "you want to trade."
        ]
      },
      "no": {
        "nextdialog": [
          "Come back if you find one."
        ]
      }
    }
  }
]
```

### Example: MoveTutorNPC

Move Tutor tab:

```text
Move ID: thunder_punch
```

Dialog:

```json
[
  {
    "tutor_thunder_punch": [
      "I can teach a shocking move."
    ],
    "answers": {
      "main": [
        "I can teach THUNDER PUNCH.",
        "Want me to teach it?"
      ],
      "yes": {
        "nextdialog": [
          "Choose the Cobblemon",
          "that should learn it."
        ]
      },
      "no": {
        "nextdialog": [
          "Maybe later."
        ]
      }
    }
  }
]
```

---

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

## 26. Recommended workflow for map makers

For each NPC:

1. Create the dialog ID first in the JSON file.
2. Spawn the NPC.
3. Set model, skin, and name.
4. Set Dialog ID.
5. Configure only the type-specific feature:
   - Battle team
   - Rival teams
   - Shop entries
   - Trade request/offer
   - Tutor move
6. Configure reward/money/command only after the main interaction works.
7. Test once as a normal player.
8. Reset the NPC and test again.
9. Test on a dedicated server before publishing a modpack/map.

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

## 29. License and project notice

KantoNPCs is part of the CobbleKanto project ecosystem and is covered by the CobbleKanto Project Custom License.

This is an unofficial fan-made Minecraft/Cobblemon project. It is not affiliated with, endorsed by, sponsored by, or approved by Nintendo, Game Freak, Creatures Inc., The Pokémon Company, Mojang Studios, Microsoft, or the Cobblemon team.
