
`.deb` files - binary packages.

### What is in a `.deb` file?

Seeing with archive utility we see three parts
`ar -tv tandem-mass_2013.09.01-1_amd64.deb`

1. `debian-binary` - version of deb file format.
2. `control.tar.gz` - package meta-data like md5, triggers, (pre|post) etc.
3. `data.tar.gz` - actual data files of the package.

### Installing a `.deb` package

Use `dpkg` tool.

```sh
sudo dpkg -i hithere_1.0-1_amd64.deb
```

### Concepts

### upstream tarball:

Usually, people package software that someone else, called the upstream developer, has written.

The upstream developers will make a release of their software in the "upstream source archive file" or tarball.

A tarball is the `.tar.gz` or `.tgz` file upstream makes (tarball is a bit of computer jargon), and it can also be compressed to `.tar.bz2`,`.tb2` or `.tar.xz`. The tarball is exactly what Debian takes and builds a package out of.

### source package:

When you have the upstream tarball, the next step is to make the Debian source package.

The source package consists, in its simplest form, of three files:

* The upstream tarball, renamed to follow a systematic pattern.
* A debian directory, with any changes made to upstream source, plus all the files created for the Debian package. This has a `.debian.tar.gz` ending.
* A description file (with `.dsc` ending), which lists the other two files.

### binary package:

From this source package you can then build the Debian binary package which is what actually get installed.


### Creating debian packages the debian way

#### Prerequisites

1. `build-essential` package is necessary to build debian packages. Automatically includes `dpkg-dev`.
2. `devscripts`
3. `debhelper` - utils for deb packaging.

To make sure that a Debian package meets all build dependencies and is not influenced by anything specific to the user's environment, packages should be built in a `chroot` environment.
`chroot` on Unix-like operating systems is an operation that changes the apparent root directory for the current running process and its children.
Tools like `pbuilder` can be used for this.


### Building the deb package

1. Get upstream tarball & rename

No hyphens, replaced with underscore. Also appended `.orig` in name

```sh
mv hithere-1.0.tar.gz hithere_1.0.orig.tar.gz
```

2. Unpack upstream tarball:

```sh
tar xf hithere_1.0.orig.tar.gz
```

3. Add debian specific files/metadata/package info:

```sh
cd hithere_1.0
mkdir debian
```

List of files needed to be provided:
1. `debian/changelog` - log of changes to debian package (Usually upstream change summary)
Created via `dch` tool:
```sh
dch --create -v 1.0-1 --package hithere
```

Generated changelog file: 
```
hithere (1.0-1) UNRELEASED; urgency=low

  * Initial release. (Closes: #XXXXXX)

 -- Lars Wirzenius <liw@liw.fi>  Thu, 18 Nov 2010 17:25:32 +0000
```

**Note**:
The hithere part MUST be the same as the source package name. 1.0-1 is the version. The 1.0 part is the upstream version. The -1 part is the Debian version: the first version of the Debian package of upstream version 1.0. If the Debian package has a bug, and it gets fixed, but the upstream version remains the same, then the next version of the package will be called 1.0-2. Then 1.0-3, and so on.

UNRELEASED is called the upload target. It tells the upload tool where the binary package should be uploaded. UNRELEASED means the package is not yet ready to be uploaded. It's a good idea to keep the UNRELEASED there so you don't upload by mistake.

2. `debian/compat` - compatibility level for `debhelper` tool.

3. `debian/control` - Specifies human readable metadata of package.
The control file describes the source and binary package, and gives some information about them, such as their names, who the package maintainer is, and so on.

e.g.
```
Source: hithere
Maintainer: Lars Wirzenius <liw@liw.fi>
Section: misc
Priority: optional
Standards-Version: 3.9.2
Build-Depends: debhelper (>= 9)

Package: hithere
Architecture: any
Depends: ${shlibs:Depends}, ${misc:Depends}
Description: greet user
 hithere greets the user, or the world.
```

4. `debian/copyright`
5. `debian/rules`
6. `debian/source/format` - version number for the format of the source package.

4. `Building the package`

```sh
debuild -us -uc
```




