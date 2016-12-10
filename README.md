# Post-installation steps after installing antergos on the ASUS ROG Strix GL702VM-GC100T

## Nvidia GeForce 1060

Install:

```
nvidia nvidia-settings nvidia-libgl (Remove mesa-libgl in the installation progress)
```

## Sound

To get rid of the loud sound pops when shutting down:
Create new file:

`/etc/systemd/system/beep-disable.service`

```
[Unit]
Description=Unloads audio module to prevent beep on shutdown
DefaultDependencies=no

[Service]
Type=oneshot
ExecStart=/bin/sh -c 'rmmod snd_hda_intel'

[Install]
WantedBy=shutdown.target suspend.target
```

and one more:

`/etc/systemd/system/beep-disable-wakeup.service`

```
[Unit]
Description=Load sound module back on system resume
After=suspend.target
Wants=local-system-resume.service
Before=local-system-resume.service

[Service]
Type=oneshot
ExecStart=/bin/sh -c 'modprobe snd_hda_intel'

[Install]
WantedBy=suspend.target
```

Then enable beep-disable.service and beep-disable-wakeup.service as root.
