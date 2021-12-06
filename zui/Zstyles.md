To change ZUI global default, invoke:

```zsh
zstyle ":plugin:zui" colorpair "white/black"
```

An application may override such default with its own default. To change default per-application, invoke:

```zsh
zstyle ":plugin:zui:app:zui-demo-fly" colorpair "250/17"  # 256 colors – zsh >= 5.3; "default" color also from this version
```

Below is complete list of available Zstyles with ZUI default values.

| ZStyle name | Default | Description |
|-------------|---------|-------------|
| colorpair        | "white/black"    | Default text and background color. For Zsh >= 5.3, color "default" is available, it might be e.g. transparent (depends on terminal configuration) |
| border           | "no"             | No border around main window |
| border_cp        | "yellow/black"   | Border (and header) color |
| bold             | "no"             | No default bold |
| status_colorpair | "white/black"    | The same as "colorpair", but for status window |
| status_border    | "no"             | No border around status window |
| status_border_cp | "green/black"    | Border color of status window |
| status_bold      | "no"             | No default bold in status window |
| mark             | "red reverse lineund" | String starting with one or two color names continued with combination of: reverse, underline, blink, bold, lineund, linerev. Last two underline, reverse whole active line. The rest **mark active button**. Uppercase color names are for background |
| altmark          | "red reverse"    | As "mark", but for terminals without underline support |
| mark2            | "yellow reverse" | The same as "mark", but for buttons with background color |
| altmark2         | "yellow reverse" | The same as "altmark", but for "mark2", i.e. mark for buttons with background color, on terminals with no underline support |
| status_size      | 4                | Height of status window, including border (drawn or not) |
| status_pointer   |"yes"             | Show line indicating position in document |
| log_append       | "above"          | Put log messages on top of others. Also available: "below" |
| log_time_format  | "[%H:%M] "       | Display hour:minute time stamp. Set to "" to disable time stamp completely |
| log_index        | "yes"            | Show order number of log messages |
| log_size         | "32"             | How many log messages to keep in memory |
| top_anchors      | "yes"            | Show anchors to each module instance at top |
| log_colors       | "white cyan yellow green cyan red magenta yellow blue" | The colors used for log messages. First two are for message's index and time stamp |
| select_mode      | "no-restart"     | What to do on non-hyperlink selection. Such selection will invoke text-select callback with segment or whole line passed as argument. Plus, it will set ZUI[line_selected] or ZUI[pure_text_selected]. If the Zstyle is set to "restart" then list restart will be performed. If set to "quit" then event loop will be exited, and REPLY will be set to line or segment. |
| text_mode        | "all"            | Navigate across each bit of text, not only hyperlinks. "hyp" – only at lines with hyperlinks, "nohyp" – only at lines with no hyperlinks, "off" - text-bit navigation fully turned off |
| text_select      | "yes"            | Allow selection on non-hyperlinks (full lines when text_mode is "off" or "hyp" – meaning text-bit mode fully turned off or enabled only for lines with hyperlinks, leaving text-only lines undivided) |
| timeout          | "-1"             | No calls to timeout callback. Denotes milliseconds. Minimum value is 200. Time is counted when there is no user input |
| palette          | "black:red:green: yellow:blue:magenta: cyan:white" | 8-color palette used by ZUI. Default is normal ANSI palette. Can be changed to indexes of 256 colors (zsh >= 5.3) |
