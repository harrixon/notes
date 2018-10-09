# Clean Install Ubuntu 18.04 LTS

### Indexes

1. [Pre-installation](#Pre-installation)
2. [Installation](#Installation)
3. [After-installation](#After-installation)
   1. [Install necessary drivers](#Install necessary drivers)
   2. [Set up dev env](#Set up dev env)



---

### Pre-installation

1. Download ISO file from <a href="https://www.ubuntu.com/download/desktop">offical site</a>

2. Check the sum/key for file integrity

3. Burn the ISO onto a DVD

   > bootable usb is not recommanded

---

### Installation

* Not a dual-boot installation

1. Connect only the HD you want to install Ubuntu

2. Boot with the DVD you have burnt

3. Follow the instructions, auto install updates and drivers

   > boot loader might be installed on some other HD if they were not disconnected

---

### After-installation

#### Install necessary drivers

1. NVIDIA

   ```bash
   $ ubuntu-drivers devices
   == /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
   modalias : pci:v000010DEd000013C2sv00001462sd00003161bc03sc00i00
   vendor   : NVIDIA Corporation
   model    : GM204 [GeForce GTX 970]
   driver   : nvidia-driver-390 - distro non-free recommended
   driver   : xserver-xorg-video-nouveau - distro free builtin
   
   $ sudo ubuntu-drivers autoinstall
   # or 
   sudo apt install nvidia-driver-390
   ```


#### Set up dev env

0. useful packages

   ```bash
   # 1. curl
   $ sudo apt-get install curl
   
   # 2. build-essential
   $ sudo apt-get install -y build-essential
   
   # 3. make
   $ sudo apt-get install make
   
   # 4. tcl
   $ sudo apt install tcl
   ```

1. nodejs and npm

   ```bash
   # https://nodejs.org/en/
   curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

2. REDIS

   ```bash
   # https://redis.io/download
   # Download latest stable version
   $ wget http://download.redis.io/releases/redis-4.0.11.tar.gz
   $ tar xzf redis-4.0.11.tar.gz
   $ cd redis-4.0.11
   $ make
   $ sudo make install
   
   # check by starting server
   $ src/redis-server
   ```

3. postgresql and pgadmin

   ```bash
   # PSQL
   # https://www.postgresql.org/download/linux/ubuntu/
   # https://wiki.postgresql.org/wiki/Apt
   # select version of ubuntu used
   $ sudo touch /etc/apt/sources.list.d/pgdg.list
   
   # Add the following line and save
   $ sudo nano /etc/apt/sources.list.d/pgdg.list
   deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main
   
   # Import the repository signing key, and update the package lists
   $ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
   $ sudo apt-get update
   $ sudo apt-get upgrade
   $ sudo apt-get install postgresql-10 pgadmin4 postgresql-contrib
   ```

4.  yarn

   ```bash
   # https://yarnpkg.com/lang/en/docs/install/#debian-stable
   $ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
   $ echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
   $ sudo apt-get update && sudo apt-get install yarn
   
   # check with
   $ yarn --version
   ```

5.  React-native

   #### react-native-cli

   ```bash
   # https://facebook.github.io/react-native/docs/getting-started.html
   # Building Projects with Native Code
   # react-native-cli
   $ npm install -g react-native-cli
   ```

   #### Java Development Kit (JDK)

   ```bash
   # Java Development Kit (JDK)
   # https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
   # Download the right file, unzip it
   # ???
   
   # or
   $ sudo add-apt-repository ppa:webupd8team/java
   $ sudo apt update
   $ apt search oracle-java
   $ sudo apt install oracle-java8-set-default
   
   # check with
   $ java --version
   ```

   #### Android Studio

   ```bash
   # Install Android Studio
   # https://developer.android.com/studio/
   # download and install from above link
   
   # make sure these 3 are checked
       Android SDK
       Android SDK Platform
       Android Virtual Device
   
   # Install the Android SDK
   	Android 8.0 (Oreo) for react-native code
   # Preferences > Appearance & Behavior > System Settings > Android SDK
   # make sure these are checked
       Android SDK Platform 26
       Google APIs Intel x86 Atom_64 System Image
   
   # Configure the ANDROID_HOME environment variable
   $ sudo nano .bashrc
   # add these and save
   export ANDROID_HOME=$HOME/Android/Sdk
   export PATH=$PATH:$ANDROID_HOME/tools
   export PATH=$PATH:$ANDROID_HOME/tools/bin
   export PATH=$PATH:$ANDROID_HOME/platform-tools
   export PATH=$PATH:$ANDROID_HOME/emulator
   
   # reload by
   $ source .bashrc
   # verify by
   $ echo $PATH
   ```


