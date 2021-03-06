# Tag Groups for HLWM

This is a rudimentary implemetation of "tag groups" for [herbstluftwm](https://herbstluftwm.org/index.html).  This uses [rofi](https://github.com/davatorium/rofi) for interactive input, though you can also use shell commands (or extend `prompt`, see below).

![Prompt using rofi](https://github.com/rpav/hlwm-tag-groups/blob/assets/doc/im/example1.png)

Tag groups are a way to better organize your tags/desktops if you work on multiple projects or tasks at any given time.   For example, perhaps your current set of projects looks like following:

  * Main application project
  * Assets for main project
  * Library sub-project A
  * Library sub-project B
  * Utility side project A
  * Utility side project B
  * Miscellaneous side tasks (browser, mail, etc)

Consider that each of these might have multiple associated editor windows and desktop for *each* .. and suddenly merely having 5-10 desktops isn't enough.  You want 5-10 desktops for _every task_, with an easy way to switch between and manage tasks... and if it's not too much to ask, have shared desktops with "common" tasks such as your browser.

This is what tag groups provides with a few simple utilities:

  * *hlwm-tag-group-switch*: Create/switch to a new tag group
  * *hlwm-tag-group-merge*: Merge/delete a tag group
  * *hlwm-tag-group-switch-tag*: Switch _tags_ numerically or relatively within the current tag group
  * *hlwm-tag-group-list*: List tag groups
  * *hlwm-tag-group-unpocket*: "Pocket/unpocket" functionality (see below)

## Commands

The following commands are intended to be bound to some reasonable keybinding, but may be used directly from the shell as well.

### hlwm-tag-group-switch

A tag group consists of a series of tags, specified in a template file (defaulting to `~/.config/herbstluftwm/tag_group_tmpl.txt).  This may look like the following:

```
# Tag-or-template     Keybinding

GROUP_0               Alt-1
GROUP_1               Alt-2
common_0              Alt-3
```

When you `tag-switch-group`, you will be prompted for a group to switch to, or you can enter the name of a new group.  This name will be substituted for `GROUP` in the template, and the "new" set of tags will be bound to the switching keys.

For more extensive customization, see *Commands and Hooks* below.

### hlwm-tag-group-merge

When you're done with a group, you may want to close it or merge its windows with another group.  Using `hlwm-tag-group-merge -c` will do the latter.  (However due to the way `hlwm` works, one must still specify a group to merge-to.)

If the "close" option is specified, only _templated_ tags, i.e. not "shared" tags, will have their windows closed.

## hlwm-tag-group-switch-tag

Within a group, you may wish to switch to the nth tag, or switch +n/-n (e.g., "next desktop" / "previous desktop" style commands).  This lets you do that:

``` console
# Switch to the next tag in the group, but don't wrap around:

$ hlwm-tag-group-switch-tag -n +1 --no-wrap

# Switch to the previous tag in the group, or go to the last tag, if already at the first:

$ hlwm-tag-group-switch-tag -n -1
```

You can bind these to the keys of your choice.

## hlwm-tag-group-unpocket

At times you may wish to move a handful of windows between tag groups (or just tags).  Pocket/unpocket lets you "put windows in a pocket" then retrieve them all later, somewhere else.  For this you need the following:

```shell
# In your autostart:

POCKET=pocket

hc add $POCKET                         # Add a `pocket` tag
hc keybind $Mod-z move $POCKET         # Or whatever key you'd prefer
hc keybind $Mod-Control-z /path/to/hlwm-tag-group-unpocket $POCKET
```

Now pressing `$Mod-z` will "pocket" the current window, and `$Mod-Control-z` will move _all_ pocketed items to the current tag.

## Commands and Hooks

### Commands

A more extensive template may also include commands:

```
# Tag-or-template     Keybinding     Commands...

GROUP_0               Alt-1          focus_monitor 0 . use T
GROUP_1               Alt-2          # defaults to `use T`
common_0              Alt-3
```

Commands are specified as a `chain .` with `T` substituted as the tag name for the current group/tag template.  E.g. if `GROUP_0` were specified as the template, and the group name were `foo`, then `T` would be `foo_0`.


### Hooks

You may want to perform further actions if a group is switched or merged.  In this case, if the script `~/.config/herbstluftwm/tag_group_hook` is called if present/`+x`.  This is called with arguments as follows:

  * On switch: `tag_group_hook --switch GROUP-NAME`
  * On merge: `tag_group_hook --merge SRC-GROUP DEST-GROUP`

One use of this is for resetting or changing rules when the group changes.


## Extending

There are many potential unimplemented features, robustness, and polish.  To some degree the scripts have been separated to make this easier.  The following "library" scripts are included, and may be invoked by `$hlib/<name>` in scripts:

  * *hc*: Shortcut for `herbstclient`
  * *prompt*: Interactively prompt for input
  * *setup*: This ought to be set up and invoked for scripts, as it sets a number of useful variables

Other things _ought_ to be moved to library scripts, but have not yet.

### `prompt`

It may be useful to extend this and break it into a number of client-specific options.  Currently, `prompt` is implemented for `rofi`, but it ought to be straightforward to add others:

  * Test for client availability in `prompt`
  * Add `prompt-<CLIENT>`
  * Call what's available
  * Support the common arguments `-i`, `-m`, `-p`, and `-x`, as `prompt-rofi` does


## License

GPL3

Copyright (C) 2021, Ryan Pavlik

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program.  If not, see <https://www.gnu.org/licenses/>.
