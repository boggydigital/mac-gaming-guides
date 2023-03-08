How to get game data from Epic Games
===============================

Guide originally made by [M0REKZ](https://github.com/M0REKZ), using the Steam one as Base

## Prerequisites

1. Some level of experience with macOS [Terminal.app](https://support.apple.com/guide/terminal/welcome/mac);
2. [Legendary](https://github.com/derrod/legendary) has been installed;
3. (Obviously) An existing Epic Games account that has games you're looking to play.

## Getting game data using Legendary

NOTE: Since this is a third-party open source software, we cant guarantee anything, use at your own risk

1. Open `Terminal.app` and enter the folder where you saved legendary `cd /path/to/legendary/` (you can type `cd`, add an space, and then drag the folder in terminal window)
2. Type `./legendary auth` to login, your web browser will be open, you may need to login with your account.
3. Copy the code that legendary is requesting you
4. You are logged in! Now type `./legendary list --platform Windows` (for some games maybe you will need to replace `Windows` with `Win32`)
5. A list will appear showing all games in your account that are compatible with the specified platform (in this case Windows), search for the game you want to install, you should copy the code next to "App name"
6. Then type `./legendary install CODEHERE --platform Windows`(Dont forgot to replace "CODEHERE" with the code from the step above!), legendary will start downloading the Windows version of the game.
7. Thats all! Now the game should be located at your Home folder, inside Games folder (`~/Games/`)


