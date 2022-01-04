How to get game data from Steam
===============================

## Prerequisites

1. Some level of experience with macOS [Terminal.app](https://support.apple.com/guide/terminal/welcome/mac);
2. [SteamCMD](https://developer.valvesoftware.com/wiki/SteamCMD) has been installed;
    - HINT: [Homebrew](https://brew.sh) provides an easy way to install SteamCMD: `brew install steamcmd`;
3. SteamCMD has been initialized. This is done automatically on first launch: run `steamcmd` in Terminal.app.
    - HINT: If you get “Breakpad.framework” can’t be opened because Apple cannot check it for malicious software. error - open System Preferences > Security & Privacy > General and look for message about "Breakpad.framework". You'll need to click the button to allow it to run.
4. (Obviously) An existing Steam account that has games you're looking to play.

## Getting game data using SteamCMD

1. Open `Terminal.app` and launch SteamCMD by running `steamcmd` command. You should see the `Steam>` prompt;
2. Set install directory by running `force_install_dir <directory>` on SteamCMD prompt;
    - NOTE: We'll assume you're using `/Users/<your_macos_username>/Downloads` as your install directory. It seems like SteamCMD has issues with `~/` paths, so use absolute path to get files downloaded under your user's home folder.
    - HINT: SteamCMD will download files into the folder you've specified, so consider creating a subdirectory specifically for the game you're downloading.
3. (Unless already done earlier) Login into your Steam account with SteamCMD: run `steamcmd` in Terminal.app, then `login <Steam_Username>` in SteamCMD prompt, where <Steam_Username> is your Steam username. Follow the prompts to provide password and second two-factor code.
4. [Set your platform as Windows](https://developer.valvesoftware.com/wiki/SteamCMD#Cross-Platform_Installation) by running `@sSteamCmdForcePlatformType windows` command on SteamCMD prompt;
5. [Download game files](https://developer.valvesoftware.com/wiki/SteamCMD#Downloading_an_app) by running `app_update <app_id> validate` on SteamCMD prompt. This command will download and validate game files;
    - HINT: You can get `app_id` from Steam game URL: `https://store.steampowered.com/app/<app-id>/<game_name>`
6. Congrats! You have got yourself game data for that game, please refer to specific game guide that will detail where to put that data for a certain source port.

