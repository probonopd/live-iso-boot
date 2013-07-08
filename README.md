live-iso-boot
=============

Download some live ISO images and put them into /boot/iso, then run sudo update-grub, and be able to boot straight into the live systems.

live-iso-boot includes the script debian which is installed in /etc/grub.d and run when update-grub is executed. The script looks for
ISO images in /boot/iso/ and adds an entry for each image found.

Howto
-----

- Get a recent live ISO image, e.g., from Ubuntu, Fedora, or debian-live
- Copy the image to /boot/iso
- Run update-grub
- Reboot and enjoy your new rescue system

Limitations
-----------

live-iso-boot uses the loopback feature of GRUB2, so it won't work with other bootloaders. Furthermore the distribution must support
booting from a loop-mounted ISO file, and live-iso-boot needs to guess some parameters based on the contents of the ISO. This
might not work for some distributions yet.

Working live ISOs
-----------------

casper-based
 * http://releases.ubuntu.com/precise/ubuntu-12.04.2-desktop-i386.iso

debian-live based
 * http://live-build-cgi.debian.net/cgi-bin/live-build wheezy images
 * http://ftp.halifax.rwth-aachen.de/kali-images/kali-linux-1.0.3-i386.iso

dracut-based (language and keyboard setting do not work yet)
 * http://download.fedoraproject.org/pub/fedora/linux/releases/19/Live/i386/Fedora-Live-Desktop-i686-19-1.iso

TODO
----

* Add support for more live images
* Provide more information about the images besides just the filename in the GRUB menu
* Integrate with https://travis-ci.org/
* Provide a way to set the language of Fedora-type Live ISOs

Investigation into Fedora-type Live ISOs
----------------------------------------

Fedora-type ISOs use a set of projects-tools, including
 * dracut  (the system that creates the initamfs used on the Live ISO)
 * lorax (the system that builds the live ISO, it calls dracut and tells it which modules to inculde in the initramfs)
 
So to get additional functionality into Fedora live systems, I need to
 * Write a dracut module that does what I need and get the dracut project to accept it
 * Convince the lorax project that they should tell dracut at ISO creation time that my module should be included

Or
 * Hook into Fedora in a non-initramfs specific way, e.g., how do they bring up the "Do you want to try out Fedora or install it" nag screen on the Live ISO?
