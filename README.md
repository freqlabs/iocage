iocage
======

**A FreeBSD jail manager.**

iocage is a zero dependency, drop in jail/container manager amalgamating some
of the best features and technologies the FreeBSD operating system has to offer.
It is geared for ease of use with a simple and easy to understand command syntax.

iocage is in the FreeBSD ports tree as sysutils/iocage.
To install using binary packages, simply run: `pkg install iocage`

- **[DOCUMENTATION](http://iocage.readthedocs.org/en/latest/index.html)**
- **IRC**: [Freenode #iocage](https://www.irccloud.com/invite?hostname=irc.freenode.net&channel=%23iocage)
- **Mailing list**: iocage@googlegroups.com
- **Contributing**: If you would like to submit a pull request, we kindly ask you to open it against the `develop` branch.

**FEATURES:**
- Templates, clones, basejails, fully independent jails
- Ease of use
- Zero configuration files
- Rapid thin provisioning within seconds
- Automatic package installation
- Virtual networking stacks (vnet)
- Shared IP based jails (non vnet)
- Resource limits (CPU, MEMORY, etc.)
- Filesystem quotas and reservations
- Dedicated ZFS datasets inside jails
- Transparent ZFS snapshot management
- Binary updates
- Differential jail packaging
- Export and import
- And many more!

**QUICK HOWTO:**

Fetch a release:

`iocage fetch`

Create a jail:

`iocage create tag=myjail ip4_addr="em0|192.168.1.10/24"`

Start the jail:

`iocage start myjail`

**USAGE:**
-  iocage activate ZPOOL
-  iocage fetch [release=RELEASE | ftphost=ftp.hostname.org | ftpdir=/dir/ | ftpfiles="base.txz doc.txz lib32.txz src.txz"]
-  iocage create [-b|-c|-e] [release=RELEASE] [pkglist=file] [property=value]
-  iocage clone UUID|TAG@snapshot [property=value]
-  iocage destroy [-f] UUID|TAG|ALL
-  iocage reset UUID|TAG|ALL
-  iocage list [-t|-r]
-  iocage start UUID|TAG
-  iocage stop UUID|TAG
-  iocage restart UUID|TAG
-  iocage rcboot
-  iocage rcshutdown
-  iocage console UUID|TAG
-  iocage exec [-u username | -U username] UUID|TAG command [arg ...]
-  iocage chroot UUID|TAG [command]
-  iocage df
-  iocage show property
-  iocage get property|all UUID|TAG
-  iocage set property=value UUID|TAG
-  iocage cap UUID|TAG
-  iocage limits UUID|TAG
-  iocage uncap UUID|TAG
-  iocage inuse [UUID|TAG]
-  iocage snapshot UUID|TAG@snapshotname
-  iocage snaplist UUID|TAG
-  iocage snapremove UUID|TAG@snapshotname|ALL
-  iocage rollback UUID|TAG@snapshotname
-  iocage promote UUID|TAG
-  iocage runtime UUID|TAG
-  iocage update UUID|TAG
-  iocage upgrade UUID|TAG
-  iocage record start|stop UUID|TAG
-  iocage package UUID|TAG
-  iocage export UUID|TAG
-  iocage import UUID [property=value]
-  iocage defaults
-  iocage version | --version
-  iocage help

**REQUIREMENTS**
- FreeBSD 9.3-RELEASE amd64 or newer
- ZFS file system
- Optional - Kernel compiled with:

        # This is optional and only needed if you need VNET and resource
        # limits

        options         VIMAGE # VNET/Vimage support
        options         RACCT  # Resource containers
        options         RCTL   # same as above

**OTHER CONSIDERATIONS**
- For resource limiting please read rctl(8)
- For the explanations on jail properties read jail(8)
- Create bridge0 and bridge1 interfaces

**HINTS**
- Use iocage set/get to modify properties
- To understand what most properties do read iocage(8).
- If using VNET consider adding the following to `/etc/sysctl.conf` on the host:

        net.inet.ip.forwarding=1       # Enable IP forwarding between interfaces
        net.link.bridge.pfil_onlyip=0  # Only pass IP packets when pfil is enabled
        net.link.bridge.pfil_bridge=0  # Packet filter on the bridge interface
        net.link.bridge.pfil_member=0  # Packet filter on the member interface
