# Getting the Linux Source Tree

## Understanding the Linux kernel source trees

The first step in order to build the Linux kernel is to get the source tree. A source tree is a directory that contains
all the source code of a project. In this case, the Linux kernel source tree contains all the source code of the Linux
kernel.

The tricky part is to know which source tree to get. There are many different source trees available for the Linux
kernel which can be confusing for a newcomer. In this section we will go over the different source trees available and
we will explain which one is the best one to get for our purposes.

### Mainline source tree (Linus' tree)

This is the main source tree for the Linux kernel. It is maintained by Linus Torvalds, the creator of Linux. This is
where all the new features are added to the Linux kernel. As we will see later, this is the source tree that we will
use to build the Linux kernel.

The mainline Linux kernel source tree is hosted on [kernel.org](https://www.kernel.org/) which is the official website
of the Linux kernel.

To get the mainline Linux kernel source tree, we can use [git](https://git-scm.com/) to clone the repository:

```bash
$ git clone https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
$ cd linux
```

This repository also has a mirror on GitHub which can be found [here](https://github.com/torvalds/linux).

It is important to note that this source tree is not meant to be used directly, by developers.
It is meant to be used by subsystem maintainers to create their own source trees, more on that later. This is because
the Linux kernel is a very big project and it is not possible for Linus to maintain it by himself. That is why he has
created a system where other people can contribute to the Linux kernel. This system is called the Linux kernel
development process.

### Stable and long term releases source tree

This is the source tree that contains the stable versions of the Linux kernel. It's maintained mainly by
Greg Kroah-Hartman. This source tree is also hosted on [kernel.org](https://www.kernel.org/) and it can be found
[here](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/).

This source tree maintains all the stable and long term releases of the Linux kernel. This means that if you want to
use a stable/long term release of the Linux kernel, you can get it from this source tree. This is important because
companies usually want to use a stable/long term release of the Linux kernel in their products due to the fact that
that these releases are supported for a long time for bug fixes and security updates.

To get the stable Linux kernel source tree, we can use [git](https://git-scm.com/) to clone the repository:

```bash
$ git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
$ cd linux
```

You can find all Long Term Support (LTS) releases [here](https://www.kernel.org/category/releases.html).

As of today (2021-05-01),  this is the list of LTS releases:

<!-- table with all  LTS -->
| Version | Maintainer | Releases | Projected EOL |
| ------- | ---------- | -------- | ------------- |
| 6.1    | Greg Kroah-Hartman & Sasha Levin | 2022-12-11 | Dec, 2026 |
| 5.15   | Greg Kroah-Hartman & Sasha Levin | 2021-10-31 | Oct, 2026 |
| 5.10   | Greg Kroah-Hartman & Sasha Levin | 2020-12-13 | Dec, 2026 |
| 5.4    | Greg Kroah-Hartman & Sasha Levin | 2019-11-24 | Dec, 2025 |
| 4.19   | Greg Kroah-Hartman & Sasha Levin | 2018-10-22 | Dec, 2024 |
| 4.14   | Greg Kroah-Hartman & Sasha Levin | 2017-11-12 | Jan, 2024 |

If you have cloned the stable Linux kernel source tree, you can switch to a specific version by using the `git checkout`
command:

```bash
# Switch to the v5.10 LTS release branch
$ git checkout v5.10
```

### Subsystems source trees

Linux is a very big project and it is not possible for a single person to maintain it. It is maintained by a group of
people called the Linux kernel maintainers.

You can think of the Linux kernel development process a hierarchy of source trees. At the top of the hierarchy, we have
the mainline source tree which is maintained by Linus. Down the hierarchy, we have the subsystem source trees which are
maintained by the subsystem maintainers. The subsystem maintainers are responsible for reviewing the patches that are
sent to them and then send them to Linus. Linus is the only one who can merge the patches into the mainline tree.

I hope I will be able to write a more detailed explanation of the Linux kernel development process in the future where
we go over the process of creating a real patch and sending it to the subsystem maintainers and tracking it until it
gets merged into the mainline tree.

For now, just keep in mind that the Linux kernel development process is a hierarchy of source trees where the mainline
tree is at the top and the subsystem trees are at the bottom. As a developer, most probably you will be working on a
subsystem tree and not on the mainline tree.

Examples of subsystem trees are:

- [usb](https://git.kernel.org/pub/scm/linux/kernel/git/gregkh/usb.git/) - USB subsystem tree
- [net](https://git.kernel.org/pub/scm/linux/kernel/git/netdev/net.git) - Network subsystem tree
- [i2c](https://git.kernel.org/pub/scm/linux/kernel/git/wsa/linux.git/) - I2C subsystem tree

You can find a list of all the subsystem trees [here](https://git.kernel.org/pub/scm/linux/kernel/git/).

### The linux-next tree

The [-next tree](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) is a special tree that is used to
test new features and changes before they are merged into the mainline tree. If you are a developer and you want to
test your changes before sending them to the subsystem maintainers, you can test them in the -next tree as a way to
make sure that your changes don't break anything.

### Distribution/Vendor source trees

Each Linux distribution and vendor has its own source tree. The purpose of these source trees is to add patches that
are specific to the distribution or vendor. For example, if a vendor wants to add support for a new hardware, they
will add the necessary patches to their source tree and then release a new version of their kernel. Ideally, these
patches should be sent to the subsystem maintainers so that they can be merged into the mainline tree. However, this is
not always the case.

The general workflow is that the vendor will take a Linux kernel LTS release, add their patches on top of it and then
release a new version of their kernel. This is why you will see that most of the Linux distributions and vendors are
using an LTS release of the Linux kernel. Also some vendors will take a LTS release and maintain it for a long time
by backporting bug fixes and security patches to it.

To distinguish between an LTS release that is maintained by Greg Kroah-Hartman as part of the stable tree and an LTS
release that is maintained by a vendor, the vendor will add a suffix to the version number. For example, if a vendor
takes the LTS release v6.1 as its base, add its patches on top of it and then release a new version of its kernel, the
version number of the new kernel will be v6.1.y-vendor1 (y is the patch number from the stable tree). This way, we can
tell that this is a vendor specific release and not a stable release.

As an exercise let's try to find the source tree of a Linux distribution. For this exercise, we will use the Ubuntu
distribution as this is the one I use daily. To be precise, my current Ubuntu version is `Ubuntu 22.04 LTS (jammy)`.

To get the version of the Linux kernel that Ubuntu is using, we can use the following command:

```bash
$ cat /proc/version_signature
```

In my case, the output of the command above is:

```bash
$ Ubuntu 5.19.0-43.44~22.04.1-generic 5.19.17
```

Base on Ubuntu's version number and its own [documentation](https://ubuntu.com/kernel), we can tell that Ubuntu is using
the stable Linux kernel `v5.19.17`  as its base. The vendor suffix is `-43.44~22.04.1-generic`. The vendor suffix is
used to identify the Ubuntu specific patches and their means is out of the scope of this book.

### Simple diagram of the Linux kernel source trees


## Downloading the Linux kernel source tree

### From tarball

### From git
