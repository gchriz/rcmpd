Hardware requirements
=====================

* computer running an operating system hosting `MPD`_, `LIRC`_ and bash - that is usually a kind of UNIX/Linux

* infrared receiver connected to that computer
    The IR detector (e.g. Vishay TSOP31238, less than 1,- €) is connected to my Pi's GPIO

* IR remote control - supported by LIRC_

* optionally a printer


.. _MPD:  https://www.musicpd.org/
.. _LIRC: http://www.lirc.org/



Software requirements
=====================


* mpd  configured and running

* mpc  standard CLI client for sending commands to mpd

* lirc running and configured (more details in the LIRC section below)::

   * /etc/lirc/lirc_options.conf    driver and device configuration.
   * /etc/lirc/lircd.conf           main config file. Just includes *.conf files
   * /etc/lirc/lircd.conf.d/*.conf  defined remote control(s)
                                    (downloaded from http://lirc.sourceforge.net/remotes/
                                    or recorded with irrecord -l)
   * ~/.lircrc                      user's command assignments

* several standard Linux tools (grep, awk, sed, sort, tr, cut, ...)

* optional (for Text To Speech): espeak_

* optional (for printing): Cups (with lp), Ghostscript (with gs and ps2pdf), paps


.. note:: Optional patch for paps

    The original *paps* package (creating Postscript for printing) in version 0.6.8-6
    doesn't have a *--title* option for filling the header on the printed document with own text.

    It shows the filename - or the string "stdin" - instead.

    Since I really wanted to be able to place variable text in the header
    I created a patch for that. It's described at https://www.ziemski.net/paps

    But of course rcmpd is working with the unpatched *paps* as well - just without the individual header.


.. _espeak: http://espeak.sourceforge.net



Installation
============

1. Save *rcmpd* into a directory available via $PATH (e.g. in ~/bin).

#. Make it executable, e.g. with *chmod u+x rcmpd*

#. execute *rcmpd --checkinstall* to do a base installation check

#. `Install-and-configure-LIRC-for-your-RC`_  (see below)

#. Create an appropriate ~/.lircrc for handling the RC commands

   | It needs to match your RC and the commands in *rcmpd*.
   | It's possible to create it manually, but it's easier to use *rcmpd --createlircrc*.

#. (Re)start irexec to let it know about the new lircrc: *rcmpd --restartirexec*

#. Test it in your shell by *rcmpd <command>* with <command> being any of the defined commands above.

|

If all works fine you should make *irexec* autostart.

That may be done via an entry in /etc/rc.local like this::

  su - pi -c "/usr/bin/irexec --daemon /home/pi/.lircrc"

Or if playback should start immediately::

  su - pi -c "/usr/bin/irexec --daemon /home/pi/.lircrc ; mpc play"

You should use a "normal" user (here: pi) for that.

