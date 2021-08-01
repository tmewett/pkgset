**pkgset** is a program for managing the packages on a Linux system in
user-defined sets.

On a typical system, the only thing I know about a package whether it was
installed explicitly, or installed as a dependency. However, as a user, I care
about a lot more information - I want to know:

*   why I installed something, and when I can remove it
*   if I need it together with some related packages which might not depend on
    each other
*   the 'scope' of the package; whether it's for the system or if it's an app /
    something I personally want.

Organising packages in named sets gives a flexible way to manage all this and
more.

## Installation

pkgset currently supports:

*   Ubuntu
*   Arch Linux
*   Debian

It is a single executable script, with one dependency: Ruby. So to install it,
simply install Ruby and copy pkgset to a directory in your path.

## Usage

### Setup

pkgset manages _package sets_ - named collections of packages. When a set is
"installed," all its contained packages are explicitly installed. You can create
and modify sets from the command line, or directly via files.

pkgset assumes that all the explicitly-installed packages on your system are
each in a set - so the first time you use pkgset on a system, you will have to
do some setup.

    $ pkgset unadded

This command will show all as-yet-unadded packages on your system.

One way to set up is to add all packages to a **n**ew, **i**nstalled set, like
so:

    $ pkgset unadded | xargs pkgset add -ni base

We can see our created set 'base', and the '*' which indicates it's installed:

    $ pkgset list
    * base

Now you can start breaking the 'base' set up into other sets, by using the
**m**ove flag:

    $ pkgset add -mni apps my-web-browser my-text-editor
    $ pkgset add -mni my-project thing gagdet gizmo

    $ pkgset list -t
    * apps
        my-web-browser
        my-text-editor
    * base
        ...
    * my-project
        thing
        gagdet
        gizmo

How you choose to arrange your sets is completely up to you!

### Adding and removing

To install packages, we add them to an (existing or new) installed set.

    $ pkgset add apps some-new-app   # existing
    $ pkgset add -ni my-other-project package1 package2   # new

The opposite to the `add` command is `remove`.

If we're done with a set, we can uninstall it:

    $ pkgset uninstall my-project
    ...package manager working...
    $ pkgset list
    * apps
    * base
      my-project

When you add a package to an installed set, pkgset makes sure it is installed on
the system by running the package manager. Similarly, when you remove a package
or uninstall a set, pkgset checks if the packages are in any other installed
sets and if not, *marks them as unneeded.* You need to do the actual removal
yourself with the package manager; see `pkgset remove -h`.

### Declarative

If you prefer, you can manage the contents of your sets by editing the files in
`/etc/pkgset/sets` instead of using the commands. You can even annotate the
files with comments beginning with `#`. When done, run `pkgset apply` to update
the configuration.

### More info

For more information, use `-h` or `--help` with any command. `pkgset -h` shows
all commands.
