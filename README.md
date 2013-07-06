live-iso-boot
=============

Download some live ISO images and put them into /boot/iso, then run sudo update-grub, and be able to boot straight into the live systems.

live-iso-boot includes the script debian which is installed in /etc/grub.d and run when update-grub is executed. The script looks for
ISO images in /boot/iso/ and adds an entry for each image found.

Howto
-----

- Get a recent live ISO image, e.g., from Ubuntu or debian-live
- Copy the image to /boot/iso
- Run update-grub
- Reboot and enjoy your new rescue system

Limitations
-----------

live-iso-boot uses the loopback feature of GRUB2, so it won't work with other bootloaders. Furthermore the distribution must support
booting from a loop-mounted ISO file, and live-iso-boot needs to guess some parameters based on the contents of the ISO. This
might not work for some distributions yet.
