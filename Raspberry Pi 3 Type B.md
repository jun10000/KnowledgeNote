# Raspberry Pi 3 Type B
[Hexxeh/rpi-firmware]: https://github.com/Hexxeh/rpi-firmware/commits/master
[raspberrypi/firmware/boot/overlays/README]: https://github.com/raspberrypi/firmware/blob/master/boot/overlays/README
[systemd - ArchWiki]: https://wiki.archlinux.jp/index.php/Systemd

## Pin Layout
Execute the command `pinout` to show the following result.
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
Parenthesized numbers are physical pin numbers.

## Firmware
- To see firmware releases, please go to [Hexxeh/rpi-firmware] page.  
- To confirm the current firmware version, execute `uname -a`.
- To update/downgrade firmware, execute following commands.
    ```
    sudo apt update
    sudo apt upgrade
    sudo apt dist-upgrade
    sudo rpi-update <HASH>
    sudo reboot
    ```
    If \<HASH\> is ellipsised, the firmware will be the latest version.  
    \<HASH\> is the full SHA string written in [Hexxeh/rpi-firmware] page.

## Device
### Backlight
Directory: `/sys/class/backlight/rpi_backlight/`
- Max brightness value is written in `max_brightness`.
- Current brightness value is written in `brightness`.  
  The value must be **0 <= value <= MaxBrightness**.  
  The changed value will be saved.
### Onboard LEDs
Directory (Green LED, Activity): `/sys/class/leds/led0/`  
Directory (Red LED, Power): `/sys/class/leds/led1/`  
- Max brightness value is written in `max_brightness`.
- Current brightness value is written in `brightness`.  
  The value must be **0** or **MaxBrightness**.  
  The changed value **won't** be saved.
- Current trigger value is parenthesized in `trigger`.  
  The value must be included in `trigger`.  
  The changed value **won't** be saved.
- To save changed values permanently, edit `/boot/config.txt`.  
  Please refer to [raspberrypi/firmware/boot/overlays/README] page.

## Daemon
### Unit File
- Directory
    - `/usr/lib/systemd/system/`
    - `/etc/systemd/system/` (For Administrator)
- Naming Rule: `<DaemonName>.service`
- Contents  
    Here is the minimal template.
    ```
    [Unit]
    Description=<DaemonName>
    
    [Service]
    ExecStart=<ProgramPath>
    
    [Install]
    #WantedBy=multi-user.target
    ```
    For details, see [systemd - ArchWiki] page.
### Systemctl
- Apply daemon information changes: `sudo systemctl daemon-reload`
- Start the daemon: `sudo systemctl start <DaemonName>`
- Stop the daemon: `sudo systemctl stop <DaemonName>`
- Restart the daemon: `sudo systemctl restart <DaemonName>`
- Enable the daemon auto starting: `sudo systemctl enable <DaemonName>`
- Disable the daemon auto starting: `sudo systemctl disable <DaemonName>`

## Swap
If you want to change swap settings permanently, `insserv` package is required.
- Enable swap temporary: `sudo dphys-swapfile swapon`
- Disable swap temporary: `sudo dphys-swapfile swapoff`
- Set to create swap on system starting: `insserv -dv dphys-swapfile`
- Set to create no swap on system starting: `insserv -r dphys-swapfile`

## Performance Survey Commands
- `top`  
  Shows CPU usage, memory usage, process list etc.  
  It can sort by CPU usage etc.  
  ```
  top - 14:47:33 up 7 days, 23:32,  2 users,  load average: 0.99, 0.67, 0.68
  Tasks: 135 total,   1 running,  93 sleeping,   0 stopped,   1 zombie
  %Cpu(s): 19.1 us,  1.6 sy,  0.0 ni, 79.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
  KiB Mem :   949452 total,    71844 free,   436932 used,   440676 buff/cache
  KiB Swap:        0 total,        0 free,        0 used.   495960 avail Mem 
  
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND    
   1286 pi        20   0  490336 165152  61044 S   2.6 17.4 307:50.77 chromium-b+
    849 pi        20   0  683140 162900  76652 S   1.3 17.2 153:46.58 chromium-b+
  12891 pi        20   0    7584   3292   2792 R   0.7  0.3   0:00.11 top        
    447 root      20   0  145140  24848  12988 S   0.3  2.6  26:36.73 Xorg       
  12286 www-data  20   0  139536   7320   2888 S   0.3  0.8   0:20.43 apache2    
      1 root      20   0   28136   5692   4480 S   0.0  0.6   0:14.40 systemd    
      2 root      20   0       0      0      0 S   0.0  0.0   0:00.46 kthreadd   
      4 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 kworker/0:+
      6 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 mm_percpu_+
      7 root      20   0       0      0      0 S   0.0  0.0   0:12.42 ksoftirqd/0
  ...
  ```
- ps  
  Shows process list.
    - `ps`  
      Shows only current user's processes.  
      ```
        PID TTY          TIME CMD
      12872 pts/0    00:00:00 bash
      13967 pts/0    00:00:00 ps
      ```
    - `ps ax`  
      Shows all user's processes.  
      ```
        PID TTY      STAT   TIME COMMAND
          1 ?        Ss     0:14 /sbin/init splash
          2 ?        S      0:00 [kthreadd]
          4 ?        I<     0:00 [kworker/0:0H]
          6 ?        I<     0:00 [mm_percpu_wq]
          7 ?        S      0:12 [ksoftirqd/0]
          8 ?        I     14:19 [rcu_sched]
          9 ?        I      0:00 [rcu_bh]
         10 ?        S      0:00 [migration/0]
         11 ?        S      0:00 [cpuhp/0]
         12 ?        S      0:00 [cpuhp/1]
      ...
      ```
    - `ps axu`  
      Shows all user's processes and more.  
      ```
      USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
      root         1  0.0  0.5  28136  5692 ?        Ss    5月24   0:14 /sbin/init sp
      root         2  0.0  0.0      0     0 ?        S     5月24   0:00 [kthreadd]
      root         4  0.0  0.0      0     0 ?        I<    5月24   0:00 [kworker/0:0H
      root         6  0.0  0.0      0     0 ?        I<    5月24   0:00 [mm_percpu_wq
      root         7  0.0  0.0      0     0 ?        S     5月24   0:12 [ksoftirqd/0]
      root         8  0.1  0.0      0     0 ?        I     5月24  14:19 [rcu_sched]
      root         9  0.0  0.0      0     0 ?        I     5月24   0:00 [rcu_bh]
      root        10  0.0  0.0      0     0 ?        S     5月24   0:00 [migration/0]
      root        11  0.0  0.0      0     0 ?        S     5月24   0:00 [cpuhp/0]
      root        12  0.0  0.0      0     0 ?        S     5月24   0:00 [cpuhp/1]
      ...
      ```
