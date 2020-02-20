For debian like users (etc. Ubuntu or Debian), you can follow this step to build DECAF at your workspace.

#### Step 1
Install dependencies.
```shell
sudo apt-get install -y libsdl1.2-dev zlib1g-dev libglib2.0-dev libbfd-dev build-essential binutils qemu libboost-dev git libtool autoconf xorg-dev

---hz
sudo apt-get install libtool autoconf xorg-dev
Err:1 http://cn.archive.ubuntu.com/ubuntu bionic/main amd64 libfreetype6-dev amd64 2.8.1-2ubuntu2
  504  Gateway Timeout [IP: 117.128.6.16 80]
Fetched 43.8 kB in 2min 24s (304 B/s)       
Unable to correct missing packages.
E: Failed to fetch http://117.128.6.16/cache/cn.archive.ubuntu.com/ubuntu/pool/main/f/freetype/libfreetype6-dev_2.8.1-2ubuntu2_amd64.deb?ich_args2=506-19233013014878_bb2b75b6fa50cdd1f9d97f1442be2ae7_10001002_9c896c2ed6c1f7d39538518939a83798_6a2726bf466a26e3f56bc69c3916a008  504  Gateway Timeout [IP: 117.128.6.16 80]
E: Failed to fetch http://cn.archive.ubuntu.com/ubuntu/pool/main/p/pixman/libpixman-1-dev_0.34.0-2_amd64.deb  Connection failed [IP: 91.189.91.14 80]
E: Aborting install.
hz@hz:~/ws/other/DECAF/decaf$ 
--hz
sudo apt-get install -y libglib2.0-dev
selecting 'binutils-dev' instead of 'libbfd-dev'
---
hz@hz:~/ws/other/DECAF/decaf$ sudo apt-get install libsdl2-dev
The following packages have unmet dependencies:
 libsdl2-dev : Depends: libegl1-mesa-dev but it is not going to be installed
               Depends: libgles2-mesa-dev but it is not going to be installed
               Depends: libpulse-dev but it is not going to be installed
               Depends: libudev-dev but it is not going to be installed
E: Unable to correct problems, you have held broken packages.

------

 sudo aptitude install libsdl1.2-dev -o APT::Get::Fix-Missing=true
Accept this solution? [Y/n/q/?] Y
The following packages will be DOWNGRADED:
  libasound2 libasound2-data libegl1 libgl1 libgles2 libglvnd0 libglx0 libpng16-16 
  libpulse-mainloop-glib0 libpulse0 libpulsedsp pulseaudio pulseaudio-module-bluetooth 
  pulseaudio-utils 
The following NEW packages will be installed:
  libasound2-dev{a} libcaca-dev{a} libdrm-dev{a} libgl1-mesa-dev{a} libglib2.0-dev{a} 
  libglib2.0-dev-bin{a} libglu1-mesa-dev{a} libglvnd-core-dev{a} libglvnd-dev{a} libopengl0{a} 
  libpcre16-3{a} libpcre3-dev{a} libpcre32-3{a} libpcrecpp0v5{a} libpng-dev{a} libpng-tools{a} 
  libpulse-dev{a} libsdl1.2-dev libslang2-dev{a} libx11-xcb-dev{a} libxcb-dri2-0-dev{a} 
  libxcb-dri3-dev{a} libxcb-glx0-dev{a} libxcb-present-dev{a} libxcb-randr0-dev{a} 
  libxcb-render0-dev{a} libxcb-shape0-dev{a} libxcb-sync-dev{a} libxcb-xfixes0-dev{a} 
  libxdamage-dev{a} libxext-dev{a} libxfixes-dev{a} libxshmfence-dev{a} libxxf86vm-dev{a} 
  mesa-common-dev{a} pkg-config{a} x11proto-damage-dev{a} x11proto-dri2-dev{a} 
  x11proto-fixes-dev{a} x11proto-gl-dev{a} x11proto-xext-dev{a} x11proto-xf86vidmode-dev{a} 
0 packages upgraded, 42 newly installed, 14 downgraded, 0 to remove and 3 not upgraded.
Need to get 7,914 kB of archives. After unpa

Setting up libglu1-mesa-dev:amd64 (9.0.0-2.1build1) ...
Processing triggers for libc-bin (2.27-3ubuntu1) ...
E: Failed to fetch http://cn.archive.ubuntu.com/ubuntu/pool/main/p/pulseaudio/pulseaudio_11.1-1ubuntu7_amd64.deb: Connection failed [IP: 91.189.91.26 80]
                                         
hz@hz:~/ws/other/DECAF/decaf$ dpkg -s libsdl1.2-dev 
dpkg-query: package 'libsdl1.2-dev' is not installed and no information is available
Use dpkg --info (= dpkg-deb --info) to examine archive files,
and dpkg --contents (= dpkg-deb --contents) to list their contents.
hz@hz:~/ws/other/DECAF/decaf$ 

```

#### Step 2
Clone the source code from github and set the environment.
```shell
git clone https://github.com/sycurelab/DECAF
cd DECAF/decaf
export DECAF_PATH=`pwd`
```

#### Step 3

Configure the sleuthkit library.侦探工具包

```shell
cd ${DECAF_PATH}/shared/sleuthkit
rm ./config/ltmain.sh
libtoolize
autoconf
./configure
make
```

#### Step 4

Build decaf and choose which features enabled by uncomment  `--enable-tcg-taint`, `--enable-tcg-ir-log` as `./configure` parameters. You can also add `--prefix` parameters to indicate which path DECAF will be  installed.

```shell
cd ${DECAF_PATH}
./configure # parameters
make #-j8 <- you can uncomment this parameter to speed up the complier
```

DECAF has three basic settings about TCG tainting, VMI, TCG IR logging. You can enable/disable at the configuration step. By default, VMI is enabled and TCG taiting and TCG IR logging is disabled.

Here are the parameters you can use to enable/disabled them:

| Feature       | Flags               | Default  |
| ------------- | ------------------- | -------- |
| TCG tainting  | --enable-tcg-taint  | Disabled |
| TCG IR logger | --enable-tcg-ir-log | Disabled |
| VMI           | --disable-vmi       | Enabled  |

#### Step 5

Build plugins

```shell
cd ${DEVAF_PATH}/plugins/[PLUGINS_NAME]
./configure
make
```