# Starting a TypeScript React-native Project

### Choose a operation system

- [Pure Ubuntu](#Pure Ubuntu)
  - [Set up development environment](#Set up development environment)
  - [Start new project](#Start new project)
- [Windows](#Windows)
- [Remarks](#Remarks)

---

### Pure Ubuntu

#### Set up development environment

0. Install Yarn, a new package manager

   > ```bash
   > $ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
   > $ echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
   > $ sudo apt-get update && sudo apt-get install yarn
   > ```

1. Reference: https://facebook.github.io/react-native/docs/getting-started.html

   > choose "Building Projects with Native Code" & "Linux" & "Android"
   >
   > FOLLOW THE INSTRUCTIONS TO THE LETTER

2. Install node.js (if you haven't already) (https://nodejs.org/en/download/package-manager/)

   > ```bash
   > $ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
   > $ sudo apt-get install -y nodejs
   > ```

3. Install React-native client

   > ```bash
   > $ yarn install -g react-native-cli
   > ```

4. Install Java Development Kit (oracle JDK) (JDK8 is the safest)

   > ```bash
   > $ sudo add-apt-repository ppa:webupd8team/java
   > 
   > `bla bla bla, press ENTER`
   > ENTER
   > 
   > $ sudo apt update
   > $ sudo apt install oracle-java8-installer
   > ```
   >
   > #### Setting the JAVA_HOME environment variable
   >
   > ```bash
   > $ sudo update-alternatives --config java
   > ` copy the path of the version you want to use`
   > $ sudo nano /etc/environment
   > ` add this line to the end`
   > JAVA_HOME="{copied_path}"
   > 
   > ` now update and check`
   > $ source /etc/environment
   > $ echo $JAVA_HOME
   > ```

5. Install Android Studio (https://developer.android.com/studio/)

   > "custom" set up > check these 3 boxes: 
   >
   > - Android SDK
   >
   > - Android SDK platform
   >
   > - Android Virtual Device

6. Install Android SDK

   > require "Android 8.0 (Oreo)" in particular
   >
   > make sure these 2 are checked
   >
   > - Android SDK Platform 26
   >
   > - Google APIs Intel x86 Atom_64 System Image

7. Configure ANDROID_HOME environment variable

   > ```bash
   > $ sudo nano ~/.bash_profile
   > `add these`
   > export ANDROID_HOME=$HOME/Android/Sdk
   > export PATH=$PATH:$ANDROID_HOME/tools
   > export PATH=$PATH:$ANDROID_HOME/tools/bin
   > export PATH=$PATH:$ANDROID_HOME/platform-tools
   > export PATH=$PATH:$ANDROID_HOME/emulator
   > 
   > $ source ~/.bash_profile
   > ```

8. Install TypeScript

   > ```bash
   > $ yarn add --dev react-native-typescript-transformer typescript
   > 
   > ` and follow other instructions on 
   > https://github.com/ds300/react-native-TypeScript-transformer `
   > ```

---

#### Start new Project

1. Prepare a device

   a. plug a physical device, turn on debug mode

   > https://facebook.github.io/react-native/docs/running-on-device
   >
   > Enable Debugging over USB
   >
   > Establish Connection
   >
   > Connect To Development Server
   >
   > Run Your App

   b. open android studio, run AVD manager, config/run an AVD

   	> again , follow the instructions for AVD settings
   	>
   	> also enable hot-reload

2. Create project with React-native client

   > ```bash
   > $ react-native init yourProject --version="0.54.0"
   > ` wait for a bit`
   > $ cd yourProject
   > $ react-native run-android
   > ```

---

### Windows

0. Use PowerShell (admin)

1. Install Chocolatey, package manager for Windows (https://chocolatey.org/)

2. Install Node 8.x & Python2 & JDK8

   > choco install -y nodejs.install python2 jdk8

3. Install React Native Client

   > npm install -g react-native-cli

4. Install Android Studio (https://facebook.github.io/react-native/docs/getting-started.html)

   > Custom setup with
   >
   > - `Android SDK`
   > - `Android SDK Platform`
   > - `Performance (Intel ® HAXM)`
   > - `Android Virtual Device`

5. Install Android SDK

   > SDK Platforms
   >
   > > install Android 8.0 (Oreo) with
   > >
   > > - `Android SDK Platform 26`
   > > - `Google APIs Intel x86 Atom_64 System Image`
   >
   > SDK Tools
   >
   > > Android SDK Build-Tools 26.0.3

6. Configure ANDROID_HOME

   > control panel > system and security > change settings > advanced > env. var. > new
   >
   > name: ANDROID_HOME
   >
   > value: c:\Users\YOUR_USERNAME\AppData\Local\Android\Sdk

---

#### Start new Project

probably same as ubuntu



---

## Remarks

Since RN is not that stable , this guide is written only to provide a way to install RN and run the demo-app without any error. Similar problems may occur after installing other libraries or updating the RN / metro / gradle etc.