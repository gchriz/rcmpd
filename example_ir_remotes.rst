Example function assignment on real remote controls
===================================================

Here the assignments I configured for my IR Remote Controls as simple description:

* First an RC with many buttons and a complete function assignment
* Second a much simpler RC with only basic functions

Both can be used in parallel of course.

Pinnacle RC1144201
------------------

.. image:: http://lirc.sourceforge.net/remotes/pinnacle_systems/RC1144201_00.jpg
   :height: 300
   :alt:    Pinnacle_RC1144201
   :target: http://lirc.sourceforge.net/remotes/pinnacle_systems/RC1144201_00.jpg

::

  Power        shutdown / cancel delayed shutdown / set sleep timer
  Mute         mute / unmute
  Menu         toggle audible command descriptions on/off / call external scripts / print keys
  TV           speak info: current time
  EPG          speak info: collection (available playlists) / print playlists
  Vol+ Vol-    increase/decrease volume (in 5% steps)
  Ch+ Ch-      next/previous playlist
  Pinnacle     speak info: volume and state of "repeat"
  A  B  C      load playlist "A", "B", "C"
  D            speak info: playlist
  Back         speak info: channel/album
  Loop         toggle "repeat"
  Fullscreen   speak info: current track/song
  Pause        toggle pause/play
  Play  Stop   start, stop playback / stop currently spoken text
  Rew  FFwd    seek 30 seconds back/forward
  Skip+ Skip-  next/previous track (within playlist)
  Record       save current song info (title/artist) in '/home/pi/rcmpd.record'
  1-9, 0       1st .. 10th track (within playlist)

  OK           start/stop multiple-key commands (build from A,B,C,D,0-9, POWER, MENU, EPG, VOL)


Denon RC-126
------------

.. image:: https://ziemski.net/rcmpd/Denon_RC-126s.jpg
   :height: 150
   :target: https://ziemski.net/rcmpd/Denon_RC-126.jpg


with only these commands::

    KEY_POWER                      shutdown in 1 minute / cancel shutdown / set sleep timer
    KEY_VOLUMEUP KEY_VOLUMEDOWN    increase/decrease volume (in 5% steps)
    KEY_CHANNELUP=KEY_NEXT         next/previous track (within playlist)
    KEY_CHANNELDOWN=KEY_PREVIOUS
    KEY_SEARCH=KEY_SCREEN          speak info: current track/song
    KEY_MUTE=KEY_PAUSE             toggle pause/play
    KEY_MODE=KEY_STOP              stop


