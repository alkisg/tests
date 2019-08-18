# Announcing LTSP 19.08
LTSP has been redesigned and rewritten from scratch as part of a [GSoC 2019 project](https://summerofcode.withgoogle.com/projects/#4558570069164032), developed by [Alkis Georgopoulos](https://github.com/alkisg) under the mentorship of [vagrantc](https://github.com/vagrantc), [fottsia](https://github.com/fottsia) and [siahos](https://github.com/siahos), and the supervision of [GFOSS - Open Technologies Alliance](https://summerofcode.withgoogle.com/organizations/4954936912117760/). Many thanks to everyone involved for this opportunity to radically improve LTSP!

The new LTSP comes with a new goal: 
> Linux Terminal Server Project helps in netbooting LAN clients from a single installation that resides in a chroot or a VM on the LTSP server. This way maintaining tens or hundreds of clients is as easy as maintaining a single PC.

I.e. the focus now is in ease of maintenance, not in recycling old hardware. New technologies like diskless fat clients with UEFI, Wayland etc are supported, while thin client support is now reduced to "remote desktop with xfreerdp / x2go / VNC".

The old LTSP will now be called LTSP5 (it was first released in 2005), to distinguish it from the new versions that follow the year.month numbering. The first LTSP 19.08 release is considered an alpha version, which should work in many setups but isn't production ready nor feature complete yet.

**To try out the new version, [read the wiki](https://github.com/ltsp/ltsp/wiki/installation).** Note that in the 19.08 version, NBD swap and client-attached printers are not yet supported.

A list of LTSP5 sites/concepts/tools and their new equivalents follows, so that LTSP5 users can more quickly familiarize themselves with the new LTSP.

## LTSP upstream resources
The following sites are maintained by LTSP developers. That usually means "only basic documentation/feedback, but accurate and well maintained".
* [https://ltsp.github.io](https://ltsp.github.io): the main LTSP site; will be updated later on :)
* [https://github.com/ltsp/ltsp/issues](https://github.com/ltsp/ltsp/issues): file bugs or feature requests that can be addressed from the LTSP code base
* [https://github.com/ltsp/ltsp/wiki](https://github.com/ltsp/ltsp/wiki): upstream LTSP wiki; start with the [installation page](https://github.com/ltsp/ltsp/wiki/installation)
* [https://github.com/ltsp/ltsp](https://github.com/ltsp/ltsp): upstream LTSP code

## LTSP community resources
The following sites are maintained by the LTSP community. That could mean "more documentation/feedback including unusual scenarios, but some information might not be very accurate or up to date".
* [https://github.com/ltsp/community/issues](https://github.com/ltsp/community/issues): ask questions about LTSP. Anyone can answer, not just LTSP developers. It replaced the LTSP 5 mailing lists mentioned below.
* [https://github.com/ltsp/community/wiki](https://github.com/ltsp/community/wiki): community maintained LTSP wiki. Yes, that means you; upload your information there.
* [https://webchat.freenode.net/#ltsp?nick=ltsp_user?](https://webchat.freenode.net/#ltsp?nick=ltsp_user?): IRC chat channel

## LTSP5 resources
Resources for the old LTSP5; will become obsolete when users switch to the new LTSP.
* [https://sourceforge.net/p/ltsp/mailman/](https://sourceforge.net/p/ltsp/mailman/): LTSP 5 mailing lists
* [https://help.ubuntu.com/community/UbuntuLTSP](https://help.ubuntu.com/community/UbuntuLTSP): unmaintained LTSP 5 Ubuntu wiki
* [https://launchpad.net/ltsp](https://launchpad.net/ltsp): LTSP 5 project page, code and translations
* [http://ltsp.org](http://ltsp.org): unmaintained LTSP 5 server with wiki, irclogs etc

## Thin clients
LTSP now doesn't natively support thin clients. But you can still netboot
clients up to the login screen, and then set up a session that uses xfreerdp,
x2go or vnc to access the server in a manner similar to thin clients. The
community is invited to
[write wiki pages](https://github.com/ltsp/community/wiki) about that.

## Directories/files
The LTSP directories have changed for FHS compliancy:
 * /opt/ltsp: now in /srv/ltsp
 * /var/lib/tftpboot: now in /srv/tftp

The configuration files have also changed, and a single [/etc/ltsp/ltsp.conf file](https://github.com/ltsp/ltsp/blob/master/docs/ltsp.conf.5.md) now manages all client and server parameters.

## Equivalents
Here is an alphabetical list with the new equivalents of LTSP5 tools, or their deprecation notices:
 * getltscfg: replaced by an internal shell function
 * init-ltsp.d: [ltsp init](https://github.com/ltsp/ltsp/tree/master/ltsp/client/init)
 * jetpipe: deprecated; may be rewritten in python3 if there's need; see also [p910nd](https://manpages.debian.org/p910nd)
 * ldm: replaced by [pamltsp](https://github.com/ltsp/ltsp/blob/master/ltsp/client/login/pamltsp) and [pwmerge](https://github.com/ltsp/ltsp/blob/master/ltsp/client/login/pwmerge), which work with all display managers, like GDM, LightDM etc. The down side is that now when a new user is added, the sysadmin needs to run `ltsp initrd` and reboot the clients.
 * ldminfod: deprecated
 * lts.conf: [/etc/ltsp/ltsp.conf](https://github.com/ltsp/ltsp/blob/master/docs/ltsp.conf.5.md) uses somewhat changed syntax and manages both the clients and the server
 * ltsp-build-client: use VMs or `ltsp image /` to generate an initial chroot, then unmksqushfs it; or use debootstrap
 * ltsp-chroot: deprecated; may be reincluded if there's need
 * ltsp-cluster-info: deprecated
 * ltsp-config dnsmasq: [ltsp dnsmasq](https://github.com/ltsp/ltsp/blob/master/docs/ltsp-dnsmasq.8.md)
 * ltsp-config isc-dhcp-server: may be reincluded if there's need
 * ltsp-config lts.conf: run `man ltsp.conf` to see how to create /etc/ltsp/ltsp.conf
 * ltsp-config nbd-server: deprecated; may be reincluded if there's need
 * ltsp-config nfs: [ltsp nfs](https://github.com/ltsp/ltsp/blob/master/docs/ltsp-nfs.8.md)
 * ltspfs: deprecated
 * ltspfsd: deprecated
 * ltspfs_mount: deprecated
 * ltspfsmounter: deprecated
 * ltspfs_umount: deprecated
 * ltsp-genmenu: deprecated
 * ltsp-info: [ltsp info](https://github.com/ltsp/ltsp/blob/master/docs/ltsp-info.8.md)
 * ltsp-localapps: deprecated
 * ltsp-localappsd: deprecated
 * ltsp-open: deprecated
 * ltsp-remoteapps: deprecated; they may be implemented with an ssh-keygen /
   ssh -X wrapper if they are needed
 * ltsp-update-image: [ltsp image](https://github.com/ltsp/ltsp/blob/master/docs/ltsp-image.8.md)
 * ltsp-update-kernels: [ltsp kernel](https://github.com/ltsp/ltsp/blob/master/docs/ltsp-kernel.8.md)
 * ltsp-update-sshkeys: its functionality is included in [ltsp initrd](https://github.com/ltsp/ltsp/blob/master/docs/ltsp-initrd.8.md)
 * nbd-client-proxy: deprecated
 * nbd-proxy: deprecated
 * nbdswapd: deprecated; may be reincluded if there's need
 * screen.d: we don't handle VTs anymore
 * update-kernels: syslinux configuration was replaced by [ltsp ipxe](https://github.com/ltsp/ltsp/blob/master/man/ltsp-ipxe.8.md)
