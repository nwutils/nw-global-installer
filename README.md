# NW.js Global Installer


### **FAQ:**

* **Q:** What is "NW.js"?
  * A run time environment (RTE) that application code runs inside of.
  * A tool used for building Cross-Platform Desktop Apps (XPDAs).
  * A modified Chromium Browser with Node.js added in and a light API to interact with the OS.


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
