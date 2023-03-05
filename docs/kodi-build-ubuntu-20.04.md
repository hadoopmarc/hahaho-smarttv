# Build Kodi from source on Ubuntu-20.04LTD

## Introduction
It can be a challenge to keep the versions of multiple independent software packages from an open source eco system consistent. In the current project, specific 3rd-party Kodi add-ons can predicate a particular Kodi version. When using Kodi as an applicance on a Raspberry Pi, life is simple, because the Kodi software including the OS can be downloaded as firmware for any Kodi version, see e.g. [https://archive.libreelec.tv/archive/](https://archive.libreelec.tv/archive/). However, when you want to install a specific Kodi version on a particular version of a particular operating system, chances are that no readily available software package is available. In that case, you have to build Kodi from source.

In the current case I wanted Kodi 19.5 Matrix to run on Ubuntu-20.04LTS Focal. When inspecting the [official Kodi launchpad](https://launchpad.net/~team-xbmc/+archive/ubuntu/ppa), it becomes clear that only a Kodi 20.0 Nexus package is available for Ubuntu Focal.

Therefore, this sections provides annotations on building Kodi Matrix from source on Ubuntu-20.04LTS, using:

- [Ubuntu instructions](https://github.com/xbmc/xbmc/blob/master/docs/README.Ubuntu.md)
- [Linux instructions](https://github.com/xbmc/xbmc/blob/master/docs/README.Linux.md)

Compared to these general instructions, the present annotations are specifically for:

 - cloning the kodi github repo to an arbitrary folder
 - checking out the 19.5-Matrix git tag
 - installing on ubuntu-20.04LTS focal
 - installing to the $HOME/.local folder (avoiding a system install with sudo when just trying out Kodi)

These annotations require some experience with building c software from source, so be prepared to invest some time for study if this is your first project to build.

## Make and install the main project:

Checkout the git repo and configure the cmake utility from a manually created kodi-build directory:
```bash
PROJECTS_DIR='/tera/TProjects'
cd "$PROJECTS_DIR"
git clone https://github.com/xbmc/xbmc.git
cd xbmc
git checkout 19.5-Matrix

mkdir "$PROJECTS_DIR"/kodi-build
cd "$PROJECTS_DIR"/kodi-build
cmake "$PROJECTS_DIR"/xbmc -DCMAKE_INSTALL_PREFIX=$HOME/.local -DCORE_PLATFORM_NAME=x11 -DAPP_RENDER_SYSTEM=gl
```

First attempt:
```bash
cmake --build . -- VERBOSE=1 -j$(getconf _NPROCESSORS_ONLN)

# E: Unable to locate package flatbuffers-dev
# E: Unable to locate package libshairplay-dev

sudo apt install libflatbuffers-dev
sudo add-apt-repository ppa:team-xbmc/ppa
sudo apt update
sudo apt install <long list of apt packages from instructions linked to above>
```

Second attempt:
```bash
cmake --build . -- VERBOSE=1 -j$(getconf _NPROCESSORS_ONLN)
/usr/bin/ld.gold: error: cannot find -lunistring
sudo apt install libunistring-dev
```

Third attempt:
```bash
cmake --build . -- VERBOSE=1 -j$(getconf _NPROCESSORS_ONLN)
# [100%] Built target kodi
# make[1]: Leaving directory '/tera/TProjects/kodi-build'
# /usr/bin/cmake -E cmake_progress_start /tera/TProjects/kodi-build/CMakeFiles 0

make install
```
For installation on a production system, follow the original instructions and install to /usr/local/ with sudo, to prevent accidental overwrites or deletes of the Kodi files.


## Make and install all add-ons in-tree:
Making all add-ons like below is not a particularly good idea, because on the first start Kodi will ask for each add-on whether you want to enable it. So, rather than using the command below, check the guides above to make and install a selection of add-ons.

```bash
cd "$PROJECTS_DIR"/xbmc
make -j$(getconf _NPROCESSORS_ONLN) -C tools/depends/target/binary-addons PREFIX=$HOME/.local
```
