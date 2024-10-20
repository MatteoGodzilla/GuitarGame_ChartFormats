# megamix.ini DJ Hero Metadata Specifications (Proposal)

This document describes the metadata that is used to define a Megamix for DJ Hero with all the informations needed to correctly play them in future engines.

## Table of Contents
- [megamix.ini DJ Hero Metadata Specifications (Proposal)](#megamixini-dj-hero-metadata-specifications-proposal)
  - [Table of Contents](#table-of-contents)
  - [Background / What are Megamixes in DJ Hero 2](#background--what-are-megamixes-in-dj-hero-2)
  - [Folder structure](#folder-structure)
    - [Example](#example)
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