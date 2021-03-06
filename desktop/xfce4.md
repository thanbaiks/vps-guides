Install  packages
----

* XFCE4 Desktop

```shell
sudo apt-get install xfce4 xfce4-goodies autocutsel tightvncserver
```

Modify ~/.vnc/xstartup
-------

```shell
#!/bin/sh

xsetroot -solid black
autocutsel -fork &
startxfce4 &
```

Make file /etc/init.d/vncserver
--------

Remember to change variables

```Shell
#!/bin/sh -e
### BEGIN INIT INFO
# Provides:          vncserver
# Required-Start:    networking
# Default-Start:     3 4 5
# Default-Stop:      0 6
### END INIT INFO

PATH="$PATH:/usr/X11R6/bin/"

# The Username:Group that will run VNC
export USER="root"
#${RUNAS}

# The display that VNC will use
DISPLAY="95"

# Color depth (between 8 and 32)
DEPTH="16"

# The Desktop geometry to use.
#GEOMETRY="<WIDTH>x<HEIGHT>"
#GEOMETRY="800x600"
GEOMETRY="1366x768"
#GEOMETRY="1280x1024"

# The name that the VNC Desktop will have.
NAME="my-vnc-server"

OPTIONS="-name ${NAME} -depth ${DEPTH} -geometry ${GEOMETRY} :${DISPLAY}"

. /lib/lsb/init-functions

case "$1" in
start)
#log_action_begin_msg "Starting vncserver for user '${USER}' on   localhost:${DISPLAY}"
su ${USER} -c "/usr/bin/vncserver ${OPTIONS}"
;;

stop)
#log_action_begin_msg "Stoping vncserver for user '${USER}' on localhost:${DISPLAY}"
su ${USER} -c "/usr/bin/vncserver -kill :${DISPLAY}"
;;

restart)
$0 stop
$0 start
;;
esac

exit 0
```

File need chmod **executable** first :)

```
chmod +x /etc/init.d/vncserver
```


Make service start on boot
-------

```shell
sudo update-rc.d vncserver defaults
```
