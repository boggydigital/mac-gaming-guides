Setting up and running BUILD engine games with Raze
===================================================

This guide will help you run [BUILD engine](https://en.wikipedia.org/wiki/Build_(game_engine)) games on macOS.

Games you should be able to run:
- [Duke Nukem 3D](#duke-nukem-3d)
- [Blood](#blood)
- [Shadow Warrior](#shadow-warrior)
- [and many others!](#https://en.wikipedia.org/wiki/Build_(game_engine)#List_of_Build_Engine_games)

NOTE: Please follow a guide for [Ion Fury](ion_fury.md) as it's [not supported by Raze](https://github.com/ZDoom/Raze/commit/ce269808dcdb930dcabf4ce7eb3649d00693df44#commitcomment-44822488).

## Setting up Raze

To run BUILD engine games you'll need to use Raze source port, prepare game data and configure Raze.

### Get Raze

1. Grab the [latest available release](https://github.com/ZDoom/Raze/releases), likely named like `raze-macos-<version>.zip`.
2. Unpack zip file and move `Raze.app` to your Application folder

Alternatively, if you've got `Homebrew` installed, you can use the following command to install Raze: `brew install raze`

### Run Raze.app at least once

Before you continue configuration you'll want to run Raze at least once to create a configuration file.

1. Navigate to your applications folder in Finder, typically `/Applications`
2. Open `Raze.app`

### Prepare game data

Download game data from [Steam](../common/steam.md) or [GOG](../common/gog.md). Please refer to [BUILD engine games](#build-engine-games) section for specifics.

Most likely you'll want to consolidate game data in a special folder, for example `/Users/<your-user-name>/BUILD` or any other folder. Please note the folder where your game data is located - this will come handy in the next step.

### Configure Raze to find your game data

1. Open `Finder`
2. Press `Command + Shift + G` or select `Go` > `Go to Folder...`
3. Type `~/Library/Preferences`
4. Locate and open `raze.ini`
5. Find `[GameSearch.Directories]` section
6. Add the [folder](#prepare-game-data) you're using to store BUILD engine game data

Alternatively, note the folders already configured and put your game data in one of those folders.  

### Run Raze, select and play your game

1. Navigate to your applications folder in Finder, typically `/Applications`
2. Open `Raze.app`
3. Observe the list of the games presented similarly to this:

| IWAD    | Game                          |
|---------|-------------------------------|
| DUKE3D  | Duke Nukem 3D: Atomic Edition |
| BLOOD   | BLOOD: One Unit Whole Blood   | 
| Sw      | Shadow Warrior                |
| REDNECK | Redneck Rampage               |
| STUFF   | Exhumed                       |

4. Select the desired game and press `OK`

If you don't see any games in the list, most likely you'll need to [configure Raze to find your game data](#configure-raze-to-find-your-game-data). Refer to [troubleshooting](#troubleshooting) section for some recommendations.

## BUILD engine games

### Duke Nukem 3D

GOG.com no longer has `Duke Nukem 3D` for sale. You can however find it in your account if you purchased it before it was delisted.

Steam app-id for `Duke Nukem 3D: 20th Anniversary World Tour`: `434050`.

### Blood

GOG.com lists [Blood: Fresh Supply](https://www.gog.com/en/game/blood_fresh_supply) that seems to include `Blood: One Unit Whole Blood - Windows` as an extra. It seems like you can use either version.

Steam app-id for `Blood™ Fresh Supply`: `1010750`

#### Death Wish for Blood mod

- Download the latest version from [ModDB](https://www.moddb.com/mods/death-wish-for-blood/downloads)
- Move the zip file you've downloaded to the folder you store your `Blood` game data (e.g. `/Users/<your-user-name>/BUILD/Blood`)
- Run `Raze.app`
- In the `Additional Parameters:` add full path to the zip file you've placed with Blood game data, e.g. `-file "/Users/<your-user-name>/BUILD/Blood/Death_Wish_<version>.zip"`
- Select `BLOOD` from the list of games

If everything goes well - you'll see `Death Wish` menu screen. 

However, if nothing happens, you might have to do some extra work:

- Delete everything in `Additional Parameters:`
- Select `BLOOD` and launch again

See this [issue comment](https://github.com/ZDoom/Raze/issues/142#issuecomment-739335715) for more details.

### Shadow Warrior

GOG.com lists:
- `Shadow Warrior Classic Redux`
- `Shadow Warrior Classic Complete`

Steam app-id for `Shadow Warrior Classic Redux`: `225160`

## Troubleshooting

As you've launched Raze - notice that it opened two windows. Check out `Raze <version> - Console` window that would list some log messages.