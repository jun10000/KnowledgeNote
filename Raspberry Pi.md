# Raspberry Pi
[Hexxeh/rpi-firmware]: https://github.com/Hexxeh/rpi-firmware/commits/master

## Pin Layout
Execute the command `pinout` at Raspberry Pi to show the following result.  
Parenthesized numbers are physical pin numbers.

- Raspberry Pi 3 Type B:
    ```
       3V3  (1) (2)  5V    
     GPIO2  (3) (4)  5V    
     GPIO3  (5) (6)  GND   
     GPIO4  (7) (8)  GPIO14
       GND  (9) (10) GPIO15
    GPIO17 (11) (12) GPIO18
    GPIO27 (13) (14) GND   
    GPIO22 (15) (16) GPIO23
       3V3 (17) (18) GPIO24
    GPIO10 (19) (20) GND   
     GPIO9 (21) (22) GPIO25
    GPIO11 (23) (24) GPIO8 
       GND (25) (26) GPIO7 
     GPIO0 (27) (28) GPIO1 
     GPIO5 (29) (30) GND   
     GPIO6 (31) (32) GPIO12
    GPIO13 (33) (34) GND   
    GPIO19 (35) (36) GPIO16
    GPIO26 (37) (38) GPIO20
       GND (39) (40) GPIO21
    ```

## Firmware
If you'd like to see firmware releases, please go to [Hexxeh/rpi-firmware] page.  

### Confirm the current firmware version
Execute `uname -a`.
### Update/Downgrade firmware
Execute following commands.
```
sudo apt update
sudo apt upgrade
sudo apt dist-upgrade
sudo rpi-update <HASH>
sudo reboot
```
Note:  
    If \<HASH\> is ellipsised, the firmware will be the latest version.  
    \<HASH\> is the full SHA string written in [Hexxeh/rpi-firmware] page.
