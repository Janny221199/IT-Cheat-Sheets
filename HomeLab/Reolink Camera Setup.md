1. connect camera to WiFi (and set to 2.4G only)
2. Set static private IP address e.g. `192.168.2.22X` and reboot camera
3. allow http or https for camera to access the admin interface (mobile App: `Settings` -> `More Features` -> `Advanced Network Settings` -> `Server Settings`)
4. disable `UPnP` and `UID` within the Advanced Network Settings (currently only via Web Admin Portal) to ensure that no one outside the home network can access the video material and it is not uploaded to Reolink (web App: `Settings` -> `Network` -> `Advanced`)
   ![](attachments/Screenshot%202026-10-17%20at%2018.28.21.png)
5. set wifi setting to 2.4G only
6. ensure Auto-Update turned on
7. disable watermark
8. enable Audio recording
9. Set Movement / Smart Detection (1. Object Size, All-day Sensitivity, Non-Detection-Zone)
10. create notification (email or push) (and disable any motion from email notification)
11. create new user (e.g. `sanker`) with no admin permissions
12. set Auto-Reboot to Every day (sometimes it only reconnected to wifi when it was restarted)
13. connect to mobile Reolink App via this IP + device password and not with QR-Code or UID (`+` -> `Manual Input` -> `IP`)
   