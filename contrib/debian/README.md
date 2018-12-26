
Debian
====================
This directory contains files used to package opterond/opteron-qt
for Debian-based Linux systems. If you compile opterond/opteron-qt yourself, there are some useful files here.

## opteron: URI support ##


opteron-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install opteron-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your opteron-qt binary to `/usr/bin`
and the `../../share/pixmaps/opteron128.png` to `/usr/share/pixmaps`

opteron-qt.protocol (KDE)

