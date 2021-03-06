Binding to port 23
==================

X/84 does not require privileged access, and its basic configuration binds to port 6023. Multi-user systems do not typically allow non-root users to bind to port 23. Alternatively, you can always use port forwarding on a NAT firewall.

Linux
-----

using privbind_, run the BBS as user 'bbs', group 'adm'::

  sudo privbind -u bbs -g adm x84

Solaris 10
----------

grant net_privaddr privilege to user 'bbs'::

  usermod -K defaultpriv=basic,net_privaddr bbs

BSD
---

redirection using pf(4)::

  pass in on egress inet from any to any port telnet rdr-to 192.168.1.11 port 6023

Other
-----

Usingirect socat_, listen on 192.168.1.11 and for each connection, fork as 'nobody', and pipe the connection to 127.0.0.1 port 6023::

  sudo socat -d -d -lmlocal2 TCP4-LISTEN:23,bind=192.168.1.11,su=nobody,fork,reuseaddr TCP4:127.0.0.1:6023

This has the disadvantage that x84 is unable to identify the originating IP.

.. _privbind: http://sourceforge.net/projects/privbind/
.. _socat: http://www.dest-unreach.org/socat/
