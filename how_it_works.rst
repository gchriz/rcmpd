How it works - technically
==========================

Here I would like to describe how the parts are playing together.
Additional infos are shown in the installation section.

1.  First there is the software LIRC_. It

    * knows about the codes the remote control sends

    * and listens to the IR receiver all the time when started.

    In LIRC's configuration file(s) *lircd.conf* and/or preferred *lircd.conf.d/\*.conf* there are the used remote control(s) defined.

    Shortened example::

        begin remote

          name  Pinnacle_RC1144201
          ...

              begin codes
                  power                    0x6E2D
                  menu                     0x6EE7
                  mute                     0x6E1E
                  stop                     0x6E2B
                  play                     0x6ED1
                  pause                    0x6E2C
                  ...

    You can see the name of the RC and some of the known buttons.



2.  LIRC provides - beneath others - a simple program *irexec*.

    *irexec* is able to communicate with the above running lircd and gets infos about a pressed button.

    To know what to do with that button info it has a configuration file too: *.lircrc*.

    That configuration file maps button-presses to actions, for example starting a program.

    In my case I map all buttons to *rcmpd* - with an appropriate parameter to distinguish them.

    Shortened example::

        begin
            prog   = irexec
            remote = Pinnacle_RC1144201
            button = power
            config = /home/pi/bin/rcmpd power
            repeat = 0
        end

        begin
            prog   = irexec
            remote = Pinnacle_RC1144201
            button = pause
            config = /home/pi/bin/rcmpd pause
            repeat = 0
        end

    You can see the matching name of the RC(!) and the matching buttons as well.

    So on every button press *rcmpd* gets called with the appropriate function as parameter.

    That file can be crafted manually or automaticall, described a bit later.

    To get this working *irexec* needs to run in background and wait for button infos.


3.  Finally within *rcmpd* there is a table or block for every used remote control.

    **This is your individual setup - part 1**

    Example::

		#*********************************************************************
		# You need to define the choosen keys of your remote controls here!
		#
		# Used for the creation of .lircrc (and as reference for humans...)
		#
		# Need to match the remote's names and the button names from the files
		# /etc/lirc/lircd.conf and/or /etc/lirc/lircd.conf.d/*.conf!
		# Edit the current ones below, add or replace with your own RC(s).
		#*********************************************************************

		define_remotes()
		{
		  REMOTES=(

		  [Pinnacle_RC1144201]="
			mute power
			menu tv epg
			a b c d
			vol+ vol-
			pinnacle
			ch+ ch-
			ok
			back loop
			fullscreen
			pause play stop
			ffwd rew
			skip+ skip-
			rec
			1 2 3 4 5 6 7 8 9 0
			cancel
		  "

		  # The definition below uses an "aliasing" feature:
		  # If an RC sends a button X but you want rcmpd to behave like button Y
		  # then write X=Y

		  [DENON_RC_126]="
			KEY_POWER
			KEY_VOLUMEDOWN
			KEY_VOLUMEUP
			KEY_CHANNELUP=KEY_NEXT
			KEY_CHANNELDOWN=KEY_PREVIOUS
			KEY_SEARCH=KEY_SCREEN
			KEY_MUTE=KEY_PAUSE
			KEY_MODE=KEY_STOP
		  "

		  )
		}

    Later on in *rcmpd* there is function *button_menu()*

    **That is your individual setup - part 2 - if really needed**

    There are the mappings from "button" to internal function where the commands are executed then::

      #-------------------------------------------------------------
      # The assignments of RC buttons to commands in this program.
      # Here you can do your "setup".
      #-------------------------------------------------------------

      MUTE|KEY_MUTE)            cmd_mute ;;
      VOL+|KEY_VOLUMEUP)        cmd_volume_up ;;
      VOL-|KEY_VOLUMEDOWN)      cmd_volume_down ;;

      PLAY|KEY_PLAY)            cmd_play ;;
      STOP|KEY_STOP)            cmd_stop ;;
      PAUSE|KEY_PAUSE)          cmd_pause ;;
      LOOP|KEY_MEDIA_REPEAT)    cmd_toggle_repeat ;;

      SKIP+|KEY_NEXT)           cmd_next_track ;;
      SKIP-|KEY_PREVIOUS)       cmd_previous_track ;;
      0|1|2|3|4|5|6|7|8|9)      cmd_digit $btn ;;
      KEY_0|KEY_1|KEY_2|KEY_3|KEY_4|KEY_5|KEY_6|KEY_7|KEY_8|KEY_9)  cmd_digit $btn ;;
      CH+|KEY_CHANNELUP)        cmd_next_playlist ;;
      CH-|KEY_CHANNELDOWN)      cmd_previous_playlist ;;
      A|B|C|KEY_A|KEY_B|KEY_C)  cmd_ABC $btn ;;

      REC|RECORD|KEY_RECORD)    cmd_record ;;

      MENU|KEY_MENU)            cmd_toggle_speech ;;

      EPG|KEY_EPG)              cmd_info_collection ;;  # of playlists
      D|KEY_D)                  cmd_info_playlist ;;    # current playlist
      BACK|KEY_BACK)            cmd_info_station ;;     # respective "album"
      FULLSCREEN|KEY_SCREEN)    cmd_info_track ;;
      PINNACLE)                 cmd_info_state ;;
      TV|KEY_TV)                cmd_info_time ;;

      POWER|KEY_POWER)          cmd_poweroff ;;
      #-------------------------------------------------------------
