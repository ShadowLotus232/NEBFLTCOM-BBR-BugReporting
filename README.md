# NEBFLTCOM-BBR-BugReporting
Issue-only repository for reporting bugs in the Better Battle Report mod for Nebulous: Fleet Command

Usage guide here: <TODO>

# Not-a-Bug
**"My file isn't saving."** Saving a file takes time. The larger the file, the longer it will take. If you start a new game too quickly, a previously requested report that has not finished writing may fail and be lost. When your file is complete there should be a pop-up text box that informs you so. If you wish to be sure, check the directory where your reports are saved and make sure the file size is not 0. If it is 0, be patient. This process can sometimes take a couple of minutes.

![image](https://github.com/ShadowLotus232/NEBFLTCOM-BBR-BugReporting/assets/34362934/c428df69-2815-4d76-b75b-afd91589c922)

**"Things (like missile paths) seem to jump around while timeline is moving."** This is a result of linear interpolation used to save space and record less data. It can look a little weird if you're not used to it, and it does result in a mildly approximated data picture, but it stops files from becoming too unnecessarily bloated and smoothes things over.

# Known Issues
  - Mod support is currently as-is. Reporting issues with mod compatibility is useful for the future, but is not a priority right now.

  - Viewer settings are very much in an alpha state. Please do not report issues with the settings panel.

  - Viewer transparency rendering order is a little wonky. Sometimes objects may appear to be on the wrong side of another transparent object. If that's all the issue is, please do not report it.

  - Projectiles can appear to move through things they did not in-game, or stop short of a target they hit. This is the result of a combination of factors:
    - latency
    - small approximation sacrifices made for filesize reduction
    - in the case of defender/pavise type weapons: they are non-physical in the game. While capturing their end points may be technically possible, it would also increase filesize by a non-insignificant amount. I've decided it's too much burden for too little a return, and there are currently no plans to address this issue.
