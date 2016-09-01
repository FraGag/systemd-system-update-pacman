# systemd-system-update-pacman

This is a couple of scripts
that enable the use of [pacman][pacman]
to perform system updates
using [systemd][systemd]'s system update mechanism.
This means that in order to use these scripts,
you must be using a Linux-based distribution
using pacman as a package manager
and systemd running as init (PID 1).

## Usage

Typical usage looks like this:

* Synchronize pacman's database: `pacman -Sy`
* Optionally, download the updated packages in advance: `pacman -Suw`
* Schedule a system update: `sudo schedule-system-update`
* Reboot

## Installation

Install the package from the [AUR][our-aur-package].

[our-aur-package]: https://aur.archlinux.org/packages/systemd-system-update-pacman/

## Files

The first script is `schedule-system-update`.
Run this script as root (e.g. with `sudo`)
to schedule a system update for the next reboot.
All this does is create the file `/system-update`.
On your next reboot,
systemd will see this file
and perform the system update
instead of performing a normal system boot.

The second script is `perform-system-update`.
This is referenced by `system-update-pacman.service`,
a systemd service unit that will be started to perform the system update.
You do not run this script manually.
It removes `/system-update`
(otherwise, systemd will keep launching system updates on each boot),
then runs `pacman -Su --no-confirm` to update the system.
Note that this does *not* pass the `-y` flag to sync the database.
You should sync the database and review the updates
before performing a system update.

[pacman]: https://www.archlinux.org/pacman/
[systemd]: https://www.freedesktop.org/wiki/Software/systemd/
