# Announcing LTSP 19.08

LTSP has been redesigned and rewritten from scratch as part of a [GSoC 2019 project]((https://summerofcode.withgoogle.com/projects/#4558570069164032)), developed by [Alkis Georgopoulos](https://github.com/alkisg) under the mentorship of [vagrantc](https://github.com/vagrantc), [fottsia](https://github.com/fottsia) and [siahos](https://github.com/siahos), and the supervision of [GFOSS - Open Technologies Alliance](https://summerofcode.withgoogle.com/organizations/4954936912117760/). Many thanks to everyone involved for this opportunity to radically improve Epoptes!

. The developer, [Alkis Georgopoulos](https://github.com/alkisg), wishes to thank his mentors, 

The new LTSP has been rewritten from scratch and shares almost no code with
LTSP5. Compatibility was completely broken, and only some concepts remained
the same. It's recommended that you completely purge LTSP5 (backup first if
you want) before installing the newer LTSP.

## No thin clients
LTSP now doesn't natively support thin clients. But you can still netboot
clients up to the login screen, and then set up a session that uses xfreerdp,
x2go or vnc to access the server in a manner similar to thin clients. The
community is invited to
[write wiki pages](https://github.com/ltsp/community/wiki) about that.

## LTSP5 equivalents
The LTSP directories have changed for FHS compliancy:
 * /opt/ltsp: now in /srv/ltsp
 * /var/lib/tftpboot: now in /srv/tftp

And here is a list of LTSP5 tools/concepts and their new equivalents, or the
deprecation notices:
 * getltscfg: replaced by an internal awk function
 * init-ltsp.d: [ltsp init](https://github.com/ltsp/ltsp/blob/master/man/ltsp-client.conf.5.md)
 * jetpipe: included for now; see also [p910nd](https://manpages.debian.org/p910nd)
 * lts.conf: [/etc/ltsp/client.conf](https://github.com/ltsp/ltsp/blob/master/man/ltsp-client.conf.5.md) with completely different directives
 * ltsp-build-client: use VMs or `ltsp image /` to generate an initial chroot,
   then unmksqushfs it; or use debootstrap
 * ltsp-chroot: deprecated; may be reincluded if there's need
 * ltsp-cluster-info: deprecated
 * ltsp-config dnsmasq: [ltsp dnsmasq](https://github.com/ltsp/ltsp/blob/master/man/ltsp-dnsmasq.8.md)
 * ltsp-config isc-dhcp-server: deprecated; may be reincluded if there's need
 * ltsp-config lts.conf: create /etc/ltsp/client.conf manually
 * ltsp-config nbd-server: deprecated; may be reincluded if there's need
 * ltsp-config nfs: [ltsp nfs](https://github.com/ltsp/ltsp/blob/master/man/ltsp-nfs.8.md)
 * ltspfs: deprecated
 * ltspfsd: deprecated
 * ltspfs_mount: deprecated
 * ltspfsmounter: deprecated
 * ltspfs_umount: deprecated
 * ltsp-genmenu: deprecated
 * ltsp-info: [ltsp info](https://github.com/ltsp/ltsp/blob/master/man/ltsp-info.8.md)
 * ltsp-localapps: deprecated
 * ltsp-localappsd: deprecated
 * ltsp-open: deprecated
 * ltsp-remoteapps: deprecated; they may be implemented with an ssh-keygen /
   ssh -X wrapper if they are needed
 * ltsp-update-image: [ltsp image](https://github.com/ltsp/ltsp/blob/master/man/ltsp-image.8.md)
 * ltsp-update-kernels: [ltsp kernel](https://github.com/ltsp/ltsp/blob/master/man/ltsp-kernel.8.md)
 * ltsp-update-sshkeys: its functionality is included in [ltsp initrd](https://github.com/ltsp/ltsp/blob/master/man/ltsp-initrd.8.md)
 * nbd-client-proxy: deprecated
 * nbd-proxy: deprecated
 * nbdswapd: deprecated; may be reincluded if there's need
 * screen.d: we don't handle VTs anymore
 * update-kernels: syslinux configuration was replaced by [ltsp ipxe](https://github.com/ltsp/ltsp/blob/master/man/ltsp-ipxe.8.md)
