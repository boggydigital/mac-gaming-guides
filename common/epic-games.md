How to get game data from Epic Games
===============================

## Prerequisites

1. Some level of experience with macOS [Terminal.app](https://support.apple.com/guide/terminal/welcome/mac);
2. [Legendary](https://github.com/derrod/legendary) has been installed;
3. (Obviously) An existing Epic Games account that has games you're looking to play.

## Getting game data using Legendary

NOTE: Since this is a third-party open source software, we cant guarantee anything, use at your own risk

1. Open `Terminal.app` and enter the folder where you saved legendary `cd /path/to/legendary/` (you can type `cd`, add an space, and then drag in terminal window the Folder)
2. Type `./legendary auth` to login, your web browser will be open, you may need to login with your account.
3. Copy the code that legendary is requesting you
4. You are logged in! Now type `./legendary list --platform Windows` (or replace Windows for Win32)
5. A list will appear with all games for the specified platform that you have in your account, search for the game you want to install, you should copy the code in "App name"
6. Then type `./legendary install CODEHERE --platform Windows`, legendary will start downloading the windows version of the game.
7. Thats all! Now the game should be located at your home folder, inside Games folder (~/Games/


