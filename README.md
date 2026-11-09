# Tiling Dock for Omarchy

Omarchy 4.5 ships an app dock, but it only appears on **floating** workspaces.
Tiling Dock is a drop-in replacement for it that shows up on **every** workspace,
tiled or floating, and lays out your apps the way the macOS Dock does.

![Tiling Dock preview](screenshots/preview.png)

## Features

- **Works in tiling mode.** The dock is shown on every workspace, whatever its
  layout (dwindle, scrolling or floating). It still hides behind fullscreen windows.
- **Pinned apps first, macOS-style.** Your pinned apps always sit on the left, in
  the order you pinned them. Apps that are open but not pinned show up on the
  right, after a separator.
- **Smart auto-hide.** With auto-hide on, the dock stays visible on an empty
  workspace and tucks away only once an app is open there. Push the pointer to
  the screen edge to bring it back.
- **Everything else from the stock dock still works.** You can drag to reorder,
  pin and unpin from the right-click menu, open the app grid, restore minimized
  windows, use any of the four positions, and see running indicators.

## Install

Requires **Omarchy 4.5 or later**.

```bash
omarchy plugin add https://github.com/gusmezomo/omarchy-tiling-dock --enable
```

That's it. Enabling Tiling Dock turns off the built-in `omarchy.dock` for you, so
you won't end up with two docks.

> Plugins run unsandboxed inside the Omarchy shell. As with any plugin, read the
> code before you install it.

## Configure

Tiling Dock reads the same `dock` settings as the built-in dock, so the stock
commands work:

```bash
omarchy dock position bottom      # top | bottom | left | right
omarchy dock autohide on          # on | off | toggle
omarchy dock pin org.gnome.Nautilus
omarchy dock unpin org.gnome.Nautilus
```

Or edit `~/.config/omarchy/shell.json` directly:

```json
"dock": {
  "position": "bottom",
  "autohide": true,
  "pinned": ["org.gnome.Nautilus", "chromium", "com.mitchellh.ghostty"]
}
```

Pinned entries are desktop file ids (the `.desktop` file name without the extension).

## Update

```bash
omarchy plugin update gusmezomo.dock
omarchy restart shell
```

## Uninstall

```bash
omarchy plugin disable gusmezomo.dock   # switches the built-in dock back on
omarchy plugin remove gusmezomo.dock
```

## How it works

Tiling Dock started as a copy of the built-in dock made with
`omarchy plugin clone omarchy.dock`. The first commit in this repository is that
unmodified copy, so `git diff <first-commit>` shows every change made on top of
it. The changes are:

- `DockWindow.qml` no longer needs the workspace to be floating to show the dock.
  Auto-hide only applies when the active workspace has windows. It also draws
  the separator between pinned and running apps.
- `Dock.qml` always lists pinned apps first, in pin order, and the running apps
  after them. It reads the dock settings straight from `shell.json`, because
  cloned plugins don't get the shell's config object.
- `manifest.json` declares the `menu` kind, with a placeholder `Menu.qml`. That
  is what gives a third-party plugin access to the shell's app library, which is
  used to launch apps, load icons and build the app grid.

## Credits

Based on the dock from [Omarchy](https://github.com/omacom/omarchy) (MIT).

## License

[MIT](LICENSE)
