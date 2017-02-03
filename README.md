PKGBUILDs for SELinux support in Arch Linux
===========================================

Complete documentation will soon be available at:
https://wiki.archlinux.org/index.php/SELinux

Authors
-------

Authors are credited in the PKGBUILD file for each package.

Binary repository
-----------------

No binary repository available at the moment.

Build order
-----------

Remember to build as a non-root user, and to keep a root logged-in console to
install packages (especially for sudo/shadow/pam packages).

* linux-selinux (support in Arch Official kernel on the way:
  https://bugs.archlinux.org/task/37578) can be built at any time.

First, we build all packages from the SELinux userspace projet. They do not
replace any official Arch Linux packages:

* libsepol
* libselinux
* checkpolicy setools
* libcgroup libsemanage sepolgen
* policycoreutils

Now we start replacing core packages:

* pambase-selinux
* pam-selinux
* coreutils-selinux shadow-selinux cronie-selinux sudo-selinux
* util-linux-selinux
* systemd-selinux

Optionnal but very nice to have:
* openssh-selinux findutils-selinux psmisc-selinux

Policy
------

There is not yet a SELinux policy for Arch.  To build a policy, here are some useful links:

* https://github.com/TresysTechnology/refpolicy The Reference Policy
* https://github.com/pebenito/refpolicy ongoing work to include a systemd policy in the refpolicy (announcement: http://oss.tresys.com/pipermail/refpolicy/2014-October/007430.html)
* http://anonscm.debian.org/cgit/selinux/refpolicy.git/tree/debian/patches Debian patches for refpolicy package (including systemd patches)
* https://github.com/selinux-policy/selinux-policy/tree/rawhide-base Fedora policy

How I Used This Repo
--------------------
This is around mostly as a reference for myself but others looking to contribute.

I used a vagrant machine with libvirt as a provider. Currently this provider is broken for Arch but I don't like Virtualbox. Most credit for this goes to fischilico who helped me get it up and running.

Vagrant 
```
yaourt -S vagrant vagrant-libvirt
sudo sed 's/libruby\.so\.2\.4/libruby.so.2.2/' -i /opt/vagrant/embedded/gems/extensions/x86_64-linux/2.2.0/ruby-libvirt-0.7.0/_libvirt.so
vagrant plugin install vagrant-libvirt

```

A couple weeks ago, as I am told, terrywang's archlinux vagrant image is borken for libvirt. Using packer-arch I was able to create a compatible image.
```
git clone https://github.com/elasticdog/packer-arch.git
cd packer-arch
#Their readme isn't up to date for libvirt Provider. I used qemu becuase I prefer it plus I like it for emulating my arm devices
yaourt -S qemu
./wrpacker -c US -p qemu 
vagrant box add archlinux output/packer_arch_qemu.box
```

After this I was able to run ``vagrant up`` and get myself going!

