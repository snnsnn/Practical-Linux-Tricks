# Nemo file manager

## Fix terminal path

Nemo uses Cinnamon's default terminal stored in the gsettings configuration. So running following command should fix the issue on Manjaro:

```bash
$ gsettings set org.cinnamon.desktop.default-applications.terminal exec 'xfce4-terminal'
```

## Add application to right click context menu

Create a file in `~/.local/share/nemo/actions` with app name such as `vscode.nemo_action` and content as follows:

```
[Nemo Action]
Name=Open in VS Code
Comment=Open in VS Code
Exec=code %F
Icon-Name=visual-studio-code
Selection=Any
Extensions=dir;
```