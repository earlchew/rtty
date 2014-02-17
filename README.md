rtty
====

Start remote terminal sessions in a separate window

#### Background

When starting remote terminal sessions using programs like `ssh` or `mosh`,
it is convenient to have them interact with the desktop in the following ways:

* A new terminal window per connection
* A colour scheme that differentiates this remote connection, especially
from local consoles

`rtty` is a wrapper that creates a new terminal window, customises the scheme,
and launches a remote terminal session within that new window.

![](https://raw2.github.com/earlchew/rtty/master/example.png)

#### Requirements

* Python 2.6, 2.7
* OSX or X11

#### Installation

`rtty` is a single Python script that can be installed anywhere by
copying it into place, as required. One good approach would be to
retrieve the package using `git clone` and to create a symbolic link
to it from your local `~/bin` directory.

##### Direct Use

If `rtty` is invoked directly, the first argument is used to name the
remote terminal application to run. Examples:

* `rtty ssh host.example.com`
* `rtty mosh host.example.com`

In these examples, `rtty` will create a new terminal window, then
create a remote terminal session to `host.example.com` using the
indicated application.

##### Symbolic Link

Another approach is to create a symbolic link using the name of the
desired application:

* `ln -s ~/pkg/rtty.git/rtty ~/bin/ssh  # Equivalent to rtty ssh`
* `ln -s ~/pkg/rtty.git/rtty ~/bin/mosh # Equivalent to rtty mosh`

Once these symbolic links are in place, and assuming that `PATH` is
configured to search the directory containing the symbolic links, you
can invoke the script using:

* `ssh host.example.com`
* `mosh host.example.com`

#### Supported Applications

* `ssh`
* `mosh`

#### Supported Terminals

* OSX Terminal
* OSX ITerm2
* X11 xfce-terminal
* X11 gnome-terminal

#### Additional Command Line Options

`rtty` accepts the following command line usage:

> rtty [appname] [rttyopts ...] [appopts ...] [user@]host [cmd ...]

* `appname` specifies the name of the remote terminal application.
* `rttyopts` specifies the options specific to `rtty`.
* `appopts` specify the options that are specific to the chosen
remote terminal application.

The following options are specific to `rtty`:

* `-geometry WxH` | `-geometry WxH+X+Y`
Specify the width and height of the new terminal window. Under X11,
the second form `WxH+X+Y` can also be used to position the new
terminal window in addition to providing its new size.

#### Configuration Files

`rtty` currently leverages `~/.ssh/config` to associate the newly
created remote terminal session with a session specific
configuration. As described in `ssh_config(5)`, the `ssh`
configuration file will associate particular configuration items with
specific `Host` entries, and also provide global or default
configuration items.

`rtty` parses the `ssh` configuration file, and in addition to the
`ssh` specific items, also reads items with keys beginning with a
`#@-rtty-` prefix:

* `#@-rtty-Geometry` Specify the geometry of the new terminal window using
`WxH`, or `WxH+X+Y` notation. The X and Y offsets are only used for
deployments running X11 configurations.

* `#@-rtty-Osx-Colour-Scheme` | `#@-rtty-Osx-Color-Scheme` Specify the OSX
colour scheme for normal foreground text in OSX terminal applications.

* `#@-rtty-Gnome-Colour-Scheme` | `#@-rtty-Gnome-Color-Scheme` Specify the
name to be supplied to the `--profile` option in `gnome-terminal`.

* `#@-rtty-Xfce4-Colour-Scheme` | `#@-rtty-Xfce4-Color-Scheme` Specify the
name of the directory that contains the desired xfce4 profile. A
leading `~/` will be replace with the content of the `HOME` environment
variable. The named directory will be set into the environment variable
`XDG_CONFIG_HOME`.
