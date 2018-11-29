
Debian
====================
This directory contains files used to package astrald/astral-qt
for Debian-based Linux systems. If you compile astrald/astral-qt yourself, there are some useful files here.

## astral: URI support ##


astral-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install astral-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your astral-qt binary to `/usr/bin`
and the `../../share/pixmaps/astral128.png` to `/usr/share/pixmaps`

astral-qt.protocol (KDE)

