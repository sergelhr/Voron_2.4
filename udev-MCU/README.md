Setting a UDEV rule to show a device in /dev/<myname> instead of using the /by-id or /by-path, 
even works with broken udev version.

First: Find your unique serial number
``udevadm info /dev/<device>`` where <device> would be ttyACM0 or ttyACM1, or /dev/serial/by-id/<device>

Look for the tag labeled like this
``ID_SERIAL=Klipper_stm32f446xx_14001A00095053424E363420``


Second: Edit /etc/udev/rules.d/local.rules, may need to create the file.

``SUBSYSTEM=="tty",ENV{ID_SERIAL}=="Klipper_stm32f446xx_090038001650335331383520",SYMLINK+="octopus",MODE="0666",GROUP="dialout"
SUBSYSTEM=="tty",ENV{ID_SERIAL}=="Klipper_rp2040_E6605481DB58BB36",SYMLINK+="pico",MODE="0666",GROUP="dialout"
SUBSYSTEM=="tty",ENV{ID_SERIAL}=="Klipper_stm32f072xb_480033001853565639333820",SYMLINK+="voron-skirt",MODE="0666",GROUP="dialout"
``

In my setup, the first line creates /dev/octopus, the second line creates ``/dev/pico``, and the third creates ``/dev/voron-skirt``.

save/exit 

reboot

-or-

# reload rules for udev w/out a reboot
#
``sudo udevadm control --reload-rules && sudo udevadm trigger``
