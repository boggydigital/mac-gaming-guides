How to update 32-bit macOS Unity game to run on 64-bit macOS 
============================================================

**Thank you [M0REKZ](https://github.com/M0REKZ) for the [original version of the porting guide](https://github.com/M0REKZ/PortingMacGames/blob/Principal/Unity/Unity%20Porting%20Guide.md) I've reformatted and detailed here!**

In 2019 Apple has released macOS Catalina that only [supported 64-bit apps](https://en.wikipedia.org/wiki/MacOS_Catalina#Removed_or_changed_components). With that change many older Unity games that shipped 32-bit Unity Player stopped working.

This guide will provide a step-by-step instruction on updating the game to 64-bit Unity Player to allow them to run on macOS versions post Catalina, as well as on [Apple Silicon](https://en.wikipedia.org/wiki/Apple_silicon) devices through [Rosetta 2](https://en.wikipedia.org/wiki/Rosetta_(software)#Rosetta_2).

Here are the steps required to update the game:

- [Download and install the game](#download-and-install-the-game)
- [Determine Unity version](#determine-unity-version)
- [Download Unity](#download-unity-)
- [Prepare standalone player files](#prepare-standalone-player-files)
- [Update the game](#update-and-run-the-game)
- (Optionally) [Troubleshoot common problems](#troubleshoot-common-problems)

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

### Method 1: Using Info.plist

- Navigate to the folder where the game is installed
- Locate the game icon at this new location and Right-click / Ctrl+click, them select `Show Package Contents`
- Open `Contents`/`Info.plist` (NOTE: you might use `TextEdit.app` or any other text editor)
- Look for the string that starts with: `Unity Player version X.Y.Z`, for example: `Unity Player version 5.6.4p4 (72f24c04957f). (c) 2017 Unity Technologies ApS. All rights reserved.`
- That version is what we need, in the example above that's `5.6.4p4` (NOTE: Unity added 64-bit support at version 4.2, so games before that can't be updated)

### Method 2: Using level0 file

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
- Rename `Payload` -> `Payload.zip` and confirm that you want to add `.zip` extension
- Unpack `Payload.zip` and observe a new folder containing `Unity.app` (e.g. `Unity`/`Unity.app`)

### Get standalone playback engine

Regardless of how you've got `Unity`/`Unity.app` - either by installing or extracting:

- Navigate to the location of `Unity.app` in `Finder`
- Right-click / Ctrl+click, them select `Show Package Contents`
- Navigate to `Contents`/`PlaybackEngines`/`MacStandaloneSupport`/`Variations`
- Carefully observe folder names in that location:
    - You'll need `macosx64_nondevelopment_mono` folder
    - NOTE: you likely don't want to use `universal_nondevelopment_mono` as that refers to Universal 1 (PowerPC / Intel), not Universal 2 (Intel / Apple Silicon) for older installers
- While in `macosx64_nondevelopment_mono` folder, locate `UnityPlayer.app` and Right-click / Ctrl+click, them select `Show Package Contents`
- Navigate to `Contents`
- Ultimately you need two artifacts:

  - `Contents`/`Frameworks` folder (it should contain `MonoEmbedRuntime`/`osx`)
  - `Contents`/`MacOS`/`UnityPlayer` binary

- Keep them ready or copy to some more convenient location, either way - you're ready for the next step!

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

## Troubleshoot common problems

Sometimes the game fails to run despite your best efforts. The best way to debug that would be launching the game with [`Terminal.app`](https://support.apple.com/guide/terminal/welcome/mac) and observing the error message:

- Start [`Terminal.app`](https://support.apple.com/guide/terminal/welcome/mac)
- Navigate to the game package: `cd /Applications/<GamePackage>/Contents/MacOS`
- Attempt to start the game: `./<GameTitle>`
- Observe the messages

- NOTE: Sometimes command-line output doesn't work, in those cases consider adding log file output: `./<GameTitle> -logfile <location-of-logfile>`, where `<location-of-logfile>` is something like `~/Downloads/unity_log.txt`

### Incorrect Unity standalone playback engine version

You might see: `Invalid serialized file version. File: "/Applications/<GamePackage>/Contents/Resources/Data/globalgamemanagers. Expected version: <expected_version>. Actual version: <actual_version>.` (e.g. `Expected version: 5.6.4f1. Actual version: 5.6.4p4.`)

- Make sure you find and download the correct expected version: [Determine Unity version and download the player](#determine-unity-version-and-download-the-player)

### Unity 4.x.x games not launching

[Reddit user HomeStarRunnerTron](https://www.reddit.com/user/HomeStarRunnerTron/) troubleshooting tip:

`All the Unity v4 games that I tested (which were all small indie projects) don't launch at all unless I click on the Input tab before starting. I think that reinitializes the keyboard mapping or something. I don't know if there's a way around this step, but I have to do this every time for those games before they'll launch.`