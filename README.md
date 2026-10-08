# ISA_CH37X_XTIDE_DoC_combo_card
ISA card with a CH375/CH376 Chip or module, XTIDE ROM chip, and a Disk on Chip module.

I designed this card for my personal use with the intended purpose of being able to use it to be able to quickly boot-up, setup and/or troubleshoot older 8088-386 DOS computers. I have been using this card for awhile and I like it. I'm posting this here in case anybody else wants to try it or experiment with it. I'm not a professional anything so there my be hardware, firmware, and/or SW bugs.

<b>SW1:</b>
- SW1.1 ON = Disables USB hardware. Not currently implemented, see Known Issues
- SW1.2 ON = Disables the DoC module. This can be useful if you don't want it to boot from the DoC or because of an address conflict
- SW1.3 ON = Disables the XTIDE ROM. This can be useful on a system that doesn't need the XTIDE ROM and/or to avoid an address conflict

<br>
<b>Addr Sel jumpers:</b>
The Addr Sel jumpers can set the address of the XTIDE ROM and DoC to one of a few different values listed below. The specific addresses could be reprogrammed in the DOC_ROM_DEC pld.<br>
  - ROM#1 - OFF, ROM#2 - OFF --> ROM address = C0000h<br>
  - ROM#1 - ON, ROM#2 - OFF --> ROM address = C8000h<br>
  - ROM#1 - OFF, ROM#2 - ON --> ROM address = D0000h<br> 
  - ROM#1 - ON, ROM#2 - ON --> ROM address = D8000h<br>
  - DOC#1 - OFF, DOC#2 - OFF --> DOC address = C0000h<br>
  - DOC#1 - ON, DOC#2 - OFF --> DOC address = C8000h<br>
  - DOC#1 - OFF, DOC#2 - ON --> DOC address = D0000h<br>
  - DOC#1 - ON, DOC#2 - ON --> DOC address = D8000h<br>
<br>
<b>CH375 or CH376:</b>
There are two options for the USB port support that have been tested <br>
OPTION #1 - Install a CH375A or B SMT chip and all the supporting components (U3, Y1, J3, R3, C1, C2, C3, C4, C6, C8) <br>
OR <br>
OPTION #2 - Don't install any of the components above and instead install the 2x8 female header J4 and then use CH376 module such as https://www.aliexpress.us/item/3256807155423379.html. Double check the pinout of whatever module you buy to make sure it matches the labels on the card, keeping in mind the module is intended to be plugged in upside (component side) down. <br>
<br>
It is probable that a CH376 chip could be installed instead of the CH376 in option #1 or a CH375 module could be used instead of the CH376 module in option #2 if you can one with the correct pinout. This hasn't bee tested. <br>
<br>
<b>Known issues:</b><br>
  * Rev0 of this card had a defect where the MEMr and MEMw lines were swapped at the ISA connected. The card worked after bodging those connections. This was fixed in Rev1, which is what is uploaded here, so this USED to be and issue but should be fine now.
  * The USB disable switch is connected, but not implemented in the USB address decoding PLD yet, so it doesn't actually disable anything.
  * The USB address is hard coded as 0260h in the PLD, it can be changed by requires re-programming that chip.
  * The XTIDE has to be a 27256 chip. It is possible a 2764, 27128, 28256, etc could be used but you would have to check the datasheets/pinouts and bodge as necessary. Specifically, I think if a smaller chip is used, the VPP needs to be tied high, but you would have to double check.
  * The Disk on Chip module is specifically the M-Systems MD-2800-D08 chip. I'm not sure if other versions of this chip will work. Specifically, there is a chip that ends with something like -3V that is a 3V version of the chip and won't work.
  * The initial formatting of the DoC module was a challenge for me. At least on my system, the DoC was always detected first amd the computer would try to boot from it first, but if it isn't formatted yet, then it will fail. But if you boot it with the DoC disabled with the switch, then even after re-enabling it, the DoC software wouldn't detect the card. My solution was the slightly risking procedure of booting with the DoC disabled then after booting, re-moving the card, flipping the disable switch off, then re-installing the card with while the computer was still powered up. If I did that, then the DoC software would see the module and let me format it. All of this may have been complicated by quirks of my system, like I needed this card installed at boot because I needed the XTIDE ROM installed to be able to boot at all.
  * There are several options for a DOS driver for the CH375 chip, but if a CH376 is used, I was unable to find one. Eventually I was able to get AI to make a driver for the CH376. That driver is here: https://github.com/dfmcwhir/CH376DOS_driver
  * Regardless of which driver is used, this card has only been tested with the interrupt option on the driver set to 0 (disabled). To be honest there is part of the HW for this card that was copied from the original CH375 reference design that I don't understand. The PLD has the ability to make it so the INT from the CH375 can be read from the base address + 2 and it will be on D0. I don't know if with the CH735 stock driver it uses that port all the time, or if it uses it when the interrupt option on the driver is set to something other than 0, or ???. There is also another version of the CH375 reference board that specifically says it an interrupt version of the board, so maybe the driver interrupt value is only for that card? I know that my CH376 driver never uses that base address + 2 port, so I assume it will only work with the interrupt option set to 0.
  * The form factor of the board doesn't match ISA specifications in terms of offset from the rear of the MB and mounting hole spacing. This was done as a cost saving measure to keep the board size < 100mm x 100mm. The result of that is the USB port maybe hard to plug thing into through the gaps in the card bracket on the computer.
<br><br>
