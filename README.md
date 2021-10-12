# NW.js Global Installer


### **FAQ:**

* What is NW.js?
  * A run time environment (RTE) that application code runs inside of.
  * A tool used for building Cross-Platform Desktop Apps (XPDAs).
  * A modified Chromium Browser with Node.js added in and a light API to interact with the OS.
* What problem is this project trying to solve?
  * NW.js is very large, depending on version it can be between 85-350MB (once unzipped). On top of this, you'll also have the unique application code, which is generally very small (only a few MB).
  * This project would allow developers to ship an installer that only contains the unique application code, and only downloads the large NW.js runtime files if they are not already installed on the user's computer.


# Project Document


This project is broken into two parts:

1. Builder
1. Installer


## Builder (CLI)

1. A tool to allow you to create the Installer `exe` described below
1. It should allow you to pick:
   * Folder containing the application files
   * A list of compatible NW.js versions (defaults to latest, multiple can be supplied)
   * Architechture: 32-Bit, 64-Bit, Either (defaults to 32-Bit)
   * Type: Normal, SDK, Either (defaults to Normal)
   * An icon to be used for the installer and desktop/start menu shortcuts
1. It will output a ready-to-use `.exe` file

Bare minimum this needs to work via CLI for automation. A UI can be built on top of the CLI.


## Installer (GUI)

1. User downloads a `.exe` file and runs it (made by the builder detailed the above section).
1. It contains a compressed set of files that it will decompress and place in the `C:\Program Files` (or `C:\Program Files (x86)`) folder.
   * Before placing the decompressed files, check if the folder already exists, and if so, delete the current contents first.
1. It then checks to see if the required version of NW.js already exists locally.
1. If it does not, it downloads and unzips it to a shared global location.
1. Finally it adds shortcuts to the Desktop and Start Menu.
   * The shortcut would point to the global location of the `nw.exe` file followed by a path in quotes to the application folder location that contains the `package.json`
   * **Example:** `C:\some global location\0.36.0-32-normal\nw.exe "C:\Program Files (x86)\MyAppName"`


## References

1. All NW.js versions can be downloaded from:
   * https://dl.nwjs.io
1. All NW.js versions in JSON form are listed here:
   * https://nwjs.io/versions.json


## Possible additional features:

* 90+% of desktop users are on Windows, however NW.js is designed to work on Debian based systems like Ubuntu, and on OSX as well. So though a Windows-only solution is acceptable, a cross-platform solution is preferred.
* The builder will eventually be published as an node module on npm, so that it can be version controlled, installed, and used as a build tool by others.
* It would be useful to accept a JSON file as the settings input for the builder so they could be stored inside the `package.json`. Then that file can be fed into the CLI as `nw-global-installer ./package.json` or something similar.
* Not sure exactly where the best place to store the downloaded shared NW.js versions is. Open to suggestions.
* Would be nice if the windows version also came with an uninstaller.
  * Should have checkbox that says "Remove NW.js 0.57.1 (may be used by other applications)" when uninstalling?
* May be better to have the desktop/start menu shortcuts point to a small exe that checks the `package.json` file, then checks to see if NW.js is still installed and if not, start downloading/installing it first, then when done, launch the app normally. This would be a sort of self-repairing system in case someone uninstalled a different NW.js based app and removed a shared NW.js install that was still needed. This may be overkill though, as the user could just re-install the software they broke.

