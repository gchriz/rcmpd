Usage
=====

*rcmpd* is easy to call.

The first iexample is the normal operation format and the others are for installation/configuration purposes::

  rcmpd <command> ...     manages a command, e.g. from an infrared RC buttonpress

  rcmpd --checkinstall    checks some aspects of the install (prerequisites)
  rcmpd --createlircrc    creates /home/pi/.lircrc if not existing
  rcmpd --restartirexec   restarts irexec after changes on .lircrc
  rcmpd --showsonginfos   shows the "recorded" infos in '/home/pi/rcmpd.record'
  rcmpd --listplaylists   lists all playlists and the contents of current one

  rcmpd                   without any parameters this usage info and a short summary of keys is shown
