# .chart DJ Hero Track Specifications (Proposal)

This document details a proposal for adding DJ Hero tracks to the .chart format.

## Table of Contents
- [.chart DJ Hero Track Specifications (Proposal)](#chart-dj-hero-track-specifications-proposal)
  - [Table of Contents](#table-of-contents)
  - [Section Names](#section-names)
  - [Metadata](#metadata)
  - [Global Events](#global-events)
    - [Megamix-specific Events](#megamix-specific-events)
  - [Track Details](#track-details)
    - [Note, Modifier, and Special Phrase Type Divisions](#note-modifier-and-special-phrase-type-divisions)
    - [Turntable Tracks](#turntable-tracks)
    - [Turntable Note Types](#turntable-note-types)
    - [Turntable Special Phrase Types](#turntable-special-phrase-types)
    - [Turntable Local Events](#turntable-local-events)

## Section Names

Added difficulties:

- `Beginner`

Tracks:

- `DJTurntable` - Single-table track
- `DJDualTurntable` - Dual-table track

## Metadata

| Key                | Description                                                                           | Data type |
| :----------------- | :------------------------------------------------------------------------------------ | :-------- |
| `DJArtist`         | The DJ/producer who mixed the song.                                                   | string    |
| `DJGreenStream`    | Green track audio.                                                                    | file path |
| `DJRedStream`      | Red track audio.                                                                      | file path |
| `DJBlueStream`     | Blue track audio.                                                                     | file path |
| `DJSample<number>` | An audio sample.<br>`number` is the number used to refer to this sample in the chart. | file path |

## Global Events
The standard `section` and `prc_` events are extended to mark rewind checkpoints, in addition to the following events:

| Event Text                                          | Description                                                                                                                                                                                               |
| :-------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dj_rewind_checkpoint [name]`                       | Marks an additional rewind checkpoint.<br>`name` is an optional name given to the checkpoint/section.                                                                                                     |
| `dj_battle_lanes <green> <red> <blue>`              | Specifies which players get which lanes in a DJ battle.<br>`green` specifies which players get the green lane: `p1`, `p2`, or `both`. `red` and `blue` for the red and blue lanes follow the same syntax. |
| `djdual_battle_lanes <g1> <r1> <b1> <g2> <r2> <b2>` | Specifies which players get which lanes in a dual-table DJ battle.<br>This event's parameters work the same as `dj_battle_event`, it just has 3 additional lane parameters for dual-table.                |
| `dj_battle_checkpoint [name]`                       | Marks a battle checkpoint.<br>`name` is an optional name for the checkpoint.                                                                                                                              |

### Megamix-specific Events
| Event Text              | Description                                                                                                                                                                 |
| :---------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dj_megamix_intro`      | Marks the point where the megamix extended intro begins. This event must be placed before a `dj_megamix_intro_end` event.                                                   |
| `dj_megamix_intro_end`  | Marks the point where the megamix extended intro ends and where the normal mix begins. When playing the mix by itself only the notes after this event should be considered. |
| `dj_megamix_transition` | Marks the transition point of a megamix.                                                                                                                                    |

## Track Details

### Note, Modifier, and Special Phrase Type Divisions

- Note/modifier types:

| Note Type   | Description                        |
| :---------- | :--------------------------------- |
| 0 thru 31   | 1st table taps/scratches/freestyle |
| 32 thru 63  | 2nd table taps/scratches/freestyle |
| 64 thru 95  | Crossfade markers                  |
| 96 thru 127 | Miscellaneous                      |

- Special phrase types:

| Note Type   | Description              |
| :---------- | :----------------------- |
| 0 thru 31   | Standard special phrases |
| 32 thru 63  | 1st table phrases        |
| 64 thru 95  | 2nd table phrases        |
| 96 thru 127 | Miscellaneous            |

### Turntable Tracks

- `DJTurntable`
- `DJDualTurntable`

Note that this section specifies types for both tracks in a single section. Types specific to a track will be specified as such.

### Turntable Note Types

Note types marked with an asterisk (\*) are required for full compatibility with the DJ Hero 2 feature set. All other note types are extensions of the the DJ Hero 2 feature set, and are defined in order to give an engine the option to use them.

| Note Type     | Description                                                    |
| :------------ | :------------------------------------------------------------- |
| First table   |                                                                |
| 0\*           | Table 1: Green (1st lane) tap                                  |
| 1\*           | Table 1: Red (2nd lane) tap                                    |
| 2\*           | Table 1: Blue (3rd lane) tap                                   |
| 3             | Table 1: Open tap / Lift                                       |
|               |                                                                |
| 4\*           | Table 1: Green scratch up                                      |
| 5             | Table 1: Red scratch up                                        |
| 6\*           | Table 1: Blue scratch up                                       |
| 7             | Table 1: Open scratch up                                       |
|               |                                                                |
| 8\*           | Table 1: Green scratch down                                    |
| 9             | Table 1: Red scratch down                                      |
| 10\*          | Table 1: Blue scratch down                                     |
| 11            | Table 1: Open scratch down                                     |
|               |                                                                |
| 12\*          | Table 1: Green scratch any direction                           |
| 13            | Table 1: Red scratch any direction                             |
| 14\*          | Table 1: Blue scratch any direction                            |
| 15            | Table 1: Open scratch any direction                            |
|               |                                                                |
| 16\*          | Table 1: Green scratch zone                                    |
| 17            | Table 1: Red scratch zone                                      |
| 18\*          | Table 1: Blue scratch zone                                     |
| 19            | Table 1: Open scratch zone                                     |
|               |                                                                |
| 20            | Table 1: Green freestyle sample zone                           |
| 21\*          | Table 1: Red freestyle sample zone                             |
| 22            | Table 1: Blue freestyle sample zone                            |
|               |                                                                |
| 24\*          | Table 1: Green freestyle scratch zone                          |
| 25            | Table 1: Red freestyle scratch zone                            |
| 26\*          | Table 1: Blue freestyle scratch zone                           |
|               |                                                                |
| Second table  | These note types are only used in the `DJDualTurntable` track. |
| 32            | Table 2: Green (1st lane) tap                                  |
| 33            | Table 2: Red (2nd lane) tap                                    |
| 34            | Table 2: Blue (3rd lane) tap                                   |
| 35            | Table 2: Open tap / Lift                                       |
|               |                                                                |
| 36            | Table 2: Green scratch up                                      |
| 37            | Table 2: Red scratch up                                        |
| 38            | Table 2: Blue scratch up                                       |
| 39            | Table 2: Open scratch up                                       |
|               |                                                                |
| 40            | Table 2: Green scratch down                                    |
| 41            | Table 2: Red scratch down                                      |
| 42            | Table 2: Blue scratch down                                     |
| 43            | Table 2: Open scratch down                                     |
|               |                                                                |
| 44            | Table 2: Green scratch any direction                           |
| 45            | Table 2: Red scratch any direction                             |
| 46            | Table 2: Blue scratch any direction                            |
| 47            | Table 2: Open scratch any direction                            |
|               |                                                                |
| 48            | Table 2: Green scratch zone                                    |
| 49            | Table 2: Red scratch zone                                      |
| 50            | Table 2: Blue scratch zone                                     |
| 51            | Table 2: Open scratch zone                                     |
|               |                                                                |
| 52            | Table 2: Green freestyle sample zone                           |
| 53            | Table 2: Red freestyle sample zone                             |
| 54            | Table 2: Blue freestyle sample zone                            |
|               |                                                                |
| 56            | Table 2: Green freestyle scratch zone                          |
| 57            | Table 2: Red freestyle scratch zone                            |
| 58            | Table 2: Blue freestyle scratch zone                           |
|               |                                                                |
| Crossfades    |                                                                |
| 64\*          | Green crossfade                                                |
| 65\*          | Center crossfade                                               |
| 66\*          | Blue crossfade                                                 |
| 67\*          | Crossfade force spike/non-spike swap                           |
|               |                                                                |
| 68\*          | All-lane freestyle crossfade                                   |
| 69\*          | Freestyle crossfade Green guideline markers                    |
| 70\*          | Freestyle crossfade Blue guideline markers                     |
|               |                                                                |
| Miscellaneous |                                                                |
| 96            | Effect dial spin left                                          |
| 97            | Effect dial spin right                                         |

Open notes are notes that require no buttons to be pressed.

Crossfades will automatically become spike crossfades if their length is a 1/16th note or less. These spikes can be forced as regular crossfades by using the spike/non-spike swap note. Regular fades can also be forced as spikes regardless of their length.

The sample used in a freestyle sample/scratch is determined by a corresponding or prior `dj_sample` local event.

### Turntable Special Phrase Types

Special types marked with an asterisk (\*) are required for full compatibility with the DJ Hero 2 feature set. All other special phrase types are extensions of the the DJ Hero 2 feature set, and are defined in order to give an engine the option to use them.

| Special Type  | Description                                                      |
| :------------ | :--------------------------------------------------------------- |
| 2\*           | Euphoria phrase                                                  |
|               |                                                                  |
| First table   |                                                                  |
| 32\*          | Table 1: Green effect zone                                       |
| 33\*          | Table 1: Red effect zone                                         |
| 34\*          | Table 1: Blue effect zone                                        |
| 35\*          | Table 1: All-lane effect zone                                    |
|               |                                                                  |
| Second table  | These phrase types are only used in the `DJDualTurntable` track. |
| 64            | Table 2: Green effect zone                                       |
| 65            | Table 2: Red effect zone                                         |
| 66            | Table 2: Blue effect zone                                        |
| 67            | Table 2: All-lane effect zone                                    |
|               |                                                                  |
| Miscellaneous |                                                                  |
| 96\*          | Nullify crossfades and force center crossfade                    |

The effect type for effect zones is determined by a corresponding or prior `dj_effect` local event.

### Turntable Local Events

| Event Text                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| :-------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dj_effect <name>`          | Specifies the audio effect to be used by a corresponding effect zone.<br>`name` is the name of the effect to be used.                                                                                                                                                                                                                                                                                                                                     |
| `dj_sample <number> [lane]` | Specifies a sample number to be used by a corresponding freestyle sample/scratch zone.<br>`number` is the number of the sample to use, as specified in the `[Song]` section.<br>`lane` is an optional parameter to implement & use that specifies the lane a sample should apply to, for cases where multiple lanes are in use.<br>The engine playing the chart may extend this event to apply to other notes, e.g. use samples as tap/scratch hitsounds. |

Available effect names for `dj_effect`:

- `BeatRoll`
- `BeatRoll_AutoAdvance`
- `BitReduction` (bitcrush)
- `Delay`
- `Filter` (low-pass to the left, hi-pass to the right)
- `Flanger`
- `RingMod`
- `Robot`
- `Stutter`
- `Wah`

The engine may choose to implement additional effects.
