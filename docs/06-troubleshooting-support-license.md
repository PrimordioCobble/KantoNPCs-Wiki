# Skins, Reset and Workflows

[← Back to documentation index](../README.md)

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
