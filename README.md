Bunnyscript Support for P4wnP1
==============================

Author : hahnstep (https://github.com/hahnstep)

Credits :

P4wnP1 is made by Mame82 : https://github.com/mame82

Darren Kitchen (hak5darren) for Bunnyscript and and the lovely BashBunny

https://github.com/hak5/bashbunny-payloads/blob/master/docs/readme.txt

## 

There are 4 shell functions ATTACKMODE, LED, QUACK and Q.

Its not possible to run payloads from bashbunny, the two platforms are to diffrend. <br> 
This is for easy payload generation with a nice syntax, nothing more. 


### installation 

	cd /home/pi/P4wnP1
	git clone https://github.com/hahnstep/bunnyscript

add to payload header

	source $wdir/bunnyscript/bunnyscript

## ATTACKMODE

attackmode can only used once in a payload and acts as configuration switch so 

	ATTACKMODE RNDIS_ETHERNET ECM_ETHERNET HID VID_0x1D6B PID_0x0237

equals to 

	USB_VID="0x1D6B" 
	USB_PID="0x0237"
	USE_ECM=true
	USE_RNDIS=true
	USE_HID=true

attackmode supports follow options


| option | payload / setup.cfg |
| -- | -- | 
| VID_0x1D6B | USB_VID="0x1D6B" | 
| PID_0x0237 | USB_PID="0x0237" | 
| ECM_ETHERNET | USE_ECM=true | 
| RNDIS_ETHERNET | USE_RNDIS=true |
| STORAGE | USE_UMS=true |
| HID | USE_HID=true |
| RAWHID | USE_RAWHID=true |
| MOUSE | USE_HID_MOUSE=true |
| WIFI_CLIENT | WIFI_CLIENT=true |
| WIFI_ACCESSPOINT | WIFI_ACCESSPOINT=true |

## LED

LED 

| STATES | led_blink | rgb |
| -- | -- | -- |
| SETUP | led_blink 1 | Magenta solid |
| FAIL | led_blink 2 | Red slow blink |
| FAIL1 | led_blink 2 | Red slow blink |
| FAIL2 | led_blink 2 | Red fast blink |
| FAIL3 | led_blink 2 | Red very fast blink |
| ATTACK | led_blink 3 | Yellow single blink |
| STAGE1 | led_blink 3 | Yellow single blink |
| STAGE2 | led_blink 3 | Yellow double blink |
| STAGE3 | led_blink 3 | Yellow triple blink |
| STAGE4 | led_blink 3 | Yellow quadruple blink |
| STAGE5 | led_blink 3 | Yellow quintuple blink |
| SPECIAL | led_blink 4 | Cyan inverted single blink |
| SPECIAL1 | led_blink 4 | Cyan inverted single blink |
| SPECIAL2 | led_blink 4 | Cyan inverted double blink |
| SPECIAL3 | led_blink 4 | Cyan inverted triple blink |
| SPECIAL4 | led_blink 4 | Cyan inverted quadriple blink |
| SPECIAL5 | led_blink 4 | Cyan inverted quintuple blink |
| CLEANUP | led_blink 5 | White fast blink |
| FINISH | led_blink 6 | Green 1000ms VERYFAST blink followed by SOLID |
| OFF | led_blink 0 | Off |

## QUACK and Q

quack and q supports ducky script, but no files at the moment 

## Example payload

	#!/bin/bash
	source $wdir/bunnyscript/bunnyscript

	LED SETUP

	ROUTE_SPOOF=false
	HID_KEYBOARD_TEST=true

	lang="us" # todo DUCKY_LANG

	ATTACKMODE RNDIS_ETHERNET ECM_ETHERNET HID VID_0x1D6B PID_0x0237

	LED ATTACK

	function onKeyboardUp()
	{
	        LED SPECIAL

	        Q DELAY 2000
	        Q GUI r
	        Q DELAY 1000
	        Q STRING notepad.exe
	        Q DELAY 100
	        Q ENTER
	        Q DELAY 3000
	        Q STRING Running payload on a P4wnP1
	        Q ENTER

	        LED OFF
	}

	function onBootFinished()
	{
	        LED STAGE1
	}

	function onNetworkUp()
	{
	        LED STAGE2
	}

	function onTargetGotIP()
	{
	        LED STAGE3
	}

	function onLogin()
	{
	        LED STAGE5
	}


