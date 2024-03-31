# NEBFLTCOM-BBR-BugReporting
Issue-only repository for reporting bugs in the Better Battle Report mod for Nebulous: Fleet Command

# Things Wot BBR Do
  - Records positions of the following objects (as they are implemented in vanilla):
    - Ships
    - Missiles
    - Collidable Terrain (terrain that does not have a collider in-game will not be captured)
    - Shells
    - Beams
    - EWar
    - Non-Physical munitions (Defender/Pavise)
  - Records the terrain of *most* modded maps. It has been tested on most maps available at time of writing. New maps will be tested as released, and issues fixed as they arise.
  - Calculates the score at any given time based on the rules of the scenario (CTF and Station Capture are not implemented, and are very low priority as their scoring is much easier to intuit).
  - .bbr files will be exported to the same place as base game battle reports (`..\Steam\steamapps\common\Nebulous\Saves\SkirmishReports`) with the same name as the associated battle report by clicking the 'Export Battle Report' button in the debriefing lobby.
  - Provides C# methods for dedicated servers to interact with report data:
    - `void BBRMod.BBRMod.DedicatedServerExport(string fileName, string filePath = null, bool debug = false)` will write the .bbr file of the most recent game to the specified place with the specified name. If `filePath` is passed as null it will save to the default skirmish report location. If `debug` is passed as true BBR will also output an uncompressed .json file that contains the same information as the .bbr file. It is helpful for debugging if there is an issue, or for reading the data raw if you so wish to do so, but it will be 4-8 times larger on average.
    - If your server already saves battle reports by calling `Game.Reports.FullAfterActionReport.Export(string filename)` then .bbr files will automatically be written to the same place with the same filename.
    - `bool BBRMod.BBRMod.IsWritingReport()` can be used to check if the server is in the process of serializing/writing the report file. 
    - `bool BBRMod.BBRMod.IsReportReady()` will return false if the report has not been requested, or has not finished being serialized/written. 
    - BBR will start automatically serializing a `byte[]` after `Game.SkirmishDebriefingLobbyController.Start()` finishes running. If this does not happen and you want the report as a `byte[]`, you can use `void BBRMod.BBRMod.ForceStartSerialization()` to manually initiate the process.
    - `byte[] BBRMod.BBRMod.GetReportBytes()` will return the `byte[]` containing the compressed binary .bbr file in memory if you wish to do something with it like sending it to another server before writing. If called when a report is not ready or while the Recorder is still running it will warn you as such and return an empty array.
    - `void BBRMod.BBRMod.ForceHaltSerialization()` will immediately halt any data serialization/writing and clear all data.

# Things Wot BBR D'ain't
  - BBR is not a game. It does not have game logic. It doesn't care about collisions, or that a missile and a ship are different things, it is just a visualization tool for recorded data. A video player doesn't care what the content of the video is, it just displays it. BBR is the same: it displays the data without doing any sort of interpretation.
  - BBR is a passion project, started by one person and currently being built by another. It's not perfect. There will be bugs, there will be design choices that you may not agree with, and news/development may be slow at times. That said: BBR is also both open source and a client-side, code-only mod. The viewer is a completely standalone piece of software. If there is something you don't like, want to change, or want to fix, you can simply build your own copy of either or both.

# Things Wot BBR Will Do But Ain't Done Yet (in no particular order)
  - Mod support will be fleshed out as there is dev time available, but vanilla compatibility and completeness will always take priority.
  - Saving partial reports for game drops/draws
  - Integrate base game battle reports
  - Selection of objects and detailed readouts of information about said objects (missile and ship configurations, status conditions, damage, etc.)
  - Viewer settings
  - Sensor web visualization, target locks
  - RCS/Signature visualization
  - Lifeboats
  - Orders/waypoints
  - PD net visualization
  - Projected threat/zones of control visualization
  - Derived stats (ship movement/death heatmaps, missile heatmaps, first blood, map side winrates, etc.) (this is honestly more of a separate project but it bears mentioning)

# Not-a-Bug
**"My file isn't saving."** Saving a file takes time. The larger the file, the longer it will take. If you start a new game too quickly, a previously requested report that has not finished writing may fail and be lost. When your file is complete there should be a pop-up text box that informs you so. If you wish to be sure, check the directory where your reports are saved and make sure the file size is not 0. If it is 0, be patient. This process can sometimes take a couple of minutes.

![image](https://github.com/ShadowLotus232/NEBFLTCOM-BBR-BugReporting/assets/34362934/c428df69-2815-4d76-b75b-afd91589c922)

**"Things (like missile paths) seem to jump around while timeline is moving."** This is a result of linear interpolation used to save space and record less data. It can look a little weird if you're not used to it, and it does result in a mildly approximated data picture, but it stops files from becoming too unnecessarily bloated.

# Known Issues
  - Mod support is currently as-is. Reporting issues with mod compatibility is useful for the future, but is not a priority right now.

  - Viewer settings are very much in an alpha state. Please do not report issues with the settings panel.

  - Viewer transparency rendering order is a little wonky. Sometimes objects may appear to be on the wrong side of another transparent object. If that's all the issue is, please do not report it.

  - Projectiles can appear to move through things they did not in-game, or stop short of a target they hit. This is the result of a combination of factors:
    - latency
    - small approximation sacrifices made for filesize reduction
    - in the case of defender/pavise type weapons: they are non-physical in the game. While capturing their end points may be technically possible, it would also increase filesize by a non-insignificant amount. I've decided it's too much burden for too little a return, and there are currently no plans to address this issue.
