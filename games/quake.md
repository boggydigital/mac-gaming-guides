Running Quake on macOS
======================

This guide will help you get [Quake](https://en.wikipedia.org/wiki/Quake_(video_game)) up and running on macOS.

## Getting game data

Whether you'll get game data from Steam or GOG.com, we'll assume you'll have them located at `~/Downloads/quake`.

### Get Quake Enhanced Version from Steam

On August 19, 2021 Quake [has been updated](https://store.steampowered.com/news/app/2310/view/2945904318371492362) to the enhanced version on Steam.

To get game data for this version, [download the game from Steam](../common/steam.md) and use app-id: `2310`.

### Get Quake Enhanced from GOG.com

On December 15, 2022 Quake has been updated to the enhanced version on GOG. You have a choice to download enhanced (called `Quake Enhanced`) or original version of Quake (called `Quake`) [from your account](../common/gog.md).

## Run Quake on macOS

To run Quake on macOS you'll need one of the source ports:

1. [vkQuake](https://github.com/Novum/vkQuake)
    - NOTE: [Mac Source Ports](https://macsourceports.com/game/quake) provides Universal binaries for Intel and Apple Silicon
2. [QuakeSpasm](http://quakespasm.sourceforge.net)

### Using vkQuake to play Quake 

1. Open Terminal.app
2. Go to vkQuake app executable directory: `cd /Applications/vkQuake.app/Contents/MacOS`
3. For the original version:
    - Run `./vkquake -basedir /Users/<your-username>/Downloads/quake`
    - NOTE: If you downloaded game data into a directory, that's not [~/Downloads/quake](#Getting-game-data) - you'll need to adjust to the actual location of the game files.
4. For the enhanced version:
    - `./vkquake -basedir /Users/<your-username>/Downloads/quake/rerelease`

This base command will launch vkQuake with original game data and is applicable to both Steam and GOG.com versions.

### Using QuakeSpasm to play Quake

1. Launch QuakeSpasm
2. Use `Settings > Command line parameters` to specify launch options:
    - Original version: `-basedir /Users/<your-username>/Downloads/quake`
    - Enhanced version: `-basedir /Users/<your-username>/Downloads/quake/rerelease`

## Using source ports to play Quake official add-ons (mission packs)

Modify the command that specifies `basedir` and add `game` parameter, it should look like this:

```
-basedir <path-to-original-or-enhanced-basedir> -game <game-id>
```

Quake original version ships with the following add-ons game-ids:

- `hipnotic`: [Quake Mission Pack No. 1: Scourge of Armagon](https://en.wikipedia.org/wiki/Quake_(video_game)#Quake_Mission_Pack_No._1:_Scourge_of_Armagon)
- `rogue`: [Quake Mission Pack No. 2: Dissolution of Eternity](https://en.wikipedia.org/wiki/Quake_(video_game)#Quake_Mission_Pack_No._2:_Dissolution_of_Eternity)

Quake Enhanced additionally ships the following add-ons:

- `dopa`: [Dimension of the Past](https://en.wikipedia.org/wiki/Quake_(video_game)#Dimension_of_the_Past)
- `mg1`: [Dimension of the Machine](https://en.wikipedia.org/wiki/Quake_(video_game)#Remastered_Edition_and_Dimension_of_the_Machine)

## Using source ports to play Quake mods

Here is the general approach to playing Quake mods, using [Arcane Dimensions](https://www.moddb.com/mods/arcane-dimensions) as an example:

1. Download desired mod archive, e.g. `ad_v1_80p1final.zip`
2. Create a folder for this add-on under your Quake game data folder and unpack it there, e.g. `ad` under `~/Downloads/quake`
3. Use `ad` as game-id, with the resulting command line parameters looking like this: `-basedir /User/<you-username>/Downloads/quake -game ad`

NOTE: I'm not aware of any particular benefit of using enhanced version over original for mods, so the example uses original version basedir and should work for Steam and GOG.com versions.

## Configuring Quake

The recommended way to add certain parameters to Quake games is creating new `autoexec.cfg` file under the game folder (e.g. `~/Downloads/quake` or `~/Downloads/quake/rerelease/mg1`).

[This Quake.blog article](https://quake.blog/configuring-quakespasm-for-a-modern-retro-look.html) has some recommendations to get you started. And [this Quaddicted article](https://www.quaddicted.com/engines/software_vs_glquake) explains the difference in rendering between different engines and provides more recommendations.
