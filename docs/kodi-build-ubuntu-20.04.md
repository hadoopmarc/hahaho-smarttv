This sections provides annotations on building kodi Matrix from source on Ubuntu-20.04LTS, using:
https://github.com/xbmc/xbmc/blob/master/docs/README.Ubuntu.md
https://github.com/xbmc/xbmc/blob/master/docs/README.Linux.md

Compared to these general instructions, the present annotations are specifically for:
 - the kodi github repo is cloned into /tera/Projects
 - the 19.5-Matrix tag is checked out
 - installation is to $HOME/.local (avoiding sudo when just trying out) 
 - ubuntu-20.04LTS focal

##Make the main project:

Configure from the manually made kodi-build directory:
cmake /tera/TProjects/xbmc -DCMAKE_INSTALL_PREFIX=$HOME/.local -DCORE_PLATFORM_NAME=x11 -DAPP_RENDER_SYSTEM=gl

First attempt:
cmake --build . -- VERBOSE=1 -j$(getconf _NPROCESSORS_ONLN)

E: Unable to locate package flatbuffers-dev
E: Unable to locate package libshairplay-dev

sudo apt install libflatbuffers-dev
sudo add-apt-repository ppa:team-xbmc/ppa
sudo apt update
sudo apt install <long list from instructions>

Second attempt:
cmake --build . -- VERBOSE=1 -j$(getconf _NPROCESSORS_ONLN)
/usr/bin/ld.gold: error: cannot find -lunistring
sudo apt install libunistring-dev

Third attempt:
cmake --build . -- VERBOSE=1 -j$(getconf _NPROCESSORS_ONLN)
[100%] Built target kodi
make[1]: Leaving directory '/tera/TProjects/kodi-build'
/usr/bin/cmake -E cmake_progress_start /tera/TProjects/kodi-build/CMakeFiles 0

make install

##Make all add-ons in-tree:
cd /tera/TProjects/xbmc
make -j$(getconf _NPROCESSORS_ONLN) -C tools/depends/target/binary-addons PREFIX=$HOME/.local

