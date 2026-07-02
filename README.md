# Direwolf
## Instructions to configure and run direwolf
- This information came from: https://www.youtube.com/watch?v=_ynCsnNOELo&t=201s
- Also read this file: https://app.simplenote.com/publish/Zp8Fvl
- Then we want to start direwolf at startup: https://www.youtube.com/watch?v=xt8ik_tTVhQ
- ## Items to place into the direwolf.conf file
  - ADEVICE  plughw:3,0
  -  CHANNEL 0
  -  MYCALL KE7UIA-3
  -  MODEM 1200
  -  GPSD
  -  PTT CM108
  -  AGWPORT 8000
  -  KISSPORT 8001
  -  TBEACON DELAY=0:30 EVERY=1:00 VIA=WIDE1-1 SYMBOL=car
- ## Then we have to make a startup.sh script and make it executable
  -#!/bin/bash
        sleep 20
        #check if GPS is plugged in
        gpspipe -r -n 5

        #check if the soundcard is plugged in
        ck=$(arecord -l | grep "USB Audio Device")
        echo $ck
        while [ -z "${ck}" ]; do
                ck=$(arecord -l | grep "USB Audio Device")
                echo '.'
                sleep 1
        done

        #start direwolf
        tmux new -d -s direwolf '/usr/local/bin/direwolf'
        echo "direwolf has started"
        echo "use this command tmux a -t direwolf to view the program"
        echo "use this command to exit direwolf ctrl b d"
        echo "use this command to stop direwolf ctrl c"
- ## Then we have to put this into a cron job
    - crontab -e
    - @reboot /home/pi/startup.sh
