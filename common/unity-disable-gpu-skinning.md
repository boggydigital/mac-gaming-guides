Disable GPU skinning for Unity 2019+ games that are missing models in CrossOver
===============================================================================

When you run Windows-only Unity games in CrossOver you might notice that they're missing some 3D models. 
However, thanks to Reddit user [u/r4ymonf](https://www.reddit.com/r/macgaming/comments/11csvvq/newer_unity_titles_like_risk_of_rain_2_render/) there is a simple workaround you can apply to get the models render properly.

## Pre-requisites

- (Obviously) CrossOver with a desired game installed in a bottle
- The examples below are for GOG.com versions, for Steam you can likely set launch options in the client 

## Trying -disable-gpu-skinning command line option

- Select the game icon in CrossOver
- Click `Run With Options` in the right panel
- Type/paste `-disable-gpu-skinning` into `Command-Line Options` text field
- Click `Run`

## Making a permanent launcher with that option

- Select `Run Command`
- Browse to the game executable - for GOG.com games, by default that'll be something like `GOG Games/<Game Name>/<Game Name>.exe`
- Click `Open` to select it - don't worry, that won't launch the game
- Focus `Command:` text field and add `-disable-gpu-skinning` at the very end, so that it looks something like this: `"/Users/<username>/Library/Application Support/CrossOver/Bottles/<Bottle Name>/drive_c/GOG Games/<Game Name>/<Game Name>.exe" -disable-gpu-skinning`
- NOTE: Make sure that `-disable-gpu-skinning` is added _after_ the path enclosing `"`
- Click `Save Command as a Launcher`

## Summary of games confirmed fixed or not fixed

### Fixed

- Frogun
- Dread Templar

- Risk of Rain 2 (thanks [u/r4ymonf](https://www.reddit.com/user/r4ymonf/)) 
- Encased (thanks [u/Tommy-kun](https://www.reddit.com/user/Tommy-kun/))
- Windbound (thanks [u/Tommy-kun](https://www.reddit.com/user/Tommy-kun/))
- 

### Not fixed

- Mages of Mystralia (thanks [u/Tommy-kun](https://www.reddit.com/user/Tommy-kun/))
- Going Under (thanks [u/Tommy-kun](https://www.reddit.com/user/Tommy-kun/))
- The Spectrum Retreat (thanks [u/Tommy-kun](https://www.reddit.com/user/Tommy-kun/))
- Pine (thanks [u/Tommy-kun](https://www.reddit.com/user/Tommy-kun/))
- Moving Out (thanks [u/Tommy-kun](https://www.reddit.com/user/Tommy-kun/))
- Aer: Memories of Old (thanks [u/Tommy-kun](https://www.reddit.com/user/Tommy-kun/))
- God's Trigger (thanks [u/Tommy-kun](https://www.reddit.com/user/Tommy-kun/))