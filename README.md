# Introduction

Welcome, Creality K1/Max user! This repository is intended for those who wish to experiment with the Multi-Material Unit (MMU) using HappyHare on the Creality K1/Max. 
The K1 is an excellent printer ( If you know how to address its issues 😉)

Since the K1 comes with "Creality OS" and an outdated version of Klipper, running HappyHare with an MMU on it can present several challenges. This guideline outlines the steps I took to successfully run ERCF v2 with HappyHare on my K1.

**Important Notice:** I am not a Creality engineer, just a user. I am not responsible for any issues that may arise with your printer.  
**Proceed at your own risk.**

# How to
1. **Reset the printer to factory default**  
   If you are sure that your printer has no conflicts running HH, you do not need to factory reset it!  

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
    + OPKG
    After installing all these, restart your K1.  
  
5. **Flash your MMU board**  
   You need to combine and flash your mmu board with Klipper that is same version with your printer is running.  
   If your printer is up to date. You can use [this](https://github.com/lamtranBKHN/creality_k1_make) klipper on other linux machine.  
   I used another Raspberry Pi to flash my ERB board.  
   **Since I dont know exact environment that you are using, I only give you the general step, you'll need to adapt the command to your (eg. path, ...)  **
    + Install klipper on your linux machine by: [This](https://github.com/dw-0/kiauh)  
    + Switch back Klipper version.  
    <pre> cd *Klipper parent folder* </pre>  
    <pre> mv klipper klipper_bk </pre>  
    <pre> git clone https://github.com/lamtranBKHN/creality_k1_make klipper </pre>  
    <pre> cd klipper </pre>  
    <pre> make clean </pre>  
    <pre> make menuconfig </pre>  
    Config your board and save.  
    <pre> make </pre>   
    Flash your mmu board.  

6. **Install HH**  
    <pre> cd /usr/data </pre>  
    <pre> git clone https://github.com/moggieuk/Happy-Hare.git </pre>  
    <pre> opkg update </pre>  
    <pre> opkg install bash </pre>  
    <pre> cd Happy-Hare </pre>  
    <pre> chmod u+x install.sh </pre>  
    <pre> cd Happy-Hare/extras </pre>  
    <pre> wget https://raw.githubusercontent.com/k1-801/Happy-Hare/k1/extras/mmu_toolhead.py </pre>  
    <pre> cd .. </pre>  
    <pre> bash ./install.sh -i </pre>  
    If you plan to install LED on your MMU, you need:  
    <pre> cd ~ </pre>  
    <pre> git clone https://github.com/julianschill/klipper-led_effect.git </pre>  
    <pre> mkdir /usr/share/klipper/extras </pre>  
    <pre> systemctl stop klipper </pre>  
    <pre> ln -s klipper-led_effect/led_effect.py </pre>  
    <pre> /usr/share/klipper/klippy/extras/led_effect.py </pre>  
    <pre> systemctl start klipper </pre>  

    Next, under mmu/base edit the mmu_macro_vars.cfg line 21 to  
    *filename: ~/../usr/data/printer_data/config/mmu/mmu_vars.cfg*  

    Then you need to calibrate your MMU: [HappyHare wiki](https://github.com/moggieuk/Happy-Hare/wiki/Hardware-Configuration)  

7. **Update your MCUs firmware**  
    **!Important: If your printer could not start up due to firmware mismatch, you need to do this step. If not, you're good to go!**  
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
        If you follow above steps and run it within 15s, I will get an success as bellow  
        ![MCU Flashed](./resource/mcu_flashed.png)  
    2. **Nozzle MCU**
    After flashing main mcu, your klipper might now through an error of not able to communication with MCU.  
    If you can reset MCUs from your mainsail, skip step 2.  
        1. ssh to your printer.  
        2. Unplug your nozzle mcu from your printer motherboard and replug it back again. You can find the nozzle header on [Board layout](https://guilouz.github.io/Creality-Helper-Script-Wiki/others/boards-layout-k1/)  
        3. Run bellow command  
        <pre> python3 mcu_util.py -c -i /dev/ttyS1 -g -u -f noz0_120_G30-noz0_015_000.bin </pre>  
    3. **Bed MCU**
    After flashing main mcu, your klipper might now through an error of not able to communication with MCU.  
    If you can reset MCUs from your mainsail, skip step 2.  
       1. ssh to your printer.  
       2. Unplug your bed mcu from your printer motherboard and replug it back again. You can find the nozzle header on [Board layout](https://guilouz.github.io/Creality-Helper-Script-Wiki/others/boards-layout-k1/)  
       3. Run bellow command  
        <pre> python3 mcu_util.py -c -i /dev/ttyS9 -g -u -f bed0_110_G21-bed0_012_000.bin </pre>  