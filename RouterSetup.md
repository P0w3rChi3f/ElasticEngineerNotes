# Router Build

Need pfsense box (black box - mine is labelled SG09, 4 lan, 1 com, 2 usb, 1 vga), keyboard, thumbdrive, no mouse required.

## Hardware Setup

1. Boot pfsence from thumbdrive
    
    a. Plug in VGA monitor before bootup.

1. Press `ESC` to enter BIOS Setup
1. Press `F11` to enter boot menu

## Initial Install

1. Press `A` to Accept
1. Press `I` to Install
1. Press `OK` for default US keymap
1. Select `Auto` for guided disk setup
    
    a. Install runs... select default options
1. Reboot
## Configure PFSence

Lan4 | Lan 3| Lan 2 | Lan 1 | Com
---- | ---- | ----- | ----- | --- 
em3  | em2  | em1   | em0   | local