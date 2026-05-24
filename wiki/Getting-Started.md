# Getting Started

[← Back to Home](Home)

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
