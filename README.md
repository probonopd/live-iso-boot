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
* Package patch-dracut-initrd

Investigation into Fedora-type Live ISOs
----------------------------------------

Unlike most other Live images, Fedora-type Live images currently do not include (documented) boot-time kernel arguments to set the language and the keyboard, among other things.

Fedora-type ISOs use a set of projects-tools, including
 * dracut  (the system that creates the initamfs used on the Live ISO)
 * lorax (the system that builds the live ISO, it calls dracut and tells it which modules to inculde in the initramfs; currently it apparently uses dracut --nomdadmconf --nolvmconf --xz --add "livenet dmsquash-live convertfs pollcdrom" --omit plymouth)
 
So to get additional functionality into Fedora live systems, I need to
 * Write a dracut module that does what I need and get the dracut project to accept it
 * Convince the lorax project that they should tell dracut at ISO creation time that my module should be included

Or
 * Hook into Fedora in a non-initramfs specific way, e.g., 
  * /etc/init.d/livesys sets the "liveuser" up and makes it autologin, and brings up the nag splash screen. Likely this would be the most approriate location to parse additional arguments and act upon them. Interestingly, rpm -qf states that the file /etc/init.d/livesys is not owned by any package - so where should I send patches to? Apparently it gets generated by the kickstart file that generates the Live image. Now I would need to know in which rpm is the kickstart file that generates the Fedora Live CD images... 
  * systemd is supposed to act upon certain kernel command line arguments, e.g., systemd.setenv= and locale.LANG= but I could not get it to work

