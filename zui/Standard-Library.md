Standard Library contains functions to:

- initialize and cleanup an application,
- load and set application's configuration,
- create hyperlinks (buttons, anchors, text fields, list boxes),
- handle hyperlinks (e.g. check if given text is a hyperlink),
- control document regeneration on-the-fly (the DHTML-like way),
- handle modules (e.g. read module's position in document),

## Calls of Standard Library

Below are descriptions of Standard Library functions. Arguments in triangular brackets are
mandatory, in square brackets – optional.

---

### -zui_std_init
```Cirru
-zui_std_init [app:"application ID"] [app_name:"Application name"]
```

Initializes application. To be called **before emulate**. Optional argument `app:...` will set
`ZUI[app]` – hash field needed by any application. Argument `app_name:` does the same for
`ZUI[app_name]` (it is a human-readable application name, displayed in header).

---

### -zui_std_init2
```Cirru
-zui_std_init2
```

Initialization to be called **after emulate**. `emulate` is the command that makes a function an
independent program and each ZUI application should use it.

---

### -zui_std_stalog
```Cirru
-zui_std_stalog <Text 1> [Text 2] ...
```

Appends message to the status window logs. Each text argument has a color assigned – see
the `log_colors` [Zstyle](https://github.com/zdharma/zui/wiki/Zstyles), it controls the colors.

---

### -zui_std_special_text
```Cirru
-zui_std_special_text <text> [output array]
reply+=( "{output string}" )
```

Quote special characters in text. This allows to use strings like `That's` in document – special
character `'` will not disturb content. Default output array is `reply`.

---

### -zui_std_button_ext
```Cirru
-zui_std_button_ext <ID> <data1> <data2> <data3> <data4> <button text> [handler] [output array]
```

Creates string with button. Every button has an ID assigned – it is the first argument. Then go four
user-data arguments – if handler will be invoked, the user-data will be passed to it along with the
ID. `<button text>` is the label of the button. `[handler]` is the function name or inline code to
be called at press of the button. `[output array]` name can be provided to have result appended to
that array (the default array is `reply`).

If handler has substring "internal" in it (in function name or in inline code), then it will be
invoked without list restart. Otherwise, a list restart will be performed (this is like invoking
JavaScript without web page reload, or doing the reload and calling code on the server side).

---

### -zui_std_rc_button_ext
```Cirru
-zui_std_rc_button_ext <ID> <data1> <data2> <data3> <data4> <button text> [handler] [output array]
```

Function works identically to `-zui_std_button`, but it wraps button text in square brackets – "rc"
is for "rectangular".

Also, both functions have no-`_ext` versions that do not have `<data1>`...`<data4>` arguments.

---

### -zui_std_anchor
```Cirru
-zui_std_anchor <ID> <index> <data1> <data2> <data3> <button text> [handler] [output array]
```

Creates an anchor – a hyperlink that moves cursor to specified line. Appends it to `[output array]`
(a parameter specified by name) – `reply` by default. `<index>` is the line number to jump to. It is
relative to current module. It in general cannot point to absolute line number in document. To point
to line outside the module, use `A+B` syntax, e.g. `1-2`, to jump `2` lines before first line of the
module.

Instead of handler you may use `<data2>` and `<data3>` to pass a module regeneration instruction.
For example, pass `",mod2_ice1,"` `"arg"` to regenerate some module numbered 2, instance 1, with passed
user-data "arg". This regeneration is with list restart (i.e. it is like web page reload with `arg`
passed to script on the server side).

If handler has substring "internal" in it (in function name or in inline code), then anchor will be
internal – will not cause document reload. Anchor of which `<data2>` matches `,*,` is set to be
external.

Example call:

```Cirru
-zui_std_anchor regen1 4 "" ",mod1_ice1," $RANDOM "[${ZUI[MAGENTA]}Regen${ZUI[FMT_END]}]"
```

This instructs to regenerate module `1` instance `1`, with no handler call, with `$RANDOM` as
generator's third input - regeneration user-data. `4` is the line number on which the cursor will be
placed after the regeneration. Note that any generator call has instance ID (mod and ice) in `$1`
and `$2` by the design of restart-regeneration loop.

---

### -zui_std_text_field
```Cirru
-zui_std_text_field <ID> <width param> <index param> <text param> <data1> <data2> <data3> [handler] [output array]
```

Creates text-field. Every text-field has width, given in indirect way, by supplying **name of
variable** holding the width number. In the same way start-index is to be provided – it specifies
from which character the text should be displayed (so it can e.g. start from 5th character).
`<text param>` is name of variable holding the string that the text-field contains.

Handler will be called on accept event (i.e. Enter-press; Cancel is ESC-press, it restores previous
text-field contents).

---

### -zui_std_list_box
```Cirru
-zui_std_list_box <ID> <width param> <index param> <options param> <data1> <data2> <data3> [handler] [output array]
```

Creates list-box. Every list-box has a text-width that it will occupy in document regardless of
option's text length. This width is specified via **name of variable** holding the width number.
Current-selected option is `<index param>` – also a variable name. Options are specified by
`;`-separated string, put in a variable whose name is passed as fourth argument (`<options param>`).

Handler will be called on accept event (i.e. Enter-press; Cancel is ESC-press, it restores previous
list-box current option).

---

### -zui_std_get_ganchor
```Cirru
-zui_std_get_ganchor <module index> <instance index> <button text>
```

Anchors cannot use global indexes and easily point to other modules. However, there are `top
anchors` that point to each module. The top anchors can be hidden. However they are always
accessible by this function. It fetches anchor pointing to module `<module index>`, instance
`<instance index>`. The anchor will be having specified `<button text>`. You can use it as any other
anchor, with the notable fact that handler cannot be specified, however a callback will be called on
anchor's press: `-zui-standard-global-anchors-callback()`, with anchor's ID in `$1`, line number in
`$2`, module index in `$3`, instance index in `$4`.

---

### -zui_std_decode_hyperlink
```Cirru
-zui_std_decode_hyperlink <hyperlink string> [output array]
array=( ID data1 data2 data3 data4 )
```

Decodes given hyperlink (anchor, button, raw link). Its ID and user-data are placed in array given
by name (default is `reply` array). Testable.

---

### -zui_std_decode_text_field
```Cirru
-zui_std_decode_text_field <hyperlink string> [output array]
array=( ID width-param index-param text-param data1 data2 data3 )
```

Decodes given text-field hyperlink. Its ID, names of backend-parameters and user data are placed in
array given by name (default is `reply` array). Testable.

---

### -zui_std_decode_list_box
```Cirru
-zui_std_decode_list_box <hyperlink string> [output array]
array=( ID width-param index-param options-param data1 data2 data3 )
```

Decodes given list-box hyperlink. Its ID, names of backend-parameters and user data are placed in
array given by name (default is `reply` array). Testable.

---

### -zui_std_decode
```Cirru
-zui_std_decode <hyperlink string> [output parameter] [output array]
array=( {data decoded from hyperlink} )
parameter=1|2|3
```

Tries various decoding functions (for regular hyperlinks, text-fields, list-boxes). Testable.
Returns (in `REPLY` or specified parameter) 1 if recognized regular hyperlink (anchor, button, raw
link), 2 if text field, 3 if list-box. Will return 0 for unrecognized string, however the function
is testable so normal return value test can be performed.

---

### -zui_std_get_stext
```Cirru
-zui_std_get_stext <special-text string> [output parameter]
REPLY={text}
```

Output variable (default: `REPLY`) is set to text contained in the special-text string.

---

### -zui_std_is_hyperlink
```Cirru
-zui_std_is_hyperlink <hyperlink string>
```

Checks if given string is a regular hyperlink (anchor, button, raw link). Testable (true – the
string is a correct hyperlink).

---

### -zui_std_is_text_field
```Cirru
-zui_std_is_text_field <hyperlink string>
```

Checks if given string is a text-field. Testable (true – the string is a correct text-field).

---

### -zui_std_is_list_box
```Cirru
-zui_std_is_list_box <hyperlink string>
```

Checks if given string is a list-box. Testable (true – the string is a correct list-box).

---

### -zui_std_is_any_hyperlink
```Cirru
-zui_std_is_any_hyperlink <hyperlink string> [output parameter]
parameter=1|2|3
```

Checks if given string is any possible hyperlink, from anchor to list-box. Output parameter (`REPLY`
by default) will contain 1 if recognized regular hyperlink, 2 if text-field, 3 if list-box. For
unrecognized string it will contain 0, however the function is testable, so a regular return value
check can be performed.

---

### -zui_std_has_any_hyperlinks
```Cirru
-zui_std_has_any_hyperlinks <hyperlink string>
```

Similar to `-zui_std_is_any_hyperlink`, but doesn't return type of the hyperlink recognized.
Testable.

---

### -zui_std_load_config
```Cirru
-zui_std_load_config <variable> <default> <time limit> <output parameter>
```

Loads variable from configuration if it's older than e.g. `2` seconds (the `<time limit>` argument).
Time limit is used only if `<output parameter>` points to `ZUI` hash field, e.g.  `ZUI[text_mode]`.
Otherwise the configuration is always read regardless of time limit.

`<variable>` should have `b:` prefix for boolean type, `s:` for string type (`s:` is the default).
Boolean variables are mapped to just `0` or `1`, the same only values are accepted as `<default>`
value for that variable type. Example call:

```Cirru
-zui_std_load_config s:text_mode "off" 2 'ZUI[text_mode]'  # No text-segment navigation
```

The **Zstyle** variable is looked up at path `:plugin:zui`, then at `:plugin:zui:app:${ZUI[app]}`.
The latter path has higher priority.

---

### -zui_std_store_default_app_config
```Cirru
-zui_std_store_default_app_config <variable> <value>
```

Stores given variable to Zstyle **if** the variable is not already set. Can be used to set up
default configuration of application. User will be still able to set his own configuration, the
function will not overwrite it. Example call:

```Cirru
-zui_std_store_default_app_config b:top_anchors 0  # No top-anchors
```

---

### -zui_std_cleanup
```Cirru
-zui_std_cleanup [serialize|deserialize:"app"]
```

Clears the `ZUI` hash – all configuration fields, anchors, buttons, etc. Also, clears fields that
start with `my_` – this is the provided namespace to use by applications. If `serialize` given, will
store `my_*` fields and `zui-list` state fields into special `ZUI` field. It can be then retrieved
by `deserialize:"app"` – effect will be like if the application was never left.

---

### -zui_std_set_mod_factor
```Cirru
-zui_std_set_mod_factor <module index> <factor>
```

Sets how many instances of module given by index should be generated (the module-factor).

---

### -zui_std_get_mod_factor
```Cirru
-zui_std_get_mod_factor <module index> [output parameter]
```

Gets number of instances of module given by index. Stores result in `REPLY`, or in other parameter
specified by name.

---

### -zui_std_load_global_index_and_size 
```Cirru
-zui_std_load_global_index_and_size <module index> <instance index> [output param1] [output param2]
```

Loads where module is located (at which line in document) and what size it has (how many lines it
occupies). Stores to `REPLY` and `REPLY2` by default, or to specified parameters.

---

### -zui_std_reset_replies
```Cirru
-zui_std_reset_replies
```

Generators use parameters `reply`, `reply2`, `reply3`, `reply4`. This functions clears them.

---

### -zui_std_map_replies
```Cirru
-zui_std_map_replies
```

Generator output should be mapped onto parameters:

```
mod${midx}_ice${iidx}_output
mod${midx}_ice${iidx}_size
mod${midx}_ice${iidx}_nonselectables
mod${midx}_ice${iidx}_hops
mod${midx}_ice${iidx}_anchors
```

This function does this. It should normally not be needed, `-zui_std_fly_mod_regen` does this
automatically.

---

### -zui_std_fly_mod_regen
```Cirru
-zui_std_fly_mod_regen <module index> <instance index>
```

Schedules on-the-fly document-fragment update. This corresponds to DHTML, to doing
`document.getElementById('...').innerHtml=...`. No list restart is required (no "page reload").
Arguments `<module index>` and `<instance index>` specify which module instance should be
regenerated. The generator used to obtain new content is taken from `zui-event-loop` list (see
`-zui_std_fly_array_refresh`) and depends only on `<module index>`. In other words, the same
generator is used, the one normally assigned to module instance.

---

### -zui_std_fly_mod_regen_ext
```Cirru
-zui_std_fly_mod_regen <generator> <module index> <instance index>
```

The same as `-zui_std_fly_mod_regen`, but uses alternate, specified generator-function.

---

### -zui_std_fly_array_refresh
```Cirru
-zui_std_fly_array_refresh <module index>
```

Submits on-the-fly array refresh. The given `<module index>` should point to an array ("a:" prefix
at `zui-event-loop`). The array will be read again and pasted into document replacing previous
content. For example, in the history demo there is:

```Cirru
zui-event-loop 1:demo_generator_A a:u-history 1:demo_generator_A
```

Second module (and 1st instance) is array `history` that is made unique (the `u-` prefix). You can
refresh that content (document fragment) via:

```Cirru
-zui_std_fly_array_refresh 2
```
