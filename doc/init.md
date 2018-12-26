Sample init scripts and service configuration for opterond
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/opterond.service:    systemd service unit configuration
    contrib/init/opterond.openrc:     OpenRC compatible SysV style init script
    contrib/init/opterond.openrcconf: OpenRC conf.d file
    contrib/init/opterond.conf:       Upstart service configuration file
    contrib/init/opterond.init:       CentOS compatible SysV style init script

Service User
---------------------------------

All three Linux startup configurations assume the existence of a "opteron" user
and group.  They must be created before attempting to use these scripts.
The OS X configuration assumes opterond will be set up for the current user.

Configuration
---------------------------------

At a bare minimum, opterond requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, opterond will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that opterond and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If opterond is run with the "-server" flag (set by default), and no rpcpassword is set,
it will use a special cookie file for authentication. The cookie is generated with random
content when the daemon starts, and deleted when it exits. Read access to this file
controls who can access it through RPC.

By default the cookie is stored in the data directory, but it's location can be overridden
with the option '-rpccookiefile'.

This allows for running opterond without having to do any manual configuration.

`conf`, `pid`, and `wallet` accept relative paths which are interpreted as
relative to the data directory. `wallet` *only* supports relative paths.

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/opteron.conf`.

Paths
---------------------------------

### Linux

All three configurations assume several paths that might need to be adjusted.

Binary:              `/usr/bin/opterond`
Configuration file:  `/etc/opteron/opteron.conf`
Data directory:      `/var/lib/opterond`
PID file:            `/var/run/opterond/opterond.pid` (OpenRC and Upstart) or `/var/lib/opterond/opterond.pid` (systemd)
Lock file:           `/var/lock/subsys/opterond` (CentOS)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the opteron user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
opteron user and group.  Access to opteron-cli and other opterond rpc clients
can then be controlled by group membership.

### Mac OS X

Binary:              `/usr/local/bin/opterond`
Configuration file:  `~/Library/Application Support/opteron/opteron.conf`
Data directory:      `~/Library/Application Support/opteron`
Lock file:           `~/Library/Application Support/opteron/.lock`

Installing Service Configuration
-----------------------------------

### systemd

Installing this .service file consists of just copying it to
/usr/lib/systemd/system directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run `systemctl start opterond` and to enable for system startup run
`systemctl enable opterond`

### OpenRC

Rename opterond.openrc to opterond and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/opterond start` and configure it to run on startup with
`rc-update add opterond`

### Upstart (for Debian/Ubuntu based distributions)

Drop opterond.conf in /etc/init.  Test by running `service opterond start`
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon utility.

### CentOS

Copy opterond.init to /etc/init.d/opterond. Test by running `service opterond start`.

Using this script, you can adjust the path and flags to the opterond program by
setting the RAVEND and FLAGS environment variables in the file
/etc/sysconfig/opterond. You can also use the DAEMONOPTS environment variable here.

### Mac OS X

Copy org.opteron.opterond.plist into ~/Library/LaunchAgents. Load the launch agent by
running `launchctl load ~/Library/LaunchAgents/org.opteron.opterond.plist`.

This Launch Agent will cause opterond to start whenever the user logs in.

NOTE: This approach is intended for those wanting to run opterond as the current user.
You will need to modify org.opteron.opterond.plist if you intend to use it as a
Launch Daemon with a dedicated opteron user.

Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
