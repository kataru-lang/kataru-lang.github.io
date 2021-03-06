# Installation

For syntax highlighting in VSCode:
[Install Kataru Extension](https://marketplace.visualstudio.com/items?itemName=Kataru.vscode-kataru).

## Unity users
1. Install via git url by adding this entry in your manifest.json:
```
"com.katarulang.unitykataru": "https://github.com/kataru-lang/unity-kataru.git"
```
2. Become familiar with <a href="#/api/unity?id=kataru-settings">Project Settings/Kataru Settings</a>.

---

Alternatively, if your project is in a git repo, you can add it as a submodule by running the following command from the root of your Unity project:
```
git submodule add https://github.com/kataru-lang/unity-kataru <path> 
```
where `<path>` points to a directory relative to the current working directory that you want the submodule to go into.

## Non-Unity users
Download the source code of [Kataru](https://github.com/kataru-lang/kataru).
