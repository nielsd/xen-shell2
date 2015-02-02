# xen-shell2
XEN guests / cloud management shell which provides secure, simple and easy (remote, multi-user, multi-level) console based access to XEN DomU management 
xen-shell2 is a fork of xen-shell (initially from Steve Kemps - http://www.xen-tools.org/software/xen-shell/)

xen-shell2 offers compatibility to the current XEN XL tool stack, which ubstitutes the outdated XM stack step by step.

More details will follow here asap.

The Project is licensed under the GPLv2 and a fork of the xen-shell from Steve kemps - many thanks to him for his excellent work!

xen-shell2 is a project from Niels Dettenbach <nd@syndicat.com>.

Pls feel free to contact me in case of questions:

CONTACT
--------

      Niels Dettenbach
      Mail: nd@syndicat.com
      WWW: http://www.syndicat.com

      Current project page: 
      https://github.com/nielsd/xen-shell2

Hope you enjoy it!


PRE-REQUISITES
--------------
- XEN
- xen-tools (xl stack)
- screen
- sudo
- vnstat (only required for bandwidth viewing)


INTALLATION
-----------

      cd /
      tar xvpzf xen-shell2.tar.gz
      cd /etc/xen-shell
      vi xen-shell.conf
      vi _screenrc

      # make xen-shell a valid login shell
      echo "/usr/bin/xen-shell" >> /etc/shells

      # create a group for xen-shell users 
      groupadd xen-shell

      # allow xen-shell group to sudo xl:


(Installation by Makefile follows asap)

UPGRADE
--------
xen-shell2 is widely compatible to xen-shell 1.9 (the last xen-shell version from Steve) - except xm commands like (xm-reimage) are now substituted by xl (i.e. xl-reimage). Domu configurations are compatible, except that the default blacklist of commands is not empty anymore for security reasons. 

However, We recommend to use and adopt our new /etc/xen-shell/xen-shell.conf in the current version.

Just copy over the files xen-shell and xen-login-shell in /usr/bin/


GIVE NON-PRIV USERS A XEN-SHELL
-------------------------------
xen-shell is intended to run as a login shell 

      useradd -m -s /usr/bin/xen-login-shell -g xen-shell -c '1st XEN guest's admin' xenadmin1

Configure DomUs in their config files for individual access:

      vi /etc/xen/auto/<1st_domU.cfg>
      ...

(alternatively you may place the config directives in dedicated files in another path - the need at least the name and vif (for bandwidth usage) parameter from the full config file)

add lines like:

      ## These users may control this domU.
      #
      # give xenadmin1 access  to this DomU (comma seperated if multiple)
      xen_shell = "xenadmin1"

      # block access to these xen-shell2 commands for this domU
      xen_shell_blacklist = "top"

If you want to offer bandwidth usage viewing (by vnstat), you have to define static interface name(s) for your DomU by "vifname" - i.e.:

      vif = [ 'vifname=1st_domU0, ip=10.20.30.40, mac=00:11:22:33:44:55' ]

TEST
----
then try if it works:

      ssh -v xenadmin1@xendom0.host # (remote)

if xen shell opens, call:

      help

for available commands.


MAJOR COMMANDS
--------------
The following commands are available within this shell:

      boot - Boot the Xen guest.
   console - Gain access to a Xen guest via the serial console.
      exit - Exit the shell.
      help - Show general, or command-specific, help information.
    passwd - Change the password used to access this host.
     pause - This will pause the Xen guest.
      quit - Exit this shell.
    reboot - Reboot the Xen guest.
    serial - Gain access to the Xen guest via the serial console.
  shutdown - Shutdown the Xen guest.
    status - Show the status of the Xen guest.
    sysreq - Send a 'sysreq' keystroke to the guest.
   unpause - This will unpause the Xen guest.
    uptime - Show the uptime information of your guest system and this host.
   version - Show the version of this shell, and of Xen.
    whoami - Show the user you're connected to the host system as.

For command-specific help run "help command".


REIMAGE / RESTORE
-----------------
You may place a reimage.sh in a users home directory which helds code that provides some kind of a restore of a users DomU('s).


TODO
----
- Makefile / Install
- adopting manpages
- redesigned (generic) backup / restore interface for DomUs, allowing different backup / restore techniques to adapt
- resource management (CPU POOLS, Memory)
