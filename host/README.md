This repository contains host software (Linux/Windows) for HackRF, a project to
produce a low cost, open source software radio platform.

##How to build the host software on Linux:

###Prerequisites for Linux (Debian/Ubuntu):
`sudo apt-get install build-essential cmake libusb-1.0-0-dev pkg-config libfftw3-dev`

###Build host software on Linux:
```
mkdir host/build
cd host/build
cmake ..
make
sudo make install
sudo ldconfig
```

By defualt this will attempt to install a udev rule to '/etc/udev/rules.d` to
provide the `usb` or `plugdev` group access to HackRF. If your setup requires
the udev rule to be installed elsewhere you can modify the path with
`-DUDEV_RULES_PATH=/path/to/udev`.

Note: The udev rule is not installed for by default for PyBOMBS installs as
they do not ususally get installed with root privileges.

##Clean CMake temporary files/dirs:
```
cd host/build
rm -rf *
```

##How to build host software on Windows:
###Prerequisites for cygwin, mingw, or Visual Studio:

* cmake-2.8.12.1 or more see http://www.cmake.org/cmake/resources/software.html
* libusbx-1.0.18 or more see http://sourceforge.net/projects/libusbx/files/latest/download?source=files
* Install Windows driver for HackRF hardware or use Zadig see http://sourceforge.net/projects/libwdi/files/zadig
  - If you want to use Zadig  select HackRF USB device and just install/replace it with WinUSB driver.

>**Note for Windows build:**
 You shall always execute hackrf-tools from Windows command shell and not from Cygwin or Mingw shell because on Cygwin/Mingw
 Ctrl C is not managed correctly and especially for hackrf_transfer the Ctrl C(abort) will not stop correctly and will corrupt the file.

###For Cygwin:
```
mkdir host/build
cd host/build
cmake ../ -G "Unix Makefiles" -DCMAKE_LEGACY_CYGWIN_WIN32=1 -DLIBUSB_INCLUDE_DIR=/usr/local/include/libusb-1.0/
make
make install
```

###For MinGW:
```
mkdir host/build
cd host/build
cmake ../ -G "MSYS Makefiles" -DLIBUSB_INCLUDE_DIR=/usr/local/include/libusb-1.0/
make
make install
```

###For Visual Studio 2015 x64
```
c:\hackrf\host\build> cmake ../ -G "Visual Studio 14 2015 Win64" \
-DLIBUSB_INCLUDE_DIR=c:\libusb-1.0.18-win\include\libusb-1.0 \
-DLIBUSB_LIBRARIES=c:\libusb-1.0.18-win\MS64\static\libusb-1.0.lib \
-DTHREADS_PTHREADS_INCLUDE_DIR=c:\pthreads-w32-2-9-1-release\Pre-built.2\include \
-DTHREADS_PTHREADS_WIN32_LIBRARY=c:\pthreads-w32-2-9-1-release\Pre-built.2\lib\x64\pthreadVC2.lib
```

Cmake will produce a solution file named `hackrf_all.sln` and a series of
project files which can be used with msbuild as follows:
`c:\hackrf\host\build> msbuild ALL_BUILD.vcxproj`

##How to build host the software on FreeBSD
You can use the binary package:
`# pkg install hackrf`

You can build and install from ports:
```
# cd /usr/ports/comms/hackrf
# make install
```

principal author: Michael Ossmann <mike@ossmann.com>

http://greatscottgadgets.com/hackrf/
