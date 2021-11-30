Features
========

Standard functions
------------------

Of course there are the usual, obvious commands:

* Power (shutdown)
* Mute
* Volume+/-
* toggle repeat and random
* Pause
* Play/Stop
* Next/Previous track
* 1st .. 10th track

And a not so obvious:

* do something with the current songs title/artist

  | Currently it gets saved in a file (appended, to be precise).
  | That is intended to remember a nice song for later examination,
  | especially useful when listening to radio streams.
  |
  | Of course there are more actions thinkable, e.g. mail it somewhere ...

But there is more! Please read on.


Speech output
-------------

| Inspired by a maker's discussion about a "`very simple Raspberry Pi Radio`_"
| I implemented Text-to-speech outputs  to get a more comfortable user experience.

There are some informations spoken and it's possible to get some commands "echoed" in words.

| I'm using espeak (http://espeak.sourceforge.net) for text-to-speech, but it would be possible to use other software too.
| Google's API has much better sound quality, but I wanted to have it working offline.
| Both ways are now included and can be switched by setting variable *tts_type*.

* speak info: current time
* speak info: collection (available playlists)
* speak info: volume and state of "repeat"
* speak info: playlist
* speak info: channel/album
* speak info: current track/song
* toggle audible commands on/off

.. _Very simple Raspberry Pi Radio: https://mightyohm.com/forum/viewtopic.php?f=2&t=3420#p5621


Simple Playlist features
------------------------

Additionally there are the following playlist features:

* next/previous playlist
* load playlist "A", "B", "C"
* skip through all available playlists with the buttons


Compound keys
-------------

All of the above functions are reachable with a single button press.

To extend the possibilities I implemented compound keys.

1. They are started and stopped with the *OK* button.
2. Between those two OK's there are the letters A, B, C and D, the digits 0-9 and some more keys allowed.
3. Every key needs to follow the one before within 5 seconds.


Enhanced Playlist features
--------------------------

* If a playlist has more than ten entries (reachable via buttons 1-9,0) it's possible to select higher ones with compound keys.

    E.g. entry 27 could be started with::

      OK 2 7 OK

* You can load more playlists directly by combining the letters A, B, C, D with the digits to playlists like "A7", "AC1" or "B126".

* The list of defined/available playlists and the contents of the current one could be printed on a printer with::

    OK MENU EPG OK

  More about printing see below.


Playlist preparation
....................

To make the direct loading of playlists work, the playlists in question have to be prepared a bit:

Using a MPD client on my PC I'm predefining MPD playlists which I would like to manage with the RC with a special naming convention.

Example::

  A ~ Various Artists - The ultimate songs
  B
  B1 -- Angel Hearts - The first album
  C24 ~ Alltime favourites
  Runaways - Hot!
  The lonely boys - Chicago

All the playlists starting with

* a name consisting only of letters A,B,C,D and optional digits
* or a prefix of that form, followed by a devider ('~' (a tilde)  or '--' (two dashes))

can now be loaded with the RC! (The first four in the example above.)


So the above example playlists can be loaded by::

  A

  B

  OK B 1 OK

  OK C 2 4 OK

| The two example ones without the special prefix/name can't be loaded directly that way.
| But of course they can be reached by skipping to them via *Channel+* / *Channel-*.


Power button and sleep timer
----------------------------

| The *Power* button initiates a "shutown -h now" (that is a power off (on a Raspberry Pi only sort of...)).
| Pressing it again, any delayed shutdown gets cancelled.

Using a compound key with *Power* and then one or more digits, a sleep timer with the given time in minutes is started.

For example set a sleep timer of 30 minutes::

  OK POWER 3 0 OK

| That means: The system will shut down then.
| Anyway: This delayed shutdown can be cancelled with *Power* again as already mentioned.


Volume control
--------------

Additionally to the Vol+/- in steps of 5% the volume can be set to 100% by::

  OK VOL+ VOL+ OK

to 50% by either of::

  OK VOL+ VOL- OK
  OK VOL- VOL+ OK

and to 0% by::

  OK VOL- VOL- OK


Calling external commands
-------------------------

If there are external programs/scripts existing named like rcmpd.A  (B,C,D,0-9) they can be called by::

  OK MENU A OK


Printing a short summary of keys
--------------------------------

A short reference of the possible keys can be printed on a printer with::

  OK MENU MENU OK

This is the same list that is shown if rcmpd is called without any parameters.

To increase the WAF (Woman Acceptance Factor) I added those infos in German as well now.

| To switch to German help you might edit a config value in the script,
| but it's better to control it from outside:

Add a line like the following to your ~/.bashrc file::

  export RCMPD_LANGUAGE=DE

To activate it just logoff and login again or type the above in your current shell session.


More about printing
-------------------

The optional printing features are implemented by producing Postscript output which is converted to PDF and sent to the printer.

The most current output file is stored in */tmp/rcmpd-printout.pdf* (configured as variable PRINTOUT in the script).

So it's possible to use the file for other purposes as well.


Here are two example prints, the short manual and playlist infos.

.. image:: https://ziemski.net/rcmpd/rcmpd-print-1s.jpg
   :height: 80
   :alt:    example printout: short manual
   :target: https://ziemski.net/rcmpd/rcmpd-print-1.jpg

.. image:: https://ziemski.net/rcmpd/rcmpd-print-2s.jpg
   :height: 80
   :alt:    example printout: playlist infos
   :target: https://ziemski.net/rcmpd/rcmpd-print-2.jpg

(Click to see larger picture.)

