Profiles
========

* [Differences Between GNOME Profiles](#differences-between-gnome-profiles)

Differences Between GNOME Profiles
----------------------------------

The most notable USE flag changes between Gentoo's and ours include:

1. dropping `qt3support` and `qt4`

    The former isn't something that should be enabled upstream, especially since things have moved to `qt4` and/or `qt5` by default. Users should decide for themselves if they want `qt3support`. As for the latter, it shouldn't be enabled globally at all in a GNOME environment, especially if `gtkstyle` is not enforced (and `gtkstyle` should be enforced regardless).

2. enabling `gtkstyle`

3. dropping `gtk`

    This is handled by default and on packages where it's not enabled it likely refers to optional GTK+-related support that does not enable a GUI.

The vast majority of the other USE flags dropped were there to obtain clean emerge output prior to `GNOME 3.14` and are no longer relevant.

Plumbing
========

* [Overview](#overview)
* [Classification](#classification)
* [Classification (`ConsoleKit`)](#classification-consolekit)
* [Classification (`elogind`)](#classification-elogind)
* [Implementation (`ConsoleKit`)](#implementation-consolekit)
* [Implementation (`elogind`)](#implementation-elogind)

Overview
--------

> **Note**: Ebuilds are interchangeable with any closely-related `Gentoo Linux` derivative.

[dantrell-gnome](https://github.com/dantrell/gentoo-overlay-dantrell-gnome)

    ├── app-admin
    │   ├── gnome-system-log
    │   └── system-config-printer
    ├── dev-scheme
    │   └── guile
    ├── net-im
    │   └── telepathy-mission-control
    └── sys-power
        ├── acpid
        └── upower

[dantrell-gnome-3-14](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-14), [3-16](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-16), [3-18](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-18), [3-20](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-20), [3-22](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-22), [3-24](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-24), [3-26](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-26) and [3-28](https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-28)

    ├── games-board
    │   └── aisleriot
    ├── gnome-base
    │   ├── gdm
    │   ├── gnome-control-center
    │   ├── gnome-session
    │   ├── gnome-settings-daemon
    │   ├── gnome-shell
    │   └── nautilus
    ├── media
    │   └── clutter
    ├── x11-terms
    │   └── gnome-terminal
    ├── x11-themes
    │   └── gnome-backgrounds
    └── x11-wm
        └── mutter

Classification
--------------

`Aesthetic` and `Performant`:

| Package                             | USE flag                  | Description
|-------------------------------------|---------------------------|------------
| app-admin/gnome-system-log          | N/A                       | The newer `gnome-extra/gnome-logs` depends on `sys-apps/systemd`
| app-admin/system-config-printer     | N/A                       | Regularly outdated
| dev-scheme/guile                    | N/A                       | Different implementation
| games-board/aisleriot               | N/A                       | Different implementation
| gnome-base/gnome-control-center     | -vanila-datetime          | Disables automatic datetime and timezone options
| gnome-base/gnome-control-center     | -vanila-hostname          | Disables changing hostname
| gnome-base/gnome-settings-daemon    | -vanilla-inactivity       | Restores old timeout for user inactivity
| gnome-base/gnome-shell              | +deprecated-background    | Restores old background code (avoids wallpaper corruption when resuming from suspend)
| gnome-base/gnome-shell              | -vanilla-gc               | Forces more frequent garbage collection
| gnome-base/gnome-shell              | -vanilla-motd             | Improves handling of motd when logging in (displays text in monospace and decreases the display time)
| gnome-base/gnome-shell              | -vanilla-screen           | Improves handling of screen blanking (uses the "Blank Screen" time as the idle delay)
| gnome-base/nautilus                 | -vanilla-icon             | Improves icons (adds another zoom level)
| gnome-base/nautilus                 | -vanilla-icon-grid        | Use old icon grid and text width proportions (makes text labels in icon view warp at ~16 characters)
| gnome-base/nautilus                 | -vanilla-menu             | Reorder context menu (primarily places "Open In Terminal" below "Open in New Tab" and other extensions below "Open With")
| gnome-base/nautilus                 | -vanilla-menu-compress    | Use old compress extension from File Roller instead of the native one
| gnome-base/nautilus                 | -vanilla-rename           | Support slow double click to rename (triggers rename mode by clicking an icon twice with a pause in between)
| gnome-base/nautilus                 | -vanilla-search           | Support alternative search (find without recursion by pressing `CTRL` + `SHIFT` + `F`)
| gnome-base/nautilus                 | -vanilla-thumbnailer      | Use FFmpeg as the video thumbnailer instead of Totem
| media-video/totem                   | -vanilla-thumbnailer      | Use FFmpeg as the video thumbnailer instead of Totem
| x11-terms/gnome-terminal            | +deprecated-transparency  | Restores background transparency¹
| x11-terms/gnome-terminal            | -vanilla-hotkeys          | Disables function keys (avoids clashes with command line utilities)
| x11-themes/gnome-backgrounds        | -vanilla-live             | Replaces Adwaita live backgrounds (800x600) with the set from GNOME 3.10 (2560x1440)
| x11-wm/mutter                       | +deprecated-background    | Restores old background code (avoids wallpaper corruption when resuming from suspend)
| x11-wm/mutter                       | +vanilla-mipmapping       | Disables mipmapping (trades scaled and smoothed window previews for a measurable performance increase)

> **¹** This patch is sourced from [Fedora](http://pkgs.fedoraproject.org/cgit/rpms/gnome-terminal.git/) with the notification aspects removed. ↩

Classification (`ConsoleKit`)
-----------------------------

`Critical`:

| Package                             | USE flag                  | Description
|-------------------------------------|---------------------------|------------
| gnome-base/gdm                      | +ck                       | Restores support for `sys-auth/consolekit`
| gnome-base/gnome-control-center     | +ck                       | Restores support for `sys-power/upower`
| gnome-base/gnome-session            | +ck                       | Removes dependency on an older version of `sys-power/upower`
| gnome-base/gnome-settings-daemon    | +ck                       | Restores support for `sys-power/pm-utils`
| gnome-base/gnome-shell              | +ck                       | Restores support for `sys-power/pm-utils`
| media-libs/clutter                  | N/A                       | Reorganizes backends (prioritizes X11)
| net-im/telepathy-mission-control    | +ck                       | Remove dependency on an older version of `sys-power/upower`
| sys-power/acpid                     | N/A                       | Restores support for GNOME's Power Management System
| sys-power/upower                    | +ck                       | Restores support for `sys-power/pm-utils`

Classification (`elogind`)
--------------------------

`Critical`:

| Package                             | USE flag                  | Description
|-------------------------------------|---------------------------|------------
| gnome-base/gdm                      | +elogind                  | Supports `sys-auth/elogind`
| gnome-base/gnome-control-center     | +elogind                  | Supports `sys-auth/elogind`
| gnome-base/gnome-session            | +elogind                  | Supports `sys-auth/elogind`
| net-misc/networkmanager             | +elogind                  | Supports `sys-auth/elogind`
| sys-apps/accountsservice            | +elogind                  | Supports `sys-auth/elogind`
| sys-apps/dbus                       | +elogind                  | Supports `sys-auth/elogind`
| sys-auth/pambase                    | +elogind                  | Supports `sys-auth/elogind`
| sys-auth/polkit                     | +elogind                  | Supports `sys-auth/elogind`
| sys-fs/udisks                       | +elogind                  | Supports `sys-auth/elogind`
| x11-wm/mutter                       | +elogind                  | Supports `sys-auth/elogind`

Implementation (`ConsoleKit`)
-----------------------------

`UPower`:

* Use `sys-power/upower` instead of `sys-power/upower-pm-utils`
* Reintegrate Power Management Utilities (`pm-utils`) support
* Have relevant packages depend on `>=sys-power/upower-0.99:=[ck]`

`GNOME`:

* Globally disable the `systemd` USE flag
* Replace the masked `openrc-force` USE flag with unmasked `ck` and `systemd` USE flags.
* Replace `sys-apps/systemd` dependency with this:

```sh
	ck? ( <sys-auth/consolekit-0.9 )
	systemd? ( >=sys-apps/systemd-186:0= )
```

or this (or equivalent):

```sh
	ck? ( >=sys-power/upower-0.99:=[ck] )
	systemd? ( >=sys-apps/systemd-186:0= )
```

`GNOME Control Center`, `GNOME Session`, `GNOME Settings Daemon`, `GNOME Shell` and `Telepathy Mission Control`:

* Reintegrate `UPower` support

`GDM`:

* Reintegrate `ConsoleKit` support

`acpid`:

* Account for power button functionality moving to `GNOME Settings Daemon`

`Guile`:

* Remove incomplete SLOT support (alternatively, fix incomplete SLOT support and provide `eselect guile`)

Implementation (`elogind`)
-----------------------------

99.99% Configure Goo™.
