# Linux-on-Android
How to set up a Java development environment on an Android phone

## Revised installation

* Install **Jump Desktop** (VNC client)
* Install **CodeBoard** (on-screen keyboard)
* Install **UserLAnd** from Play Store
* Launch **UserLAnd** and open Arch Linux (terminal interface)
* Update package registry:
  * `sudo pacman -Syu`
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
  * `sudo pacman -S zip unzip`
  * `sudo pacman -S xarchiver`
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
* Install developer tools:
  * `sudo pacman -S base-devel glibc`
* Install `fakeroot-tcp`:
  * Add Arch4Edu repository: 
    * https://github.com/arch4edu/arch4edu/wiki/Add-arch4edu-to-your-Archlinux
  * `sudo pacman -Sy fakeroot-tcp`
* Install `yay` package manager:
  * NOTE: This package must be built locally, because no one appears to have published it for the `aarch64` architecture.
  * https://github.com/Jguer/yay#source
* JDK11, JDK8
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
* Install **TigerVNC**:
  * `sudo pacman -S tigervnc`
  * `nano .vnc/config`
  ```
    session=xfce4
    geometry=1920x1080
    localhost
    alwaysshared
  ```
* Install **XFCE4**:
  * `sudo pacman -S xfce4 xfce4-goodies`
  * may not need: `pacman -S xfce4-session`
* Install **Firefox** and its driver:
  * `sudo pacman -S firefox geckodriver`
* Start VNC server:
  * `vncserver :1`
* Access XFCE4 session with Jump Desktop
* Open Firefox and download **Eclipse Installer**
* Extract Eclipse Installer and run it to install **Eclipse** 
  * Install the TestNG plug-in, but not the optional Maven integration
* Install **Visual Studio Code** via `yay`:
  * `su scoba`
  * `yay -S visual-studio-code-bin`


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
