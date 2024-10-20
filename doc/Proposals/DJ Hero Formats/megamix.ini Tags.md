# megamix.ini DJ Hero Metadata Specifications (Proposal)

This document describes the metadata that is used to define a Megamix for DJ Hero with all the informations needed to correctly play them in future engines.

## Table of Contents
- [megamix.ini DJ Hero Metadata Specifications (Proposal)](#megamixini-dj-hero-metadata-specifications-proposal)
  - [Table of Contents](#table-of-contents)
  - [Background / What are Megamixes in DJ Hero 2](#background--what-are-megamixes-in-dj-hero-2)
  - [Anatomy of a Megamix](#anatomy-of-a-megamix)
    - [First mix](#first-mix)
    - [Middle mixes](#middle-mixes)
    - [Ending mix](#ending-mix)
    - [Example](#example)
  - [Folder structure](#folder-structure)
    - [Example](#example-1)
  - [megamix.ini Tags](#megamixini-tags)
    - [Megamix](#megamix)
    - [Setlist](#setlist)
  - [megamix.ini Example](#megamixini-example)


## Background / What are Megamixes in DJ Hero 2
In DJ Hero 2 a Megamix is a set of mixes that are played in sequence, without stopping the music, in order to appear to the player as a single continuous mix.

Megamix are composed of two types of mix:normal and transition mixes.
Normal mixes are the kind of mix that can be played by themselves by the player, so they appear for example in the Quick Play menu in DJ Hero 2.
On the other hand, a transition mix is a kind of mix that is used in order to transition (hence the name) from a normal mix to another in a megamix. These are often specially crafted to work on just a single megamix and they do NOT appear under Quick Play in DJ Hero 2.

Megamixes have two star counters: one that is showing the star rating of the currently playing mix and a cumulative star rating for the whole Megamix.
This is because when entering a new normal mix the current star rating is reset, while the cumulative is not. Note that this does NOT happen for transition mix, which in the context of star rating they get merged into the previous normal mix.

## Anatomy of a Megamix
As previously defined, a Megamix is a set of mixes that are played in sequence, without stopping the music. This definition (for clarity sake) is not 100% exact.
A Megamix is a set of **part of** mixes that are played in sequence, without stopping the music.
This is because sometimes for musical and gameplay reasons a Megamix should switch from a mix to another seamlessly *without hearing the outro of the previous mix*.

For these reasons there are specific global events for Megamixes defined in the [.chart specification](./chart%20Format.md), with the goal of adding specific timestamps to use as "markers" so that parts of different mixes can be extracted from the full mixes and then glued together to form a coherent Megamix.

### First mix
In DJ Hero 2 there is a quirk about the first mix in a Megamix: it is used both as the first mix to be played by the player, but it **may** also contains the Megamix Intro before that. This leads to the first mix having a structure that in the most complicated case looks like the following diagram:
```
| Skippable opening | Extended Megamix intro | Playable Section | Mix outro |
0                   A                        B                  C          End

(Diagram 1)
```

| Section                | Required? | Description                                                                                                                                                                                                                 |
| :--------------------- | :-------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Skippable opening      |    no     | This is an audio-only portion of the mix that builds up the start of the Megamix.<br>Ideally there should be a way to skip this section and jump straight to the Extended Megamix intro (A), or to the Standalone Chart (B) |
| Extended Megamix Intro |    no     | This is a playable section of the chart that should only appear while playing the Megamix, and it leads to the Standalone Chart                                                                                             |
| Playable Section       |    yes    | This is the section of the mix that should be played when the player is NOT playing the Megamix but rather just playing this mix alone.                                                                                     |
| Mix Outro              |    no     | This is a section that should only be played when the player is NOT playing the Megamix. In the case of Megamixes, this section is cut out and the next mix is put instead.                                                 |

In Diagram 1, `A`, `B` and `C` refer to global events that must be placed in the mix, using the ones defined in the [.chart specification](./chart%20Format.md).
The types of events differ depending if the first mix contains a Megamix intro or not.

In the case the first mix **contains both the Skippable opening and Extended Megamix intro**:
| Marker |       Event type        |
| :----: | :---------------------: |
|  `A`   |   `dj_megamix_intro`    |
|  `B`   | `dj_megamix_intro_end`  |
|  `C`   | `dj_megamix_transition` |

In the case the first mix **only contains the Skippable opening**, then the `A` and `B` markers refer to the same time point:
|  Marker   |       Event type        |
| :-------: | :---------------------: |
| `A` / `B` | `dj_megamix_intro_end`  |
|    `C`    | `dj_megamix_transition` |

In the case the first mix **does NOT contain anything before the Standalone Mix**, then there is no point `A` and the events are as following:
| Marker |       Event type        |
| :----: | :---------------------: |
|  `B`   | `dj_megamix_intro_end`  |
|  `C`   | `dj_megamix_transition` |

### Middle mixes
A mix in the middle of the Megamix that is preceded and followed by a mix. The two transition points define the start of this mix and when it should transition to the mix following this one.
```
Middle: | Skipped start | Playable section | Skipped end |
        0               A                  B            End
```
| Marker |       Event type        |
| :----: | :---------------------: |
|  `A`   | `dj_megamix_transition` |
|  `B`   | `dj_megamix_transition` |

### Ending mix
This is the last mix in the megamix. There is only one marker, which defines when this mix should start, because this mix is played in its entirety.
```
Ending: | Skipped start | Playable section |
        0               A                 End
```
| Marker |       Event type        |
| :----: | :---------------------: |
|  `A`   | `dj_megamix_transition` |

### Example
The following example is taken from the [first Megamix](https://www.youtube.com/watch?v=sSiU1qxve-4) in DJ Hero 2 (Ibiza opening night). All credits to Luke Snywalker for the video, permission to use the video for this document was granted.

Here is the Megamix shown in the video as a diagram:
```
Sections: | Skippable Opening | Extended Intro | The Message vs. Jungle Boogie | Bridge | Informer vs. ABC | Groove Is In The Heart vs. Le Freak |
Marker:   0                   A                B                               C        D                  E                                    End
Timestamp:| 0:00              | 0:18           | 0:32                          | 2:40   | 3:25             | 6:30                                |
```

Here is a table showing all the required events for each mix:
| Mix                                 |        `A`         |          `B`           |           `C`           |           `D`           |  `E`  |
| :---------------------------------- | :----------------: | :--------------------: | :---------------------: | :---------------------: | :---: |
| The Message vs. Jungle Boogie       | `dj_megamix_intro` | `dj_megamix_intro_end` | `dj_megamix_transition` |            -            |   -   |
| Bridge                              |         -          |           -            | `dj_megamix_transition` | `dj_megamix_transition` |   -   |
| Informer vs. ABC                    |         -          |           -            | - | `dj_megamix_transition` | -
| Groove Is In The Heart vs. Le Freak |         -          |           -            | - | - | `dj_megamix_transition`


Note: Informer vs. ABC is defined to be played until the end, so there is no second `dj_megamix_transition` event placed in `E`

## Folder structure
A Megamix is defined in the filesystem as a directory containing a `megamix.ini` file and one or more subdirectories, each one for a mix that is included in the megamix. Every subdirectory contains all the required files to play that mix.
For transition mix it is not necessary to have a `song.ini` file since they are not shown ingame and so they don't require additional metadata

### Example
```
Megamix/
- megamix.ini
- mix 1/
  - song.ini
  - notes.chart
  - cover.png
  - ...
- transition 1-2/
  - notes.chart
  - ...
- mix 2/
  - song.ini
  - notes.chart
  - cover.png
  - ...
```

## megamix.ini Tags
### Megamix
All the following tags must be placed under the Megamix section of the megamix.ini file

| Tag Name  | Description                                                          | Data type |
| :-------- | :------------------------------------------------------------------- | :-------: |
| `name`    | The name of the Megamix.                                             |  string   |
| `dj`      | The dj(s) that made the mixes and transitions, separated by a comma. |  string   |
| `charter` | The charter(s) that charted this megamix, separated by a comma.      |  string   |

### Setlist
This section contains a list of properties of the form `<order> = <subdirectory>`, where:
- `<order>` is an integer number starting from 1 that defines in what order the mixes should be played
- `<subdirectory>` is a string path that points to the directory containing all files for the mix

## megamix.ini Example
This example corresponds to the directory shown under [Folder Structure/Example](#example)
```ini
[Megamix]
name = "Super iper Megamix"
dj = "foo, bar, baz"
charter = "richard, jeremy, james"

[Setlist]
1 = "mix 1"
2 = "transition 1-2"
3 = "mix 2"
```