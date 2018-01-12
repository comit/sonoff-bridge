How to setup and configure "OTA over SCP" upload for PlatformIO. The uploader pushes .bin files to remote OTA server using SCP. Images can be served to Sonoff-Tasmotas from there.

# Configuration
To upload .bin images to OTA server using SCP, one has to edit following lines under target environment:
```
; *** Upload file to OTA server using SCP
upload_port = pi@192.168.2.11:/var/www/html/Updates
extra_scripts = pio/sftp-uploader.py
```
upload_port should be modified to reflect user, host and path on the host where images should be uploaded.

# Requirements
SSH communication between the build server and OTA server should be pre-configured so that it doesn't require password (pre-shared keys).

## Add the pre-shared key
On a linux client machine type the following to generate the key:
`ssh-keygen -t rsa -C "YOUR OWN KEY INFO"`
Press enter three times (without any input).
Copy the key to your ssh server:
`ssh-copy-id -i ~/.ssh/id_rsa.pub USER@IPADRESS`
You need to confirm this action. Use your server ssh password (one last time).
_Optional reload the ssh service:_
`sudo /etc/init.d/ssh restart`

Easy compilation and upload can be performed from the icons at the laft side of the PlatformIO screen or use Ctrl + Alt + U to upload (will build if needed).

More: https://github.com/arendst/Sonoff-Tasmota/pull/934