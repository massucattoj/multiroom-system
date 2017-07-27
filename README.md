------------------------------------------------------------------------------
Author: Jean Massucatto
Date: 07/07/2017
------------------------------------------------------------------------------

BUILD A BASIC MULTIROOM AUDIO SYSTEM WITH RASPBERRY + VOLUMIO + SNAPCAST

Server:

	- Arch Linux (Acredito que sejam as mesmas configuracoes para outras distribuicoes)
	- MPD (audio player)
	- MPC (Or another client to controle mpd, like ncmpcpp)	
	- Snapserver

	1) Download audio player and client
		sudo pacman -S mpc mpc

	2) Download Snapcast and install both, snapclient, snapserver
		wget https://github.com/badaix/snapcast/archive/master.zip
		unzip master.zip
		cd snapcast-master
		make all
		

	2)Config the audio player:
		nano /etc/mpd.conf

		Here you need set things like:
			- playlist_directory "~./your_playlist_directory"
			- music_directory    "~./your_music_directory"
			- db_file			 "~./mpd/database"
			- state_file 		 "~./mpd/state"	
			
			# listening port
			- port				 "6600"
			
			- #input
				input {
				        plugin "curl"
				}

			- #audio_output				
				audio_output {
				        type            "fifo"
				        name            "my pipe"
				        path            "/tmp/snapfifo"
				        format          "44100:16:2"
				#       mixer_type      "software"
				}

				#for alsa users
				audio_output {

				        type "alsa"
				        name "my alsa device"

				}

				#Optional
				volume_normalization "yes"
				auto_update "yes"

	3)Config Snapserver:
		nano /etc/default/snapserver

		START_SNAPSERVER=true
		USER_OPTS="--user snapserver:snapserver"

		SNAPSERVER_OPTS="-d -s pipe:///tmp/snapfifo?name=name_if_you_want"



Client:

	- Raspberry B
	- Volumio
	- Snapclient

	1)Download Volumio in https://volumio.org/ and install on a SD card

	2)Config MPD
		nano /etc/mpd.conf

		Here just put the audio output for example:

			audio_output {

			        type "alsa"
			        name "my alsa device"

			}

	3)Config Snapclient

		Search for the line:
			SNAPCLIENT_OPTS=""

		and add:
			SNAPCLIENT_OPTS="-d -h IP_HOST"


	NOW HAVE FUN, OPEN A MPD TERMINAL AND WRITE MPD PLAY!

	If you have problems with audio output, try disconnect the HDMI cable!

https://wiki.archlinux.org/index.php/Music_Player_Daemon
https://volumio.org/forum/multiroom-audio-output-from-volumio-with-snapcast-t3217.html



