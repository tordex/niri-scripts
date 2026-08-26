# Niri Scripts by @tordex

## niri-pocket

### What niri-pocket does

The `niri-pocket` script adds scratchpad-like functionality to Niri. You can add windows to a pocket and toggle the visibility of all windows in a pocket with a hotkey.

Features:

* Unlimited number of pockets
* Supports both floating and tiled windows
* Sends desktop notifications on errors and when adding windows to a pocket
* Each pocket has a name that is shown in notifications
* Additional commands to make sidebar or centered window
* Automatically resizes sidebar and centered windows when moving them to monitors with different dimensions

### How it works

Unlike similar scripts, `niri-pocket` does not use a named workspace; it uses a virtual monitor instead. First, you need to set up a virtual monitor by following these instructions: [one](https://dev.to/foxes/how-to-get-virtual-display-outputs-on-linux-without-using-a-dummy-plug-50d3), [two](https://www.reddit.com/r/linux_gaming/comments/199ylqz/streaming_with_sunshine_from_virtual_screens/).

When you hide the windows in a pocket, `niri-pocket` moves them to the virtual monitor. Because this monitor is invisible, all of its windows become invisible as well. Although you can still access these windows with the Alt-Tab hotkey, this is better than using a workspace. At least this allows `niri-pocket` to work with dynamic workspaces.

### Setup niri pockets

Download `niri-pocket` and copy it to your computer. Then add it to Niri's startup:

```
spawn-at-startup "~/.config/niri/niri-pocket"
```

Set up keybindings for the pockets: one for adding a window to a pocket and another for toggling a pocket:

```
Mod+F5 { spawn-sh "echo toggle-pocket F5 DP-3 | nc -U /tmp/niri_pocket.sock"; }
Mod+CTRL+F5 { spawn-sh "echo to-pocket F5 DP-3 | nc -U /tmp/niri_pocket.sock"; }
Mod+F6 { spawn-sh "echo toggle-pocket F6 DP-3 | nc -U /tmp/niri_pocket.sock"; }
Mod+CTRL+F6 { spawn-sh "echo to-pocket F6 DP-3 | nc -U /tmp/niri_pocket.sock"; }

Mod+KP_Left { spawn-sh "echo sidebar left 0 0 40 0.35 | nc -U /tmp/niri_pocket.sock"; }
Mod+KP_Right { spawn-sh "echo sidebar right 4 0 40 0.35 | nc -U /tmp/niri_pocket.sock"; }
Mod+KP_Begin { spawn-sh "echo center 0.6 0.6 | nc -U /tmp/niri_pocket.sock"; }
```
Note that you must use the `nc` command to send commands to the `niri-pocket` daemon.

### Commands

| command | arguments | description |
|---------|-----------|-------------|
| `to-pocket`     | `pocket_name:str`<br>`monitor_name:str`  | Add focused window to the pocket |
| `toggle-pocket` | `pocket_name:str`<br>`monitor_name:str`  | Toggle pocket visibility         |
| `center`     | `width:float`<br>`height:float`  | Make a window floating and center it. Width and height must be numbers from 0 to 1. The window size is calculated by multiplying the monitor dimensions by the specified values.  |
| `sidebar`     | `align:str`<br>`offset_x:int`<br>`offset_top:int`<br>`offset_bottom:int`<br>`width:float`  | Make a window floating and align it to the left or right side. Width must be a number from 0 to 1. The window size is calculated by multiplying the monitor dimensions by the specified values. Offsets are defined in pixels. `offset_x` defines the window's offset from the left or right side. The `align` argument defines the window's alignment and must be `left` or `right`.  |

### Usual workflow

1. Run the application you want to add to the pocket.
2. Press `CTRL+Mod+F5` to add the window to the pocket. The window will be hidden if the pocket is hidden.
3. Add more windows to the pocket by pressing `CTRL+Mod+F5` as needed.
4. Press `Mod+F5` to bring the windows in the pocket to the focused monitor.
5. Press `Mod+F5` again to hide all the windows in the pocket.

Note that `niri-pocket` saves the focused window when hiding a pocket. When you show the pocket, the same window will be focused. However, if a window outside the pocket was focused when the pocket was hidden, the focused window will not change when the pocket is shown.