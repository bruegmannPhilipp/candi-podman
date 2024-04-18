candi-podman (Compile &amp; Install)
=====

The ``candi.sh`` shell script downloads, configures, builds, and installs
podman in version 4 for ubuntu 22.04 from source.



Quickstart
----

The following commands download the current stable version of the installer and
then install the latest deal.II release and common dependencies:

```bash
  git clone https://github.com/koecher/candi-podman.git
  cd candi-podman
  ./candi.sh
```

Follow the instructions on the screen
(you can abort the process by pressing < CTRL > + C)

Advanced Configuration
----

### Command line options

#### Help: ``[-h]``, ``[--help]``
You can get a list of all command line options by running
```bash
  ./candi.sh -h
  ./candi.sh --help
```

You can combine the command line options given below.

#### Prefix path: ``[-p <path>]``, ``[-p=<path>]``, ``[--prefix=<path>]``
```bash
  ./candi.sh -p "/path/to/install/dir"
  ./candi.sh -p="/path/to/install/dir"
  ./candi.sh --prefix="/path/to/install/dir"
```

#### Multiple build processes: ``[-j<N>]``, ``[-j <N>]``, ``[--jobs=<N>]``
```bash
  ./candi.sh -j<N>
  ./candi.sh -j <N>
  ./candi.sh --jobs=<N>
```

* Example: to use 2 build processes type ``./candi.sh -j 2``.
* Be careful with this option! You need to have enough system memory (e.g. at
  least 8GB for 2 or more processes).

#### Specific platform: ``[-pf=<platform>]``, ``[--platform=<platform>]``
```bash
  ./candi.sh -pf=./deal.II-toolchain/platforms/...
  ./candi.sh --platform=./deal.II-toolchain/platforms/...
```

If your platform is not detected automatically you can specify it with this
option manually. As shown above, this option is used to install deal.II via
candi on linux clusters, for example. For a complete list of supported platforms
see [deal.II-toolchain/platforms](deal.II-toolchain/platforms).

#### User interaction: ``[-y]``, ``[--yes]``, ``[--assume-yes]``
```bash
  ./candi.sh -y
  ./candi.sh --yes
  ./candi.sh --assume-yes
```

With this option you skip the user interaction. This might be useful if you
submit the installation to the queueing system of a cluster.


### Configuration file options

If you want to change the set of packages to be installed,
you can enable or disable a package in the configuration file
[candi.cfg](candi.cfg).
This file is a simple text file and can be changed with any text editor.

For a complete list of packages see
[podman/packages](podman/packages).

There are several options within the configuration file, for example:

* Remove existing build directories to use always a fresh setup
```bash
  CLEAN_BUILD={ON|OFF}
```

and more.

Furthermore you can specify the install directory and other internal
directories, where the source and build files are stored:
* The ``DOWNLOAD_PATH`` folder (can be safely removed after installation)
* The ``UNPACK_PATH`` folder of the downloaded packages (can be safely removed
  after installation)
* The ``BUILD_PATH`` folder (can be safely removed after installation)
* The ``INSTALL_PATH`` destination folder


### Single package installation mode

If you prefer to install only a single package, you can do so by
```bash
  ./candi.sh --packages="podman"
```
for instance, or a set of packages by
```bash
  ./candi.sh --packages="firstpackage otherpackage"
```

### Developer mode

Our installer provides a software developer mode by setting
``DEVELOPER_MODE=ON``
within [candi.cfg](candi.cfg).

More precisely, the developer mode skips the package ``fetch`` and ``unpack``,
everything else (package configuration, building and installation) is done
as before.

Note that you need to have a previous run of candi and
you must not remove the ``UNPACK_PATH`` directory.
Then you can modify source files in ``UNPACK_PATH`` of a package and
run candi again.
