Standard Library contains functions to:

- automate toggle buttons,
- strip color codes from text,
- etc.

## Calls of Utilities Library

Below are descriptions of Utilities Library functions. Arguments in triangular brackets are
mandatory, in square brackets – optional.

---

### -zui_util_map_bools
```Cirru
-zui_util_map_bools <expressions> <parameters> <true string> <false string>
```

Maps boolean values of expressions given in `$1` (string with entries separated by ';') to parameters given
via names in `$2` (separated by ';'). For true, `<true string>` is assigned to corresponding parameter,
`<false string>` for false.

Example call:

```Cirru
local color1 color2 color3
-zui_util_map_bools "1;[[ a = b ]];ZUI[text_select]" "color1;color2;color3" $red $white
```

Parameter `color1` will be set to `$red`, `color2` to `$white`, `color3` will be assigned depending on `$ZUI[text_select]` value. Use this to automate toggle buttons – highlight the buttons with one of two colors, depending on state of a backend variable.

---

### -zui_util_strip_codes
```Cirru
-zui_util_strip_codes <text>
```

Strips formatting codes from text in, saves result into parameter `REPLY`.

---

### -zui_util_get_time
```Cirru
-zui_util_get_time
REPLY="H:M time string"
```
Returns time in format `%H:%M`, via `datetime` module (fast) or `date` command as fallback

---

### -zui_util_get_datetime
```Cirru
-zui_util_get_datetime
REPLY="Ymd_H.M.S date string"
```

Returns date and time. Uses `datetime` zsh module (fast) or `date` command as fallback.

---

### -zui_util_get_timestamp
```Cirru
-zui_util_get_timestamp
REPLY={seconds}
```

Returns timestamp, via `datetime` module (fast) or `date` as a fallback.

---

### -zui_util_has_default_color
```Cirru
-zui_util_has_default_color
```

Returns true if the "default" color can be used with current `Zsh`/`zcurses`.

---

### -zui_util_resolve_path
```Cirru
-zui_util_resolve_path <current working directory> <file path>
reply[1]={dir-name}
reply[2]={base-name}
```

Resolves absolute path to file from `<current working directory>` and `<file path>`. Returns the
path as two components, dir-name in `reply[1]`, base-name in `reply[2]`.

---

### -zui_util_to_cmd_line
```Cirru
-zui_util_to_cmd_line <text>
```
Puts given text on command line - regardless of Zle being active or not

---

### -zui_util_circular_next
```Cirru
-zui_util_circular_next <base> <size>
REPLY={path}
```

Returns next file to write to in circular buffer set of file names `<base>.1` `<base>.2` ...
`<base>.<size>`. The buffer is ordered according to modification time – oldest file from the set is
the returned one (so after write the circular buffer updates). Files are located in
`~/.config/zui/var/circular_buffers`.

---

### -zui_util_circular_paths
```Cirru
-zui_util_circular_paths <base>
reply=( {path1} {path2} ... )
```

Returns absolute file paths of given circular buffer. The paths are ordered from most recent to
least recent. No count is obtained, so all files are returned, even actually disabled by any used
`<size>` (with `-zui_util_circular_next`).
