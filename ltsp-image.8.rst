NAME
----

**ltsp image** - generate a squashfs image from an image source

SYNOPSIS
--------

**ltsp** [*ltsp-options*] **image** [**-b** *backup*] [**-c** *cleanup*]
[**-i** *ionice*] [**-k** *kernel-initrd*] [**-m** *mksquashfs-params*]
[**-r** *revert*] [*image*] …

DESCRIPTION
-----------

Compress a virtual machine image or chroot directory into a squashfs
image, to be used as the network root filesystem of LTSP clients. It’s
used in similar fashion to live CDs, i.e. all clients will boot from
this single read only image and then use SSHFS or NFS to mount
/home/username from the server.

OPTIONS
-------

See the **ltsp(8)** man page for *ltsp-options*.

**-b**, **–backup=**\ \_0|1\_
   Backup /srv/ltsp/images/*image*.img to *image*.img.old. Defaults to
   1.
**-c**, **–cleanup**\ =\ *0|1*
   Create a writeable overlay on top of the image source and temporarily
   remove user accounts and sensitive data before calling mksquashfs.
   Defaults to 1.
**-i**, **–ionice=**\ \_cmdline\_
   Set a prefix command to run mksquashfs with a lower priority, or
   specify "" to disable it completely. Defaults to ``nice ionice -c3``.
**-k**, **–kernel-initrd=**\ \_glob-regex\_
   Pass this parameter to the ``ltsp kernel`` call after the squashfs
   creation. See ltsp-kernel(8) for more information.
**-m**, **–mksquashfs-params=**\ \_“params”\_
   Pass *$params* to the mksquashfs call unquoted; so *params* shouldn’t
   contain spaces. See mksquashfs(1) for more information.
**-r**, **–revert**\ [=*0|1*]
   Move /srv/ltsp/images/*image*.img.old to *image*.img and call
   ``ltsp kernel image``. Useful when the clients won’t boot with the
   new image.

IMAGE TYPES
-----------

There are three “image” types in LTSP, in the following locations. The
/srv/ltsp path can be configured using ``ltsp --base-dir=``:

**/srv/ltsp/img_name.img**
   Source images are placed directly under /srv/ltsp and usually are
   symlinks to virtual machine raw disk files. They’re only used by
   ``ltsp image``.
**/srv/ltsp/img_name**
   Chroot directories can be used both as sources for ``ltsp image`` and
   as NFS root exports for the clients.
**/srv/ltsp/images/img_name.img**
   Exported images (usually squashfs) are placed under the images
   directory and the clients can netboot from them.

Images can be specified as simple names like ``ltsp image img_name``, in
which case the aforementioned locations are searched, or as or full
paths like ``ltsp image ~/VMs/vm.img``.

The supported image types result in the following three methods to use
LTSP. You may use either one of the methods or even all of them at the
same time.

CHROOTLESS
----------

Chrootless LTSP, previously called “ltsp-pnp”, is the recommended way to
maintain LTSP **if** its restrictions are acceptable. In this mode, the
server operating system itself is exported into a squashfs file and used
for netbooting all the clients. You, the sysadmin, would use the typical
GUI tools to manage the server, like software centers or update
managers. Then whenever necessary, you’d run:

.. code:: shell

       ltsp image /
