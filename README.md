

# Proxmox Edge kernels

Custom Linux kernels for Proxmox VE 7 - Fork to add support for T2 Macs.

The fork simply contains the CI setup to compile kernels using the scripts and documentation from [fabianishere/pve-edge-kernel](https://github.com/fabianishere/pve-edge-kernel) and [additional patches](https://github.com/t2linux/linux-t2-patches) to support T2 Macs.

You should also refer to the [t2linux wiki](https://wiki.t2linux.org/) for help regarding miscellaneous topics related to T2 Macs.

## Installation
Select the kernel required from the [Releases](https://github.com/AdityaGarg8/pve-edge-kernel-t2/releases)
page you want to install and download the appropriate Debian package.
Then, you can install the package as follows:

```sh
apt install ./pve-kernel-VERSION_amd64.deb
```

**Note :- This fork simply uses already tried and tested scripts by fabianishere and using separate scripts is out of the scope of this fork. Reason being that I have never used proxmox before, not even on a normal PC, and do not intend to do so in the future as well. Thus, it leads to lack of testing on my part which is not a good thing to do when distributing software. So, I'll stick to using fabianishere's scripts and thus new kernels will only be released when fabianishere releases them.**

## Building manually
You may also choose to manually build one of these kernels yourself. Refer to the [CI](https://github.com/AdityaGarg8/pve-edge-kernel-t2/blob/master/.github/workflows/build.yml) for help.

#### Prerequisites
Make sure you have at least 10 GB of free space available and have the following
packages installed:

```bash
apt install devscripts debhelper equivs git
```

## Removal
Use `apt` to remove individual kernel packages from your system. If you want
to remove all packages from a particular kernel release, use the following
command:

```bash
apt remove pve-kernel-6.0*edge pve-headers-6.0*edge
```

## Credits
Following are the people/groups that made this fork possible and the links to contribute to them:
1. [fabianishere](https://www.buymeacoffee.com/fabianishere)
2. [t2linux](https://wiki.t2linux.org/contribute/)

## Contributing
Questions, suggestions and contributions are welcome and appreciated!
You can contribute in various meaningful ways:

* Report a bug through [Github issues](https://github.com/AdityaGarg8/pve-edge-kernel-t2/issues). Please report bugs only if you feel they are specific to T2 Macs. If your bug is something unrelated to T2 Macs and instead proxmox specific, I'd suggest you to report them to [fabianishere](https://github.com/fabianishere/pve-edge-kernel).
* Propose new patches and flavors for the project.
* Contribute improvements to the documentation.
* Provide feedback about how we can improve the project.

## Donations

I accept donations via Amazon Pay Gift Cards. I you wanna appreciate my work by donating, you can refer to [this link](https://wiki.t2linux.org/contribute/#support-our-maintainers) to donate me.
