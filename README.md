# Introduction

Welcome, Creality K1/Max user! This repository is intended for those who wish to experiment with the Multi-Material Unit (MMU) using HappyHare on the Creality K1/Max. 
The K1 is an excellent printer ( If you know how to address its issues 😉)

Since the K1 comes with "Creality OS" and an outdated version of Klipper, running HappyHare with an MMU on it can present several challenges. This guideline outlines the steps I took to successfully run ERCF v2 with HappyHare on my K1.

**Important Notice:** I am not a Creality engineer, just a user. I am not responsible for any issues that may arise with your printer.  
**Proceed at your own risk.**

# How to
1. **Reset the printer to factory default**
   
2. **Update the printer to lastest firmware**  
    Mine is running on 1.3.3.6

3. **Root the machine**  
    Follow this incredible [guildeline](https://guilouz.github.io/Creality-Helper-Script-Wiki/firmwares/install-and-update-rooted-firmware-k1/) to root your K1

4. **Install softwares**  
    Use [Helper script](https://guilouz.github.io/Creality-Helper-Script-Wiki/helper-script/helper-script-installation/) to install
    + Moonraker
    + Mainsail/Fluidd
    + Klipper Gcode shell command
    + Guppy Screen
   
5. **Update your MCUs firmware**  
    Note: Creality K1 MCU bootloader awaits handshake 15 seconds after powerup MCU, handshake need for any operation and only ONCE. After 15s, you need to reset or unplug and replug the MCU to the motherboard and try to send handshake command again.  
    You need to download:  
   + [Creality K1 MCU flasher utility](https://github.com/cryoz/k1_mcu_flasher)
   + [New firwares](https://github.com/pellcorp/klipper/tree/master/fw/K1)  
    All put all of them on */root*
   1. **Main MCU**.  
   THis MCU need to flash first since it does have it own reset button. 
      1. ssh to your printer
      2. Access to your Mainsail -> Firmware Restart -> Stop Klipper
      3. Run this bellow command
        <pre> python3 mcu_util.py -c -i /dev/ttyS7 -g -u -f mcu0_120_G32-mcu0_011_000.bin </pre>  
        ![MCU Flashed](./resource/mcu_flashed.png)