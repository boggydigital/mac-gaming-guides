How to get game data from GOG.com
=================================

## Prerequisites

1. Some level of experience with macOS [Terminal.app](https://support.apple.com/guide/terminal/welcome/mac);
2. (Optional) Some level of experience with [Homebrew](https://brew.sh);
3. (Obviously) An existing GOG.com account that has games you're looking to play.

## Using innoextract to unpack GOG.com Windows installers

### Prerequisites

This guide uses [innoextract](https://constexpr.org/innoextract/). Official website has recommendations on how to install it on macOS, for this guide we'll use the Homebrew option, that you need to have [installed](https://docs.brew.sh/Installation):

- Open Terminal.app
- Copy `brew install innoextract`, paste into Terminal and execute.
- Verify it's installed by running `innoextract` in the Terminal. It should output something like:

```
innoextract: no input files specified
Try the --help (-h) option for usage information.
```

### Step by step guide

1. Navigate to your [GOG.com account](https://www.gog.com/account);
2. Find the game you're interested in and download Windows installer files (usually named `setup_<game_name>_<version>.exe`);
    - NOTE: Make sure you're downloading from the "DOWNLOAD OFFLINE BACKUP GAME INSTALLERS" section and not the big blue "DOWNLOAD AND INSTALL NOW" button. The latter is a Galaxy installer, the former is an actual game installer.
3. (We'll assume you've downloaded files to `~/Downloads` folder);
    - HINT: innoextract will extract installers into the current directory, so consider moving your installer files into a new, game-specific directory you'll need to create under `~/Downloads`.
4. Open `Terminal.app` and navigate to `~/Downloads` or any other folder where the installer is located: `cd ~/Downloads`;
5. Run `innoextract <game_installer_version>.exe` and wait for the extraction to complete;
6. Congrats! You have got yourself game data for that game, please refer to specific game guide that will detail where to put that data for a certain source port.

## Using unzip to unpack GOG.com Linux installers

When a game ships Linux installers you can unpack those without any innoextract. Here is how to do that:

1. Navigate to your [GOG.com account](https://www.gog.com/account);
2. Find the game you're interested in and download Linux installer files (usually named `<game_name>_<version>.sh`);
3. (We'll assume you've downloaded files to `~/Downloads` folder);
    - HINT: unzip will extract installers into the current directory, so consider moving your installer files into a new, game-specific directory you'll need to create under `~/Downloads`.
4. Open `Terminal.app` and navigate to `~/Downloads` or any other folder where the installer is located: `cd ~/Downloads`;
5. Run `unzip <game_installer_version>.sh` and wait for extraction to complete;
6. Congrats! You have got yourself game data for that game, please refer to specific game guide that will detail where to put that data for a certain source port.
    - NOTE: Please note the version for Linux and Windows installers, as sometimes Linux installers are missing recent updates.