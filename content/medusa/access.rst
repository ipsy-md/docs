---
title: Accessing
weight: 230
---

Command Line
************
The easiest and most reliable way of connecting to Medusa is via
`SSH </docs/tools/ssh/>`_.

Connecting is as simple as running the following in your terminal:

.. code::

  ssh username@medusa.ovgu.de

.. class:: note

  **NOTE:** Users with unstable internet connections will likely find
  `tmux </docs/tools/tmux/>`_ to be a helpful tool.

VPN
*********
You need a `VPN`_ connection to go on the medusa server.

.. _VPN: https://www.urz.ovgu.de/urz/en/vpn-path-204,616.html

Graphical
*********
There are multiple methods of connecting to Medusa graphically.

**X Forwarding**
  Graphical programs run on the remote server can be displayed locally using
  `SSH's X Forwarding </docs/tools/ssh/#x%20forwarding>`_. Do note that X Forwarding
  is very sensitive to latency, so it is only practical to use when on the OvGU
  campus.

  .. code::

    ssh -X username@medusa.ovgu.de

**Sixel**
  `Sixel </docs/tools/sixel/>`_ is mostly a toy right now. But it is quite convenient
  in the few situations where there is support (image and NIfTI previews).

**VNC**
  VNC is for users who prefer a more familiar desktop experience or need to use
  graphical programs while off-campus. VNC is a multi-step process and not as
  easy as straight SSH.

  First, SSH into Medusa (explained above). Then setup your VNC password. You
  only need to do this once. The password is stored **unencrypted** in a text
  file, so do not use a valuable password (such as for Medusa, email, etc).

  .. code::

    me@medusa:~$ vncpasswd
    Password:
    Verify:

  Next, start the VNC server.

  .. code::

    me@medusa:~$ vncserver

    New 'medusa:9 (me)' desktop is medusa:9

    Starting applications specified in /home/me/.vnc/xstartup
    Log file is /home/me/.vnc/medusa:9.log

  Note the number ``9`` in ``medusa:9``. Yours will likely be different. Take
  note of your number, as it references *your* VNC session and will be used
  later.

  The VNC server runs until it is terminated by you (or a reboot of Medusa). If
  you close your client/disconnect, it will continue to run. If you "log out" in
  the session, it will terminate the server.

  **Linux**
    Open a terminal and run the following:

    .. code::

      vncviewer -via medusa.ovgu.de :9

    Note the ``:9``. Make sure that this is *your* number from above.  This
    command will first ask that you authenticate to Medusa, and then it will ask
    for the VNC password you set before.

  **macOS**
    macOS has a VNC client built-in, but it isn't SSH aware. So a SSH tunnel
    needs to be setup first.

    .. code::

      ssh -f -L 5909:127.0.0.1:5909 username@medusa.ovgu.de sleep 60

    Then you can use VNC.

    .. code::

      open vnc://127.0.0.1:5909

    Note the number "5909" in both commands. It should be 5900 + the ``number``
    noted above. Make sure that it is *your* number.

    .. class:: todo

      **TODO**: write a simple shell script that can be ``curl``-ed into the user's path.

  **Windows**
    Be sure to download the "VNC Viewer".

    Launch the viewer and enter the following information:

  **Putty Configuration on Windows**

+------------------------+------------------------------------------------------------------------------------+
|   Points               | Steps                                                                              |
+========================+====================================================================================+
| 1                      | Specify host name as your medusa login (name@medusa.ovgu.de) and Port 22           |
+------------------------+------------------------------------------------------------------------------------+
| 2                      | On the right hand side, click on the + next to SSH and then click Tunnels.         |
+------------------------+------------------------------------------------------------------------------------+
| 3                      | Type 59 + your session number for Source port                                      |
|                        | (e.g. if your session number is 11 type 5911).                                     |
+------------------------+------------------------------------------------------------------------------------+
| 4                      | Type localhost:59 (session number, here 11) as Destination then click Add.         |
|                        | (e.g. localhost:5911)                                                              |
+------------------------+------------------------------------------------------------------------------------+
| 5                      | Go back to session and you can save this session so you don’t have to repeat       |
|                        | these steps each time you want to connect.                                         |
+------------------------+------------------------------------------------------------------------------------+
| 6                      | Load and open session and connect to medusa and start the VNC Server               |
|                        | (after setting up your password)                                                   |
+------------------------+------------------------------------------------------------------------------------+
| 7                      | Open a VNC Viewer, type localhost:59(session number) and connect.                  |
+------------------------+------------------------------------------------------------------------------------+
| 8                      | You will be required to enter your VNC password, afterwards you will               |
|                        | have access to the GUI.                                                            |
+------------------------+------------------------------------------------------------------------------------+

  Note the port number: "5911". It should be 5900 + the ``number`` we
  noted above. Make sure that it is *your* number.

|pic1| first |pic2|

.. |pic1| image:: /docs/medusa/images/finalone.png
   :width: 45%

.. |pic2| image:: /docs/medusa/images/finaltwo.png
   :width: 45%

|pic3| second |pic4|

.. |pic3| image:: /docs/medusa/images/vnc_connection.png
   :width: 45%

.. |pic4| image:: /docs/medusa/images/youmadeit.png
   :width: 45%


**Helpful Commands**

How to show the List of started VNC Server.

.. code::

    vncserver -list

How to kill the process from to many started vnc server (11 is the sessionnumber).

.. code::

    vncserver -kill :11
