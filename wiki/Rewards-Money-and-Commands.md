# Dialogs and Choice Files

[← Back to Home](Home)

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
