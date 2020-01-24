---
title: "Autostart using Upstart"
description: "Instructions on how to setup Home Assistant to launch on boot using Upstart."
---

Many linux distributions use the Upstart system (or similar) for managing daemons. Typically, systems based on Debian 7 or previous use Upstart. This includes Ubuntu releases before 15.04. If you are unsure if your system is using Upstart, you may check with the following command:

```bash
$ ps -p 1 -o comm=
```

If the preceding command returns the string `init`, you are likely using Upstart.

Upstart will launch init scripts that are located in the directory `/etc/init.d/`. A sample init script for systems using Upstart could look like the sample below.

```bash
#!/bin/sh
### BEGIN INIT INFO
# Provides:          hass
# Required-Start:    $local_fs $network $named $time $syslog
# Required-Stop:     $local_fs $network $named $time $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Description:       Home\ Assistant
### END INIT INFO

# /etc/init.d Service Script for Home Assistant
# Created with: https://gist.github.com/naholyr/4275302#file-new-service-sh
#
# Installation:
#   1) If any commands need to run before executing hass (like loading a
#      virtual environment), put them in PRE_EXEC. This command must end with
#      a semicolon.
#   2) Set RUN_AS to the username that should be used to execute hass.
#   3) Copy this script to /etc/init.d/
#       sudo cp hass-daemon /etc/init.d/hass-daemon
#       sudo chmod +x /etc/init.d/hass-daemon
#   4) Register the daemon with Linux
#       sudo update-rc.d hass-daemon defaults
#   5) Install this service
#       sudo service hass-daemon install
#   6) Restart Machine
#
# After installation, HA should start automatically. If HA does not start,
# check the log file output for errors.
#       /var/opt/homeassistant/home-assistant.log

PRE_EXEC=""
RUN_AS="USER"
PID_FILE="/var/run/hass.pid"
CONFIG_DIR="/var/opt/homeassistant"
FLAGS="-v --config $CONFIG_DIR --pid-file $PID_FILE --daemon"
REDIRECT="> $CONFIG_DIR/home-assistant.log 2>&1"

start() {
  if [ -f $PID_FILE ] && kill -0 $(cat $PID_FILE) 2> /dev/null; then
    echo 'Service already running' >&2
    return 1
  fi
  echo 'Starting service…' >&2
  local CMD="$PRE_EXEC hass $FLAGS $REDIRECT;"
  su -c "$CMD" $RUN_AS
  echo 'Service started' >&2
}

stop() {
    if [ ! -f "$PID_FILE" ] || ! kill -0 $(cat "$PID_FILE") 2> /dev/null; then
    echo 'Service not running' >&2
    return 1
  fi
  echo 'Stopping service…' >&2
  kill -3 $(cat "$PID_FILE")
  while ps -p $(cat "$PID_FILE") > /dev/null 2>&1; do sleep 1;done;
  echo 'Service stopped' >&2
}

install() {
    echo "Installing Home Assistant Daemon (hass-daemon)"
    echo "999999" > $PID_FILE
    chown $RUN_AS $PID_FILE
    mkdir -p $CONFIG_DIR
    chown $RUN_AS $CONFIG_DIR
}

uninstall() {
  echo -n "Are you really sure you want to uninstall this service? That cannot be undone. [yes|No] "
  local SURE
  read SURE
  if [ "$SURE" = "yes" ]; then
    stop
    rm -fv "$PID_FILE"
    echo "Notice: The config directory has not been removed"
    echo $CONFIG_DIR
    update-rc.d -f hass-daemon remove
    rm -fv "$0"
    echo "Home Assistant Daemon has been removed. Home Assistant is still installed."
  fi
}

case "$1" in
  start)
    start
    ;;
  stop)
    stop
    ;;
  install)
    install
    ;;
  uninstall)
    uninstall
    ;;
  restart)
    stop
    start
    ;;
  *)
    echo "Usage: $0 {start|stop|restart|install|uninstall}"
esac
```

To install this script, download it, tweak it to you liking, and install it by following the directions in the header. This script will setup Home Assistant to run when the system boots. To start/stop Home Assistant manually, issue the following commands:

```bash
sudo service hass-daemon start
sudo service hass-daemon stop
```

When running Home Assistant with this script, the configuration directory will be located at `/var/opt/homeassistant`. This directory will contain a verbose log rather than simply an error log.

When running daemons, it is good practice to have the daemon run under its own user name rather than the default user's name. Instructions for setting this up are outside the scope of this document.
