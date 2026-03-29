# Linux-on-Android
How to set up a Java development environment on an Android phone

## Revised installation (Mar 28, 2026)

* Install **Unexpected Keyboard** (on-screen keyboard)
* Install **Termux** from [F-Droid](https://wiki.termux.com/wiki/Installing_from_F-Droid)
* Install **Debian**  
  1. **Update and Install Proot-Distro**  
     Open Termux and run the following commands to ensure everything is current and to install the tool that manages Linux distributions:  
     ``` bash
     pkg update && pkg upgrade -y
     pkg install proot-distro
     ```
  2. **Install Debian**  
     Install the Debian distribution using the following command:  
     ``bash
     proot-distro install debian
     ```
  3. **Login and Use Debian**  
     Log in to the newly installed Debian container:  
     ```bash
     proot-distro login debian
     ```
     _You are now in a Debian shell, appearing as root._
  4. **Initial Setup (Inside Debian)**  
     It is highly recommended to update packages and add a regular user for security:  
     ```bash
     apt update && apt upgrade -y
     apt install sudo nano adduser
     # Add a new user (replace 'username' with your preferred name)
     adduser username
     # Grant sudo permissions
     echo 'username ALL=(ALL:ALL) ALL' > /etc/sudoers.d/username
     chmod 0440 /etc/sudoers.d/username
     ```
(Optional) Install a Desktop Environment (XFCE4) 
If you need a graphical interface, you can install XFCE4 within the proot environment: 
Install XFCE4: apt install xfce4 xfce4-goodies dbus-x11 -y
To run it, you will need to install Termux:X11 and set up a VNC server or X11 forwarding. 
Reddit
Reddit
 +3

* Install **Termux:X11**:  
  To install Termux:X11, you need to install two separate components: the **Termux:X11** Android app (APK) and the companion Termux package. The installation requires Android 8 or later.
  * **Step 1: Install the Termux:X11 Android App (APK)**  
    The app is not available on the Google Play Store (which has outdated Termux versions). Instead, download the APK from the official GitHub repository's nightly releases. 
    * Go to the [Termux:X11 releases](https://github.com/termux/termux-x11/releases) page on **GitHub**.
    * Download the `app-$ARCHITECTURE-debug.apk` file that matches your device's CPU architecture.
      * If you are unsure of your device's architecture, you can download the app-universal-debug.apk file, which is compatible with most devices but uses slightly more storage.
      * Install the downloaded APK file. You may need to grant permission to install apps from unknown sources in your device's settings.
      * 
  * **Step 2: Install the companion package in the Termux terminal**  
    Once the APK is installed, open the main Termux app and run the following commands to enable the X11 repository and install the necessary package:
    * Update/upgrade Termux:
      ```bash
      pkg update && pkg upgrade -y
      ```
    * Enable the X11 repository:  
      ```bash
      pkg install x11-repo
      ```
    * Install the companion package:  
      ```bash
      pkg install termux-x11-nightly
      ```

  
      
  * **Step 4: Run Termux:X11**  
    You are now ready to use Termux:X11.
    1. First, launch the Termux:X11 Android app from your app drawer. A blank screen will appear, indicating the server is running.
    2. Switch back to the Termux terminal.
    3. Install the `nano` editor:
       ```bash
       pkg install nano
       ```
    4.
      and run the following command to start your session (using XFCE as an example):
      ```bash
      termux-x11 :1 -xstartup "dbus-launch --exit-with-session xfce4-session"
      ```
This command starts the X server on display :1 and launches the XFCE session.
Switch back to the Termux:X11 app to see your running graphical environment. 
Troubleshooting Notes:
If you only see a black screen, you might need to pass the -legacy-drawing option to the startup command: termux-x11 :1 -legacy-drawing -xstartup "xfce4-session".
You can set the TERMUX_X11_XSTARTUP environment variable in your ~/.bashrc file to avoid typing the full -xstartup command every time.

## Revised installation (Jun 26, 2023)

* Install **CodeBoard** (on-screen keyboard)
* Install **UserLAnd** from Play Store
* Launch **UserLAnd** and open Arch Linux (graphical interface)
* Set random placement option of TWM:
  * `cp /usr/share/X11/twm/system.twmrc .twmrc`
  * `nano .twmrc`
  * Move insertion point down to the blank line following the first set of options
  * Add line: `RandomPlacement`
  * Type `<ctrl-x>y<enter>` to save changes and exit
  * **NOTE**: This change eliminates the step of "placing" applications at launch
* Update package registry:
  * `sudo pacman -Syu`
* Install developer tools:
  * `sudo pacman -S base-devel glibc`
* Install `man`:
  * `sudo pacman -S man`
* Install `git`:
  * `sudo pacman -S git`
  * `git config --global user.email "john.smith@email.com"`
  * `git config --global user.name "John Smith"`
  * `git config --global credential.helper store`
  * `git config --global fetch.prune true`
  * Perform a "push" to GitHub, entering username and PAT (personal access token)
* Install archive utilities:
  * `sudo pacman -S zip unzip xarchiver`
* Install `yay` package manager:
  * NOTE: This package must be built locally, because no one appears to have published it for the `aarch64` architecture.
  * https://github.com/Jguer/yay#source
* Install **Visual Studio Code** via `yay`:
  * `yay -S visual-studio-code-bin`
  * Create `code-flags.conf` file:
    * `nano .config/code-flags.conf`
    * Add line: `--no-sandbox`
    * Add line: `--user-data-dir=/home/userland/.vscode-data`
    * Type `<ctrl-x>y<enter>` to save changes and exit
    * **NOTE**: This is needed because **UserLAnd** doesn't maintain correct ownership/permissions of file:  
`/opt/visual-studio-code/chrome-sandbox`
* Set editor, difftool and mergetool for `git`:
  * `git config --global core.editor nano`
  * `git config --global -e`
```
[core]
  editor = code --wait
[diff]
  tool = vscode
[difftool "vscode"]
  cmd = code --wait --diff $LOCAL $REMOTE
[merge]
  tool = vscode
[mergetool "vscode"]
  cmd = code --wait $MERGED
```
* Install SdkMan:
  * `curl -s "https://get.sdkman.io" | bash`
* Install Maven:
  * `sudo pacman -S maven`
* Install Gradle:
  * `sdk install gradle`
* Install/create Maven/Gradle/GitHub artifacts:
  * ~/.m2
    * settings.xml
  * ~/.gnupg
    * secring.gpg
    * pubring.gpg
  * ~/.gradle/
    * gradle.properties
* JDK11, JDK8:
  * `sudo pacman -S jdk11-openjdk`
  * `sudo pacman -S jdk8-openjdk`
* Add exports for **JDK8_HOME** and **JDK11_HOME** to `.bashrc`:
  * `~/.bashrc`
  ```
    export JDK8_HOME=/urs/lib/jvm/java-8-openjdk
    export JDK11_HOME=/urs/lib/jvm/java-11-openjdk
  ```
  
* Create Maven toolchains configuration:
  * `~/.m2/toolchains.xml`
  ```
    <?xml version="1.0" encoding="UTF8"?>
    <toolchains>
      <!-- JDK toolchains -->
      <toolchain>
        <type>jdk</type>
        <provides>
          <version>8</version>
        </provides>
        <configuration>
          <jdkHome>/usr/lib/jvm/java-8-openjdk</jdkHome>
        </configuration>
      </toolchain>
      <toolchain>
        <type>jdk</type>
        <provides>
          <version>11</version>
        </provides>
        <configuration>
          <jdkHome>/usr/lib/jvm/java-11-openjdk</jdkHome>
        </configuration>
      </toolchain>
    </toolchains>
  ```

* Install `wget`:
  * `sudo pacman -S wget`
* Install Bazelisk:
  * `wget https://github.com/bazelbuild/bazelisk/releases/download/v1.17.0/bazelisk-linux-arm64`
  * `mv bazelisk-linux-arm64 bazelisk`
  * `chmod +x bazelisk`
  * `mv bazelisk /usr/sbin/bazelisk`
* Install `buildifier`:
  * `wget https://github.com/bazelbuild/buildtools/releases/download/v6.1.2/buildifier-linux-arm64`
  * `mv buildifier-linux-arm64 buildifier`
  * `chmod +x buildifier`
  * `mv buildifier /usr/sbin/buildifier`
* Install `jed` editor:
  * NOTE: This package must be built locally, because no one appears to have published it for the `aarch64` architecture.
  * `sudo pacman -S slang libxt`
  * `git clone https://github.com/jedsoft/jed.git`
  * `cd jed`
  * `./configure --prefix=/usr/local`
  * `make clean`
  * `make`
  * `make xjed`
  * `make install`
* Install **Firefox** and its driver:
  * `sudo pacman -S firefox geckodriver`
* Disable Firefox sandboxing in `.bashrc`:
  * `~/.bashrc`
  ```
    export MOZ_FAKE_NO_SANDBOX=1
  ```
* Open Firefox
  * `dbus-run-session -- firefox`


* Clone the Selenium Foundation repository:
  * `git clone https://github.com/sbabcoc/Selenium-Foundation.git`
  
## Hardware/Accessories

* LG Velvet (LM-G900TM)
  * Android 11
  * MediaTek Dimensity 1000C MT6883 2,00 GHz [Number of cores: 8]
  * Internal memory: 128 GB
  * RAM memory: 6 GB
  * 1080 x 2460
* Pixio PX160 Portable Monitor
  * 1920 x 1080
* Folding Bluethooth keyboard/trackpad
* Zippered hard-sided case for keyboard, cables, and power supply

## Stuff you probably won't need

#### LLVM and CLang
  * `sudo pacman -S llvm`
  * `pacman -S llvm-libs`
  * `pacman -S clang`
  * `pacman -S python-pip`
  * `pacman -S cbindgen`
  * `pacman -S nodejs npm`
  
#### To install JDK7 with SunEC support:
* `pacman -S jdk7-openjdk`
* Install **SubEC** components from **Linux-on-Android** repository into JDK7

#### To build the SunEC JAR:
* Install Mercurial (version control client):
  * `sudo -u scoba yay -S mercurial`
* ... with which I downloaded the source for OpenJDK 7
* ... so that I could build a JAR from the SunEC source files
* ... which I dropped into the jre/lib/ext folder of java-7-openjdk
* I duplicated the 'libsunec.so' native library from JDK8 to JDK7, which hasn't blown up yet.

NOTE: The ability to drop an unsigned encryption library into the JRE without complaint was more than a bit surprising. Although this required file-system access, this seems like a bit of a security vulnerability to me.

#### To clone the `geckodriver` repo, you need the 'git-cinnabar' extension:
* Clone and build `git-cinnabar` extension:
  * `git clone https://github.com/glandium/git-cinnabar.git`
  * `cd git-cinnabar`
  * `make`
  * Add `git-cinnabar` folder to the **PATH**
