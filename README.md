# What is alloy?

alloy is a utility and paired library that creates a script meant to call
many other scripts.  It requires a special directory refered to as the
alloy_root.  This directory will have at minimum a /bin or a /hooks directory.
The created script is known as the "handle".

From this point on, all absolute paths are considered to have the alloy_root as
the root directory.  IE the bin directory in the alloy_root will be shown as
/bin.

If there is no executable /hooks/main file, when the handle is called it will
operate in arg-command mode.  The first argument to the handle will be used as
a script name, and it will look in /bin for that script and will attempt to
execute it.  The script will recieve all remaining arguments.

There are two valid hooks, default and main.  When no arguments are given, and
there is no main hook, then the default hook will be run.  When there is a
main hook that is executable it will be run whenever the handle is called, and
will recieve all the arguments.

# Why?

There are two benefits to doing this.  The first is simply to provide a clean
way to collect a bunch of similar scripts together, these scripts don't need
to be written in the same language, some can even be compiled binaries.  The
second purpose is to provide a simple means for those scripts to call each
other, this is done with the `. $_alloy` command available to bash and xs
scriptlets.  This effectively solves the "bash scripts can't find their source
path" problem.

The scriptlets can be written in multiple different languages, or could be
compiled binaries if necessary.

# Example

`alloy ~/mroot ~/bin/handle` where `~/mroot` is a directory that contains the
`bin/` directory with the scriptlets you want to call with the handle.  The
alloy handle will be made.  You can then run `handle goat` which will call
`~/mroot/bin/goat` with all the proper environment.

# Terms

* `handle`: The program created by the `alloy` command
* `scriptlet`: An executable program or script in /bin or /sbin
* `hook`: An executable program in /hooks
* `alloy_root`: The directory provided when running the alloy command
* `xs`: A shell like bash
* `bash`: A shell like xs

# Installation

Copy `lib` to `/usr/lib/alloy`, copy `bin/*` to a directory in your $PATH.

# . $_alloy functions

## bin

bin will call any scriptlets in bin as if the handle were called.  Except it
has no logic to call any hooks, it will just blindly call the scriptlet.

## sbin

As `bin` except it will look in /sbin instead of /bin

## lib

Will source each of the arguments in /lib.  Thus `lib a b c` will source
/lib/a, /lib/b, and /lib/c.

## sudobin and sudosbin

As `bin` and `sbin` except will prefix them with sudo.
