# WoW-Addon-Git-Backup-Lesson

The purpose of this repository is to provide instructions on how to use Git as a method of backing up all of your World of Warcraft addons **and** their settings. The existing tools like CurseForge don't do the latter part very well.

## Getting Started
### Download Git for Windows or macOS
The first step of this process involves installing Git to your gaming PC. It doesn't matter if it is Windows or macOS. Git works with both. You just need to download the appropriate client for the type of rig you're running.

https://git-scm.com/downloads

### Installing Git
Once you have the Git download package for your OS, proceed with installing it. You shouldn't need to change any of the default options during the installation. 

### Initializing the Git repository
Once Git is installed, you're now ready to initialize the Git repository by going to your World of Warcraft Account folder that has the `Interface\Addons` and `WTF` directories. For Windows clients, this is usually going to reside at `C:\Program Files (x86)\World of Warcraft\_retail_\`

_For this next part you can use either your OS command line or the GUI for Git. For Windows, you can use `cmd` or PowerShell to act as the CLI for working with the Git repo. In this example I will be using PowerShell on Windows 11. To create your shell (CLI session), for Windows, you can use the shortcut `Windows+R` hotkey to open the `Run` program where you can open `cmd` by entering `cmd` in the window and clicking `OK`_

```
PowerShell 7.2.4
Copyright (c) Microsoft Corporation.

https://aka.ms/powershell
Type 'help' to get help.

PS C:\Users\lilte> cd C:\
PS C:\> cd '.\Program Files (x86)\'
PS C:\Program Files (x86)> cd '.\World of Warcraft\'
PS C:\Program Files (x86)\World of Warcraft> cd .\_retail_\
PS C:\Program Files (x86)\World of Warcraft\_retail_> ls

    Directory: C:\Program Files (x86)\World of Warcraft\_retail_

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----           4/27/2022  7:35 PM                ..bfg-report
d----           4/27/2022  7:39 PM                Cache
d----           4/28/2022 10:05 PM                Errors
d----           4/27/2022  7:35 PM                Fonts
d----           4/27/2022  7:35 PM                GPUCache
d----           4/27/2022  7:35 PM                Interface
d----           5/19/2022  7:43 PM                Logs
d----           4/27/2022  9:46 PM                Screenshots
d----            5/3/2022 12:19 AM                Utils
d----           4/27/2022  7:36 PM                wow-addons
d----           4/27/2022  7:35 PM                WTF
-a---           5/19/2022  7:41 PM             28 .flavor.info
-a---           4/27/2022  7:35 PM            323 .gitignore
-a---           4/27/2022  7:29 PM         786320 BlizzardError.exe
-a---           4/27/2022  7:29 PM        1906768 d3d12.dll
-a---           4/27/2022  7:29 PM        1213200 DBGHELP.DLL
-a---           4/27/2022  7:29 PM        1452616 dxilconv7.dll
-a---           4/27/2022  7:35 PM             63 Wipe-WoW-Logs.ps1
-a---            5/3/2022 12:19 AM       63300040 Wow.exe

PS C:\Program Files (x86)\World of Warcraft\_retail_>
```
The printout above shows the default location of the WoW Retail folder and how to get there from the CLI. The next part is to do the initialization. First, however, we should define the `.gitignore` file, or use the one I provide you in this repository. **This is important because if you try to make the entire `_retail_` or `_classic_` directory a Git repository without exclusions, it will try to make about 2.5GB of data part of the Git repository and that is not what we want. We want a repository that is only the size that is needed to backup your addons and settings, which should be a few hundred Megabytes at most. (The size of the repository will grow over time because it keeps track of every change that is made to the files in the repository**

#### Example .gitignore file - Same as in this repository that you can download and copy to the `_retail_` directory
```
Cache/*
Logs/*
UTILS/*
Screenshots/*
Wow.exe
amd_ags_x64.dll
BlizzardError.exe
d3d12.dll
DBGHELP.DLL
dxilconv7.dll
.curseclient
.flavor.info
Interface/addons/TheUndermineJournal/*
Interface/addons/TradeSkillMaster_AppHelper/*
Interface/addons/TradeSkillMaster/*
Interface/addons/RaiderIO/*
Interface/addons/BigWigs/Media/*
```
This will exclude all of the directories listed above, which are not necessary to backup addons. I excluded the TradeSkillMaster folders because I use the TSM desktop client and pay for the premium version which automatically backs up all of my TSM settings to their own purpose-built cloud. I also excluded BigWigs Media and RadierIO folders because it is not necessary to backup BigWigs media files or the RaiderIO database which you should use the native RaiderIO client to manage that addon and its data.

Now we can proceed with the actual Git commands! Please note that I made a copy of my _retail_ folder to a different location on my machine to show how to use Git without it corrupting my live repository (although I could delete the whole folder and re-install the game using the Battle.net client and the repository if you push it to a remote location such as Github or Gitlab. This will keep your addons and settings backed up in the cloud. **Note: if you are using a Git repository service like Github that is available to the public Internet, make sure you make your repository a Private one that can only be accessed with your Github account. Otherwise anyone in the world could look at the contents of your repository which can contain sensitive personal information.**

```
PowerShell 7.2.4
Copyright (c) Microsoft Corporation.

https://aka.ms/powershell
Type 'help' to get help.

PS C:\Users\lilte> cd E:\
PS E:\> cd .\_retail_\
PS E:\_retail_> ls

    Directory: E:\_retail_

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----           5/19/2022  8:18 PM                ..bfg-report
d----           5/19/2022  8:18 PM                Cache
d----           5/19/2022  8:18 PM                Errors
d----           5/19/2022  8:18 PM                Fonts
d----           5/19/2022  8:18 PM                GPUCache
d----           5/19/2022  8:18 PM                Interface
d----           5/19/2022  8:19 PM                Logs
d----           4/27/2022  9:46 PM                Screenshots
d----           5/19/2022  8:19 PM                Utils
d----           4/27/2022  7:36 PM                wow-addons
d----           5/19/2022  8:19 PM                WTF
-a---           5/19/2022  7:41 PM             28 .flavor.info
-a---           4/27/2022  7:35 PM            323 .gitignore
-a---           4/27/2022  7:29 PM         786320 BlizzardError.exe
-a---           4/27/2022  7:29 PM        1906768 d3d12.dll
-a---           4/27/2022  7:29 PM        1213200 DBGHELP.DLL
-a---           4/27/2022  7:29 PM        1452616 dxilconv7.dll
-a---           4/27/2022  7:35 PM             63 Wipe-WoW-Logs.ps1
-a---            5/3/2022 12:19 AM       63300040 Wow.exe

PS E:\_retail_> git init
Initialized empty Git repository in E:/_retail_/.git/
PS E:\_retail_> git add .
<redacted extraneous output>
The file will have its original line endings in your working directory
PS E:\_retail_>
```
`git add .` is instructing Git to add everything in the directory, as it is a shortcut for `git add ./` which in *nix environments and newer versions of PowerShell means all of the directory you're in. This command may take up to roughly 30-60 minutes to complete depending upon the speed of the disk, the number of addons you have, and the speed of your processor. You will most likely see a boat load of `warning: CRLF will be replaced by LF in Interface/Addons/` printed on the screen. These warnings are harmless and only mean that in some instances, the URI for the directory that the Git client is adding to the repository is not compatible with the \*nix style directory syntax that the repository must adhere to. In other words, files that have whitespace in their name or parent directories is permitted in Windows, but in \*nix file names can only include spaces in them if you use the escape character `\` after every space. This is because in \*nix environments, a space at the end of a directory URI indicates where it ends. 

### Commit the added files to the repository
The next piece is an important part about Git version control. When you run `git add .` it adds any new files to the repository and any changes that have been made since the last commit. Since this is your first commit, there won't be any changes to track until you make the first commit. 

Before making a commit, you can check the status of the repository with `git status` executed in the `_retail_` folder
```
PS E:\_retail_> git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   ..bfg-report/2021-03-22/17-47-03/cache-stats.txt
        new file:   ..bfg-report/2021-03-22/17-47-03/deleted-files.txt
        new file:   ..bfg-report/2021-03-22/17-47-03/object-id-map.old-new.txt
        new file:   .gitignore
        new file:   Errors/2021-02-23 23.05.31/crash.dump
        new file:   Errors/2021-02-23 23.05.31/crash.txt
        new file:   Errors/2022-03-23 13.04.48 Crash - 34856.txt
        new file:   Errors/2022-04-26 12.31.48 Crash - 3348.txt
        new file:   Fonts/ARIALN.ttf
        new file:   Fonts/FRIZQT__.ttf
        new file:   Fonts/MORPHEUS.ttf
        new file:   Fonts/skurri.ttf
        new file:   GPUCache/data_0
        new file:   GPUCache/data_1
        new file:   GPUCache/data_2
        And so on ... ... ...
```
Now we're ready to make the first commit to the repository, which marks the state of the repository directories, files, and their contents at a point in time. 
```
PS E:\_retail_> git commit -m "Initialize Repository"
<snip>
 create mode 100644 WTF/Account/LILTECHDUDE/macros-cache.txt
 create mode 100644 WTF/Account/LILTECHDUDE/tts-cache-account.old
 create mode 100644 WTF/Account/LILTECHDUDE/tts-cache-account.txt
 create mode 100644 WTF/Config.wtf
 create mode 100644 WTF/SavedVariables/Blizzard_Console.lua
 create mode 100644 WTF/SavedVariables/Blizzard_Console.lua.bak
 create mode 100644 Wipe-WoW-Logs.ps1
PS E:\_retail_>
```
Congratulations! You just made your first Git repository commit. Now, any and every change that is made to the repository directories, files, or their contents, will be tracked relative to the state of the last commit. A commit is a sort of snapshot. At any time you can revert all or some of the changes that have been made to the files since the last commit, which would function like restoring a backup. **Note: if you revert changes that have not been committed to the repository, then they will be lost when Git does this restoration.** 

### Repository Changes
Now you may be wondering, how can we make a new snapshot of the repository to make another point-in-time backup of everything? You repeat the `git add .` and `git commit -m "<insert commit message>"` steps. That's it! 

```
PS C:\Program Files (x86)\World of Warcraft\_retail_> git commit -m "Snapshot Inter-patch 9.2.5"
[MOLECULE 6d1bb920] Snapshot Inter-patch 9.2.5
 208 files changed, 2059641 insertions(+), 2089763 deletions(-)
 rewrite Interface/AddOns/AstralKeys/CHANGELOG.md (89%)
 create mode 100644 Interface/AddOns/ElvUI/Core/Media/ChatLogos/PalmTree.tga
 create mode 100644 Interface/AddOns/ElvUI/Core/Media/Textures/NormTex3.tga
 create mode 100644 Interface/AddOns/ElvUI/Core/Modules/DataTexts/SpellHaste.lua
 rewrite Interface/addons/AllTheThings/CHANGELOG.md (95%)
 rewrite Interface/addons/ElvUI/Core/Media/ChatLogos/ElvSimpy.tga (95%)
 rewrite Interface/addons/WIM/CHANGELOG.md (73%)
 rewrite Interface/addons/WeakAurasCompanion/data.lua (97%)
 rewrite WTF/Account/LILTECHDUDE/SavedVariables/RaiderIO.lua (92%)
 copy WTF/Account/LILTECHDUDE/SavedVariables/{RaiderIO.lua => RaiderIO.lua.bak} (100%)
PS C:\Program Files (x86)\World of Warcraft\_retail_> git push origin MOLECULE
Enumerating objects: 377, done.
Counting objects: 100% (377/377), done.
Delta compression using up to 32 threads
Compressing objects: 100% (196/196), done.
Writing objects: 100% (207/207), 4.71 MiB | 3.01 MiB/s, done.
Total 207 (delta 149), reused 4 (delta 2), pack-reused 0
remote: Resolving deltas: 100% (149/149), completed with 146 local objects.
remote: . Processing 1 references
remote: Processed 1 references in total
To gitea.admin.apartment.networkgeek.cloud:wow/wow-addons.git
   aaecaa69..6d1bb920  MOLECULE -> MOLECULE
```
As you can see above, I made a commit while addon developers were updating their addons for v9.2.5. 
I went the extra step of pushing a copy of my Git repository MOLECULE (hostname of my PC) branch to a repository on a Gitea server I have running on my internal network. You can do the same thing with any remote system including Github by following the `git remote` subroutine.

At this point you should get familiar with the rest of Git's functions. A good reference is https://git-scm.com/docs

### Reverting Changes
If you need to restore an earlier commit and revert the changes that have been made since then, then you will need to use the `git revert` function, which can be tricky, especially if you only want to revert some of the changes since the last commit. I suggest that you use a GUI Git client to visualize the repository changes instead of trying to figure out the correct CLI commands.

https://git-scm.com/downloads/guis
