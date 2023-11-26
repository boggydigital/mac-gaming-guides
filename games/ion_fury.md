Running Ion Fury on macOS
=========================

This guide will help you get [Ion Fury](https://en.wikipedia.org/wiki/Ion_Fury) up and running on macOS.

**Nov 26 2023**: Added [Ion Fury: Aftershock](#running-ion-fury-aftershock-on-macos) section.

## Getting game data

Whether you'll get game data from Steam or GOG.com, we'll assume you'll have them located at `~/Library/Application Support/EDuke32`.

### Get Ion Fury from Steam

To get game data for this version, [download the game from Steam](../common/steam.md) and use app-id: `562860`.

### Get Ion Fury from GOG.com

To get Ion Fury game data, [download the game from GOG.com](../common/gog.md).

## Run Ion Fury on macOS

To run Ion Fury on macOS you'll need one of the source ports:

1. [EDuke32 from MacSourcePorts](https://macsourceports.com/sourceport/eduke32)
    - NOTE: First-party [EDuke32 macOS builds](https://dukeworld.com/eduke32/mac/) have not been updated since Oct 09 2021 and the last build is not compatible with Ion Fury 2.0 update.

### Using EDuke32 to play Ion Fury

1. Launch `EDuke32.app`
2. Assuming you have game files (fury.grp, fury.grpinfo) in `~/Library/Application Support/EDuke32` you should see Name: `Ion Fury`, File: `fury.grp` in the `Game:` section.
2. See [configuring & troubleshooting](#configuring--troubleshooting-ion-fury-on-macos) for renderer, color modes and resolution you'll need to set up for the first run.
3. Select `Start` to run the game.

## Configuring & troubleshooting Ion Fury on macOS

### Renderer & Color modes

EDuke32 supports `Classic`, `Polymost`, `Polymer` render modes. This can be implictly set before starting by selecting the corresponsing color depth (`8-bpp`: `Classic`, `32-bpp`: `Polymost`).

Additionally this can be configured in game: `Options` > `Display Setup` > `Video Mode` > `Renderer:`.

At the moment you'll need to select `Classic` renderer, `8-bpp` color depth. `32-bpp` modes - `Polymost`, `Polymer` will have unplayable performance. Given that Ion Fury uses [BUILD engine](https://en.wikipedia.org/wiki/Build_(game_engine)) aestetics, that's not a significant comprimise. The most obvious problem is incorrect perspective on vertical mouse look up/down.

### Resolution

Since we'll need to use `Classic` renderer that uses CPU, not GPU rendering - you'll likely want a sub-Retina, integer scale resolution, e.g. `1920 x 1080 8-bpp` for 4K screen. 

In addition to lauch options, you can configure resolution in game: `Options` > `Display Setup` > `Video Mode` > `Resolution:`.

Another alternative is to use built-in upscaling. For that you'll need to use your desired resolution (e.g. `3840 x 2160`) and set `2x` upscaling in game: `Options` > `Display Setup` > `Upscaling` > `2x`.

### Troubleshooting EDuke32 game data lookup

You can use `Message Log` tab in the main `EDuke32` window to check what locations are used for game data lookup:

```text
Using /Users/{current user name}/Library/Application Support/EDuke32/ for game data
Using /Applications/EDuke32.app/Contents/Resources/ for game data
Using /Users/{current user name}/.config/eduke32/ for game data
```

## Running Ion Fury: Aftershock on macOS

At the moment - the only viable option to run Ion Fury: Aftershock is some sort of Windows emulation. 

[CrossOver](http://codeweavers.com/crossover) or [Whisky](https://github.com/Whisky-App/Whisky) run the game just fine as long as you use software rendering. Using 32-bit rendering (Polymost) will make the game increadibly slow on macOS (see [Renderer & Color modes](#renderer--color-modes)).

Running Ion Fury: Aftershock in software mode takes a bit of effort though, as the Aftershock game update specifically removed UI options to enable software rendering. Here's how to make it work:

- Use a console command `setrendermode 0` in the game ([source](https://www.gog.com/forum/ion_fury/ion_fury_aftershock_without_support_for_software_mode_wtf/post2))

Alternatively:

- Navigate to a bottle where you've installed the game, and then to `users/crossover/AppData/Roaming/Ion Fury`
- Open `fury.cfg`
- Under `[Screen Setup]` change `ScreenBPP = 32` to `ScreenBPP = 8`
- Save the file and run the game again (don't change the resolution in the starting dialog!)

### Other options that might work in the future

None of the options below are viable at the time of writing this guide, keeping them listed in case that changes:

- [EDuke32 from MacSourcePorts](https://macsourceports.com/sourceport/eduke32) has been last updated on August 25, 2022 and doesn't support the latest Ion Fury update and Aftershock (script parsing error on launch)
- [Official EDuke32 macOS build](https://dukeworld.com/eduke32/mac/) have not been updated since Oct 09 2021 and don't support Aftershock (or earlier Ion Fury updates)
