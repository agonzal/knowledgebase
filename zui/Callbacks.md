Many situations are solved by callbacks. This eases the API, you don't have to e.g. supply a
function name in a call but instead just define a function. Callback names follow scheme
`-zui-standard-*-callback`. They are automatically cleared at cleanup (i.e. at `-zui_std_cleanup`
call).

---

### -zui-standard-timeout-callback
```Cirru
-zui-standard-timeout-callback
```

Called when `$ZUI[timeout]` milliseconds pass without user input (the timeout defaults to `-1`, i.e.
no timeout defined). No arguments, and the return value is not checked. Redraw of screen might be
invoked by setting `$ZUI[timeout_update]` to `1`. Regeneration of module can be scheduled by
invoking `-zui_std_fly_mod_regen`, this implies setting `$ZUI[timeout_update]`. `$ZUI[timeout]` is a
Zstyle of the same name.

[zui-demo-timeout](https://github.com/zdharma/zui/blob/master/demos/zui-demo-timeout) covers this
callback.

---

### -zui-standard-text-select-callback
```Cirru
-zui-standard-text-select-callback <type> <text>
```

Called when a text is selected. This is possible when Zstyle `text_select` is `1`. The Zstyle
`text_mode` can be `off` – only whole lines can be then selected. `<type>` is then set to string
`line`. If `text_mode` is `hyp`, then text-segments at lines with hyperlinks can be selected. When
it is `nohyp` then this applies to lines without hyperlinks. Value `all` allows selection of text
segments at all lines, with or without hyperlinks. If a text-segment is selected, `<type>` is set to
string `segment`.

---

### -zui-standard-global-anchors-callback
```Cirru
-zui-standard-global-anchors-callback <id> <initial line> <module index> <instance index>
```

Invoked when a global-anchor is pressed. Global anchors are typically at first line of document,
controlled by Zstyle `top_anchors`. First argument `<id>` is the ID of the anchor button, in format
`aglobal_m<module-index>_i<instance-index>`. Second argument `<initial line>` is set to destination
line used when creating the anchor – it might have been changed by dynamic updates to document, i.e.
module-regeneration that shifts target lines up or down. `<module index>` and `<instance index>`
specify to which module-instance the anchor is jumping to.

---

### -zui-standard-status-callback
```Cirru
-zui-standard-status-callback 0 <selectable> <uniq> <search> <line> <segment>
-zui-standard-status-callback 1 <selectable> <uniq> <search> <line> <segment> ...
```

Called after each key-press and also when timeout-callback schedules document update – `$ZUI[timeout_update]`
is then `1`. First argument can be `0` or `1` and it is the type of active segment – `0` is case:
no-hyperlink-active.  `<selectable>` **/** `<uniq>` **/** `<search>` are `0` or `1` and denote if current line
is selectable **/** if uniq mode is enabled **/** if there is a search query entered. Arguments `<line>` and
`<segment>` are current line and segment.

For variant with `1` in first argument, what follows is current-hyperlink decoded data. For anchors and
buttons, it is (follows code to be used to read the input):

```Cirru
local id="$7" data1="$8" data2="$9" data3="$10" data4="$11"
```

For text-fields, it is:

```Cirru
local id="$7" width="$8" index="$9" text="$10" data1="$11" data2="$12" data3="$13"
```

For list-boxes, it is:

```Cirru
local id="$7" width="$8" index="$9" options_text="$10" data1="$11" data2="$12" data3="$13"
```

You can test for `tfield` in ID of a text-field, and for `lbox` in ID of a list-box. These strings are
prepended to the IDs that you use when creating those hyperlinks. You can then read the whole data as follows:

```Cirru
local tpe="$1" selectable="$2" uniq="$3" search="$4" line="$5" segment="$6"
shift 6
[[ "$1" = *(tfield|lbox)* ]] && local id="$1" width="$2" index="$3" text="$4" data1="$5" data2="$6" data3="$7" ||
                                local id="$1" data1="$2" data2="$3" data3="$4" data4="$5"
```

Main function of status callback is adding message to status window. It should return `1` and set array
`reply` to add the message. For example:

```Cirru
reply=( "My " "new " "message" )
return 1
```

Returning `0` means not-updating status window, and `reply` is then ignored.
