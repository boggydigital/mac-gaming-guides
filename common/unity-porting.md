How to port Windows, Linux and 32-bits macOS Unity games to run on 64-bits macOS 
================================================================================

These guides will teach you how to run *Windows, Linux and even 32-bits macOS Games that are made in Unity* on macOS Catalina or above.


- [From 32-bits to 64-bits macOS](#from-32-bits-to-64-bits-macos) 
- &nbsp;&nbsp;&nbsp;[Download and install the game](#download-and-install-the-game)
- &nbsp;&nbsp;&nbsp;[Determine Unity version](#determine-unity-version)
- &nbsp;&nbsp;&nbsp;[Download Unity](#download-unity)
- &nbsp;&nbsp;&nbsp;[Prepare standalone player files](#prepare-standalone-player-files)
- &nbsp;&nbsp;&nbsp;[Update the game](#update-and-run-the-game)
- [From Windows/linux to macOS](#from-windowslinux-to-macos)
- &nbsp;&nbsp;&nbsp;[Getting game data](#getting-game-data)
- &nbsp;&nbsp;&nbsp;[Common steps with the 32-bits to 64-bits guide](#common-steps-with-the-32-bits-to-64-bits-guide)
- &nbsp;&nbsp;&nbsp;[Preparing needed Unity Player](#preparing-needed-unity-player)
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Old Unity versions](#preparing-unity-player-old-unity-versions)
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Modern Unity versions](#preparing-unity-player-modern-unity-versions)
- &nbsp;&nbsp;&nbsp;[Porting the game](#porting-and-running-the-game)
- [Troubleshoot common problems](#troubleshoot-common-problems)


# From 32-bits to 64-bits macOS

**Thank you [M0REKZ](https://github.com/M0REKZ) for the [original version of the 32-bits porting guide](https://github.com/M0REKZ/PortingMacGames/blob/Principal/Unity/Unity%20Porting%20Guide.md) I've reformatted and detailed here!**

In 2019 Apple has released macOS Catalina that only [supported 64-bit apps](https://en.wikipedia.org/wiki/MacOS_Catalina#Removed_or_changed_components). With that change many older Unity games that shipped 32-bit Unity Player stopped working.

This guide will provide a step-by-step instruction on updating the game to 64-bit Unity Player to allow them to run on macOS versions post Catalina, as well as on [Apple Silicon](https://en.wikipedia.org/wiki/Apple_silicon) devices through [Rosetta 2](https://en.wikipedia.org/wiki/Rosetta_(software)#Rosetta_2).

Sidenote: Maybe using extra steps, you could make the game at PowerPC if its enough old, and also if the game is 64-Bits and above Unity 2020.2, in some cases you can make it work on arm64 native.

## Download and install the game

Use any of the supported methods to download and install GOG.com, Steam or any other store version on your device.

### GOG.com notes

With the 32-bit apps deprecation GOG.com doesn't list macOS support for those older games anymore (e.g. [else Heart.Break()](https://www.gog.com/en/game/else_heartbreak)). I'm not 100% sure you'll get a macOS version if you buy them today.

However, if you've got them earlier, those games still have 32-bit macOS version available in your account. 

It's also likely those older games use GOG.com older packaging format (installer comes as a `.dmg` file), and you will need to perform additional steps to get ready for the update:

NOTE: if a game is a `.pkg` installer, most likely, you don't need to do any of that and you'll get a game package without GOG.com wrapper.

- Mount `.dmg` file in Finder
- Drag the game icon to the Application folder (or anywhere else)
- Locate the game icon at this new location and Right-click / Ctrl+click, then select `Show Package Contents`
- Navigate to `Contents`/`Resources`/`game` and you'll see the actual game package there (it seems like the rest of original package is GOG.com specific wrapper)
- Drag that game package out (NOTE: often this package will have the same name as the GOG.com package, so you might need to drag somewhere else, like `~/Downloads`)
- Congratulations! - now you have the game package ready to be updated! Feel free to discard that GOG.com package wrapper if you don't need it anymore.

## Determine Unity version

### Method 1: Checking App Info

- Navigate to the folder where the game is installed
- Right-Click the App and select Get Info (Or also Click the app only one time and press Command + I in your keyboard)
- A window will show up with the info for the Application, including Unity player version
- Optionally, instead you can Click the app one time and then press Space, that will show you Information about the app, including Unity Player version.

### Method 2: Checking Info.plist

If for some reason the System is not showing app version, you can try to Check Info.plist file.

- Navigate to the folder where the game is installed
- Locate the game icon at this new location and Right-click / Ctrl+click, them select `Show Package Contents`
- Open `Contents`/`Info.plist` (NOTE: you might use `TextEdit.app` or any other text editor)
- Look for the string that starts with: `Unity Player version X.Y.Z`, for example: `Unity Player version 5.6.4p4 (72f24c04957f). (c) 2017 Unity Technologies ApS. All rights reserved.`
- That version is what we need, in the example above that's `5.6.4p4` (NOTE: Unity added 64-bit support at version 4.2, so games before that can't be updated)

### Method 3: Checking level0 file

In case that Unity Version is not listed at Info.Plist, you could try checking the `level0` file.

- Start [`Terminal.app`](https://support.apple.com/guide/terminal/welcome/mac)
- Navigate to game `Data` folder, e.g. `cd /Applications/<UnityGame>/Contents/Resources/Data`
- Run the following command: `strings level0 | head -1`

Alternatively:

- With `Finder`, navigate to game `Data` folder, e.g. `/Applications/<UnityGame>/Contents/Resources/Data`
- Open `level0` file in a text editor of your choice and observe the first readable string, e.g. `5.6.4p4`

## Download Unity

Find an installer for the correct Unity version:

### Method 1: Check known download URLs

Check out if [a list of known Unity version installers](../games/unity.md) contains the version that you need.

### Method 2: Build a URL

Try to construct a URL like `http://download.unity3d.com/download_unity/unity-<version>.dmg`, where `<version>` is `X.Y.Z`, e.g. `4.7.0`. That seems to work well for earlier non-patch versions (e.g. `4.7.0f1` -> `http://download.unity3d.com/download_unity/unity-4.7.0.dmg`) 

### Method 3: Use search engine

Use your general search engine of choice and try `Unity <version>` (e.g. `Unity 5.6.4p4`)

### Method 4: Use archive.org

Use `archive.org` and search for `https://download.unity3d.com/download_unity/`, switch to `URL` results and type verion into `Filter results by URL or MIME Type` search field:
    
- Look for URLs ending with `MacEditorInstaller/Unity-<version>.pkg`, e.g. `https://download.unity3d.com/download_unity/ac7086b8d112/MacExampleProjectInstaller/Examples-5.6.4f1.pkg`
- You don't need to download through `archive.org` - just copy `download.unity3d.com` URL and paste it to initiate download from Unity

### Method 5: Try Windows / Linux Unity versions

If you absolutely cannot find required macOS version of Unity - check if you can locate Windows or Linux versions. Just like macOS version comes with Windows / Linux standalone players - they might contain macOS standalone players. 

SECURITY NOTICE: We strongly recommend downloading Unity installers **only** from `download.unity3d.com` website and not from any third-party websites. From our experiments - all version are available there, some need just a bit more search than others (e.g. `X.Y.Zp` patch releases).

IMPORTANT NOTE: You need a precise Unity Player version matching game data files requirement or the game won't run. Luckily, when attempted to start with a wrong player version, the game will clearly indicate required version, and you should be able to fix that problem.

## Prepare standalone player files

The simplest approach is to install Unity you've just downloaded. This might require several steps, and the process is generally pretty straightforward. You'll end up with `Unity` folder in your `Applications` folder. If so - you can skip the next section that achieves the same result without installation.

### Extract the player without installing

In case you've got a `.pkg` Unity installer - you can use `pkgutil` to extract the files:

- Start [`Terminal.app`](https://support.apple.com/guide/terminal/welcome/mac)
- Navigate to the location of the installer (e.g. `cd ~/Downloads`)
- Run `pkgutil --expand Unity-<version>.pkg ./unity` (e.g. `pkgutil --expand Unity-5.6.4f1.pkg ./unity`)
- Using `Finder`, navigate to the folder you've extracted it to (e.g. `~/Downloads`/`unity`) and look for the `Payload` file (e.g. `Unity.pkg.tmp`/`Payload`)
    - In some cases you will find another package inside named `Unity.pkg`, but to solve this you can just do Right-Click and select `Show Package Contents`
- Actually the `Payload` file is a zip, so you can Right-Click it and select `Open With -> Archive Utility` to unpack it (or just rename it to `Payload.zip`)
- A new folder containing `Unity.app` will appear (e.g. `Unity`/`Unity.app`)

Otherwise, if for any reason you got a windows installer (`.exe`), you may need a tool like 7-zip to extract its content

Example with 7-zip:

- Start [`Terminal.app`](https://support.apple.com/guide/terminal/welcome/mac)
- if you haven't done it yet, you can install 7-zip using [Homebrew](https://brew.sh) with `brew install sevenzip`
- Navigate to the location of the installer (e.g. `cd ~/Downloads`)
- Create a new Folder `mkdir UnityInstallerDir`
- Run `7zz x <path-to-windows-unity-installer> -o./UnityInstallerDir` (You may replace "UnityInstallerDir" with the name of the folder you created)
- Due to this is a Windows installer, the Playback Engine location would be different from the Mac Installer, so maybe you will need to search it.

NOTE: This wont work with all windows installers

### Get standalone playback engine

Regardless of how you've got `Unity`/`Unity.app` - either by installing or extracting:

- Navigate to the location of `Unity.app` in `Finder`
- Right-click / Ctrl+click, them select `Show Package Contents`
- Navigate to `Contents`/`PlaybackEngines`/`MacStandaloneSupport`/`Variations`
- Carefully observe folder names in that location:
    - You'll need `macosx64_nondevelopment_mono` folder
    - NOTE: you likely don't want to use `universal_nondevelopment_mono` as that refers to Universal 1 (PowerPC / Intel), not Universal 2 (Intel / Apple Silicon) for older installers
- While in `macosx64_nondevelopment_mono` folder, locate `UnityPlayer.app` and Right-click / Ctrl+click, then select `Show Package Contents`
- Navigate to `Contents`
- Ultimately you need two artifacts:

  - `Contents`/`Frameworks` folder (it should contain `MonoEmbedRuntime`/`osx`)
  - `Contents`/`MacOS`/`UnityPlayer` binary

- Keep them ready or copy to some more convenient location, either way - you're ready for the next step!

- (Optional) [Verify that you've got the correct standalone player version](#validating-that-youve-downloaded-the-correct-standalone-player-version)

## Update and run the game

IMPORTANT NOTE #1: Make a backup copy of the game package, so that you won't need to re-download/re-install if things go wrong.

- Locate and prepare the game package (see [Download and install the game](#download-and-install-the-game)):

  - Right-click / Ctrl+click, them select `Show Package Contents`
  - Observe `Contents`/`Frameworks` and `Contents`/`MacOS`/`<GameTitle>` binary there

- Locate and prepare Unity standalone playback engine (see [Prepare standalone player files](#prepare-standalone-player-files))

- Copy files from Unity standalone playback engine `Contents`/`Frameworks` to the original game `Contents`/`Frameworks` - e.g. `Contents`/`Frameworks`/`MonoEmbedRuntime`/`osx`/`*.dylib`
- IMPORTANT NOTE: Don't replace original game entire`Contents`/`Frameworks` folder, because it may contain other required files, not provided by Unity standalone playback engine!
- Copy/Move Unity standalone playback engine `Contents`/`MacOS`/`UnityPlayer` binary to original game `Contents`/`MacOS`
- Rename/Delete original game `Contents`/`MacOS`/`<GameTitle>` binary and rename `Contents`/`MacOS`/`UnityPlayer` -> `Contents`/`MacOS`/`<GameTitle>` (NOTE: use the exact binary name you had earlier!)
- Assuming everything went well - you're done! Congratulations - now your game package is ready to run on 64-bit macOS.
- Navigate to the folder containing your game package and run it (ignore the circle with a line crossing that indicates game compatibility problem - it'll disappear once you successfully launch the game)
- NOTE: macOS would likely not allow you to *just run* the game on the first try - and you will need to Right-click / Ctrl+click on the game and select `Open` then confirm your desire to run the game from unknown developer.
- (Optionally) Clean-up installed or extracted Unity, any installers you've downloaded, etc.

# From Windows/Linux to macOS

Guide made by [M0REKZ](https://github.com/M0REKZ) using information from:

- [PCGamingWiki](https://pcgamingwiki.com/wiki/Engine:Unity/Porting)
- [Script to Port Linux version of Valheim](https://reddit.com/r/macgaming/comments/1217lko/)
- [Same Unity 32-bit Guide from above](#from-32-bits-to-64-bits-macos)
- [Porting a Windows Unity 4 Game](https://reddit.com/r/macgaming/comments/10ogws9/)

If you can run Unity Windows games on Linux, Technically you could also do the same on Mac (In fact, the first guide was made following a linux one), however, since there are some dependences that are for Windows and Linux only (Like DirectX), only a selected amount of games are known to work with this method, also people does not have knowledge or interest in doing that due to same thing, so, this second section will teach you how to run games from other Platforms as if were a Native Apple Silicon/Intel macOS game.

Also, most of the Steps are similar with the 32-bits to 64-bits guide, so make sure to check the other one when/if needed.

Sidenote: Technically, you could port Unity games from other different platforms, however, file paths will be different, so only windows and linux versions are covered as example on this guide to make it be simple.

## Getting game data

If you cant download the game normally, you should check these guides for that:

- [How to get Game Data from GOG](./gog.md)
- [How to get Game Data from Steam](./steam.md) IMPORTANT NOTE: Most Steam games uses DRM, and when you launch them, you will get a Invalid Platform Error, *ITS HIGHLY RECOMMENDED to get game data from stores like GOG* instead.
- [How to get Game Data from Epic Games](./epic-games.md)

Otherwise you should try other methods like Virtual Machines, Wine, etc...

## Common steps with the 32-bits to 64-bits guide

Make sure to go back to this guide when you finish these steps:

- [Determine Unity version (Method 3 only)](#method-3-checking-level0-file): You must install the exact same version, otherwise you will get an error, also, due to the game is not a mac app, only you can check the level0 file.
- [Download Unity](#download-unity): You would prefer to download a Mac installer if possible.
- [Extract Unity Without installing](#extract-the-player-without-installing): You could also install Unity like normally, but its not recommended, specially if you are thinking to port multiple games.

## Preparing needed Unity Player

Regardless of how you've got `Unity`/`Unity.app` - either by installing or extracting:

- Navigate to the location of `Unity.app` in `Finder`
- Right-click / Ctrl+click, them select `Show Package Contents`
- Navigate to `Contents`/`PlaybackEngines`/`MacStandaloneSupport`

At some time Unity Player File structure changed a little, so this section will be splitted into 2 subsections.

### Preparing Unity Player (Old Unity versions)

If you dont see a `Source` Folder at here, you downloaded an old Unity version, otherwise, go to [Preparing Unity Player (Modern Unity versions)](#preparing-unity-player-modern-unity-versions)

- Navigate to `Variations` Folder
- Carefully observe folder names in that location:
    - You'll need `macosx64_nondevelopment_mono` folder
    - NOTE: you likely don't want to use `universal_nondevelopment_mono` as that refers to Universal 1 (PowerPC / Intel), not Universal 2 (Intel / Apple Silicon) for older installers
- While in `macosx64_nondevelopment_mono` folder, locate `UnityPlayer.app` and copy it to a convenient location.
- Right-click / Ctrl+click the App, then select `Show Package Contents`

Now, assuming that you actually [have the game data](#getting-game-data):

- Open a new Finder Window (Right-click / Ctrl+click Finder and Select `New Finder Window`)
- Navigate to the game folder, there must be
   - The game executable `gamename.exe`
   - And game data `gamename_Data`

Now we can start porting the game, go to [Porting and running the game](#porting-and-running-the-game)

### Preparing Unity Player (Modern Unity versions)

In case you find a `Source` folder Inside, you downloaded a modern Unity version, otherwise go to [Preparing Unity Player (Old Unity versions)](#preparing-unity-player-old-unity-versions)

Modern Unity Player is a little bit more complicated than the old ones, but actually you can get some games working, also, in case that the game is made on `Unity 2020.2 Alpha 21` or above, it *could run native on apple silicon*. (NOTE: due to at `2020.2 alpha 21` Apple Silicon support was very initial, you must expect a LOT of bugs, in that case you better try both Intel 64 Bits/Apple Silicon Players)

- Navigate to `Variations` Folder
- Carefully observe folder names in that location, depending of Plugin architecture support or the early Apple Silicon Support, you can select one of these:
    - You'll prefer `macos_x64_player_nondevelopment_mono` folder in case you are using an Intel Mac, or some of the plugins dont have Apple Silicon Support.
    - Otherwise, you would prefer `macos_arm64_player_nondevelopment_mono` for better perfomace on M1 Mac
    - The folder `macos_x64arm64_player_nondevelopment_mono` contains a Universal 2 Unity Player (for Intel and M1), may be useful in certain situations
    - NOTE: Folder names may vary at different Unity versions
- While in any of these folders, locate `UnityPlayer.app` and copy it to a convenient location.
- Right-click / Ctrl+click the App, then select `Show Package Contents`
- IMPORTANT: Make sure that these files are inside, otherwise, you must copy them from the `PlaybackEngines`/`MacStandaloneSupport` Folder:
    - Inside `Contents` must be a folder named `MonoBleedingEdge`, if its not here, copy it from: `PlaybackEngines`/`MacStandaloneSupport`/`MonoBleedingEdge`
    - Inside `Contents`/`Resources` must be a File named `MainMenu.nib`, if its not here, copy it from: `PlaybackEngines`/`MacStandaloneSupport`/`Source`/`Player`/`MacPlayer`/`MacPlayerEntryPoint`/`Resources`/`MainMenu.nib`
        - IMPORTANT NOTE: ¡¡Check the file extension, dont be confused with `MainMenu.xib`!!
    - Inside `Contents` must be a file named `Info.plist`, if not, copy it from: `PlaybackEngines`/`MacStandaloneSupport`/`Source`/`Player`/`MacPlayer`/`MacPlayerEntryPoint`/`Info.plist`
    - Inside `Contents`/`Frameworks` must be some libraries, including `libmonobdwgc-2.0.dylib` and `libMonoPosixHelper.dylib`, if not, copy them all from: `PlaybackEngines`/`MacStandaloneSupport`/`Frameworks`/`libmonoXXX.dylib`

IMPORTANT: Now we need to Modify some strings from `Contents`/`Info.plist`, otherwise it will cause conflicts when we try to port other games, or may cause different problems:

- Open the file with `TextEdit.app` or with your text editor of preference
- Find and Replace the next strings:
    - replace `UNITY_BUNDLE_IDENTIFIER` at `<key>CFBundleIdentifier</key>` with something like `unity.SomeCompany.gamename-Custom` replacing `gamename` with the name of your game.
    - replace `UNITY_PLAYER_BUNDLE_NAME` at `<key>CFBundleName</key>` with the name of the game.
    - if necessary, replace `UnityPlayerExec` at `<key>CFBundleExecutable</key>` with the correct name of the executable file at `MacOS` Folder (Most times is named `UnityPlayer`)
    - (Optionally) replace `UNITY_PLAYER_BUNDLE_VERSION` at `<key>CFBundleShortVersionString</key>` with a custom version number.

Now, assuming that you actually [have the game data](#getting-game-data):

- Open a new Finder Window (Right-click / Ctrl+click Finder and Select `New Finder Window`)
- Navigate to the game folder, there must be:
    - The game executable `gamename.exe`
    - And game data `gamename_Data`

## Porting and running the game

Now we will start copying game data to inside the UnityPlayer App:

- Copy Game Data Folder (`gamename_Data`) to `UnityPlayer.app`/`Contents`/`Resources`/ and rename it to `Data`
    - NOTE: On *VERY* old unity versions this folder must be inside `Contents`
- Navigate to the `Data` Folder and copy the `Plugins` Folder to `Contents`
    - IMPORTANT NOTE: You must also get mac version of the plugins inside or the game wont work, see [Missing libraries (`Fallback handler could not load library`)](#missing-libraries-(fallback-handler-could-not-load-library))
- Also, *inside of the `Data` folder* look for `Resources`, Navigate *inside* it, and copy everything into `UnityPlayer.app`/`Contents`/`Resources`/

If the game is running, Congrats!! You ported a Windows/Linux Unity game to be run on Mac!!

...Otherwise, you should [debug the binary and check the log](#troubleshoot-common-problems), most of the problems are caused by Steam DRM or because a Mac Version of some Plugin is missing, or simply there is a missing file/folder that you need to copy from Unity Editor, you must research by yourself.

# Troubleshoot common problems

Sometimes the game fails to run despite your best efforts. The best way to debug that would be launching the game with [`Terminal.app`](https://support.apple.com/guide/terminal/welcome/mac) and observing the error message:

- Start [`Terminal.app`](https://support.apple.com/guide/terminal/welcome/mac)
- Navigate to the game package: `cd /Applications/<GamePackage>/Contents/MacOS`
- Attempt to start the game: `./<GameTitle>`
- Observe the messages

- NOTE: Sometimes command-line output doesn't work, in those cases consider adding log file output: `./<GameTitle> -logfile <location-of-logfile>`, where `<location-of-logfile>` is something like `~/Downloads/unity_log.txt`

## Incorrect Unity standalone playback engine version

You might see: `Invalid serialized file version. File: "/Applications/<GamePackage>/Contents/Resources/Data/globalgamemanagers. Expected version: <expected_version>. Actual version: <actual_version>.` (e.g. `Expected version: 5.6.4f1. Actual version: 5.6.4p4.`)

- Make sure you find and download the correct expected version, see [Determine Unity version](#determine-unity-version) and [Download Unity](#download-unity)

## Unity 4.x.x games not launching

[Reddit user HomeStarRunnerTron](https://www.reddit.com/user/HomeStarRunnerTron/) troubleshooting tip:

`All the Unity v4 games that I tested (which were all small indie projects) don't launch at all unless I click on the Input tab before starting. I think that reinitializes the keyboard mapping or something. I don't know if there's a way around this step, but I have to do this every time for those games before they'll launch.`

## Validating that you've downloaded the correct standalone player version

Verify that the prepared version of the player is the correct one:

- Start [`Terminal.app`](https://support.apple.com/guide/terminal/welcome/mac)
- Navigate to the location of the player you previously discovered above, under the path `Contents/PlaybackEngines/MacStandaloneSupport/Variations` (e.g. `cd /Applications/Unity/Unity.app/Contents/PlaybackEngines/MacStandaloneSupport/Variations/macosx64_nondevelopment_mono/UnityPlayer.app/Contents/MacOS`)
- Run the following command: `strings UnityPlayer | grep <unity-version>`, e.g. `strings UnityPlayer | grep 4.7.0f1`
- Observe the output:

**Correct player version**

- The output contains multiple strings with that version, something like: `Invalid serialized file version. File: "%s". Expected version: 4.7.0f1. Actual version: %s.`

**Incorrect player version**

- If the output doesn't contain lines with that version - it's likely that you've got a wrong version and need to re-download the correct one: see [Determine Unity version](#determine-unity-version) and [Download Unity](#download-unity).

## Problems with graphics API

If the game is using an incompatible Graphics API (like DirectX11) you can try changing it to OpenGl, Vulkan, or Metal by adding one of these flags at running Unity Player:

- For Metal: `-force-metal`

- For OpenGl: `-force-glcore` or `-force-glcoreXY` where `XY` is the version number

- For Vulkan: `-force-vulkan`

For more information check [Unity Standalone Player command line arguments](https://docs.unity3d.com/Manual/PlayerCommandLineArguments.html)

## Missing libraries (`Fallback handler could not load library`)

In some cases you should check Player log file and see if the libraries are in the wrong location, in that case move the libraries to the listed path.

`In other cases` the library is missing from anywhere at the game data and you must get it from the internet or from another Unity game, if possible, preferably you should get the same version that came with the game, also [make sure that the plugin have the same architecture that the Player being used](#checking-if-the-game-doesnt-run-due-to-plugin-architecture-mismatch), in case that the plugin is not available on the needed architecture, you could try to compile it.

## Checking if the game doesn't run due to plugin architecture mismatch

Another problem that might prevent the game from launching successfully is plugin architecture mismatch. Here is how to verify that:

- Start [`Terminal.app`](https://support.apple.com/guide/terminal/welcome/mac)
- Navigate to the contents of the game app bundle (e.g. `cd /Applications/<Game.app>/Contents/`)
- Check if there is a `Plugins` folder: `cd Plugins` and plugins bundles there: `ls`

If plugins are individual files:

- Run `file <filename.dylib>`, e.g. `file libsteam_api.dylib`
- Output should contain `x86_64` and not just `i386`, e.g. this is expected good result:

```text
libsteam_api.dylib: Mach-O universal binary with 2 architectures: [i386:Mach-O dynamically linked shared library i386] [x86_64:Mach-O 64-bit dynamically linked shared library x86_64]
libsteam_api.dylib (for architecture i386):    Mach-O dynamically linked shared library i386
libsteam_api.dylib (for architecture x86_64):    Mach-O 64-bit dynamically linked shared library x86_64
```

- If the output contains just `i386` that means that the plug-in itself is 32-bit and the game won't work:

```text
libsteam_api.dylib: Mach-O bundle i386
```

If plugins are bundles:

- Navigate to each bundle `Contents/MacOS` and verify all binaries, e.g. `CSteamworks.bundle/Contents/MacOS` might have `CSteamworks`, `libsteam_api.dylib`, etc.
- Apply steps in the `If plugins are individual files:` above to validate those binaries
