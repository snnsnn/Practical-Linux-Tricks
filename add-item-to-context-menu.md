# Add VS Code to right click menu for nemo

Create a file in `~/.local/share/nemo/actions` with name `vscode.nemo_action` and content as follows:

```
[Nemo Action]
Name=Open in VS Code
Comment=Open in VS Code
Exec=code %F
Icon-Name=visual-studio-code
Selection=Any
Extensions=dir;
```