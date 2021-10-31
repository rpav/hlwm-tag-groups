# Tag Groups for HLWM

This is an rudimentary implemetation of "tag groups" for [herbstluftwm](https://herbstluftwm.org/index.html).  This uses [rofi](https://github.com/davatorium/rofi) for interactive input, though you can also use shell commands (or extend `prompt`, see below).

![Prompt using rofi](https://github.com/rpav/blob/assets/doc/im/example1.png)

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

### hlwm-tag-group-merge

When you're done with a group, you may want to close it or merge its windows with another group.  Using `hlwm-tag-group-merge -c` will do the latter.  (However due to the way `hlwm` works, one must still specify a group to merge-to.)

If the "close" option is specified, only _templated_ tags, i.e. not "shared" tags, will have their windows closed.


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
