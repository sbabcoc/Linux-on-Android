# Linux-on-Android
How to set up a Java development environment on an Android phone

Here's my installation (although not exactly sequential):
* Install **Jump Desktop** (VNC client)
* Install **CodeBoard** (on-screen keyboard)
* Install **Termux** (terminal emulator):
  * Latest, installed from F-Droid
  * `pkg update`
  * `pkg upgrade`
* Install 'proot-distro' Linux distribution manager
  * `pkg install proot-distro`
* Install **Arch Linux**:
  * `proot-distro install archlinux`

To launch **Arch Linux**:
* `proot-distro login archlinux`

* Create new user:
  * `useradd -m scoba`
  * `passwd scoba`
* Update package registry:
  * `pkg -Syu`
* Install `sudo`:
  * `pkg install sudo`
* Add user to "sudoers":
  * `sudo visudo`
  * Add this line after `root`:
    * `scoba ALL=(ALL:ALL) ALL`
  * `<esc>:wq`
* Install `man`:
  * `pacman -S man`
* Install `git`:
  * `pacman -S git`
  * `git config --global user.email "john.smith@email.com"`
  * `git config --global user.name "John Smith"`
  * `git config --global credential.helper store`
  * `git config --global fetch.prune true`
  * Perform a "push" to GitHub, entering username and PAT (personal access token)
* Install developer tools:
  * `pacman -S base-devel`
* Install `fakeroot-tcp`:
  * Add Arch4Edu repository: 
    * https://github.com/arch4edu/arch4edu/wiki/Add-arch4edu-to-your-Archlinux
  * `pacman -S fakeroot-tcp`
* Install `yay` package manager:
  * NOTE: This package must be built locally, because no one appears to have published it for the `aarch64` architecture.
  * https://github.com/Jguer/yay#source
* Install `jed` editor:
  * NOTE: This package must be built locally, because no one appears to have published it for the `aarch64` architecture.
  * `pacman -S slang libxt`
  * `git clone https://github.com/jedsoft/jed.git`
  * `cd jed`
  * `./configure --prefix=/usr/local`
  * `make clean`
  * `make`
  * `make xjed`
  * `make install`
* Install **TigerVNC**:
  * `pacman -S tigervnc`
* Install **XFCE4**:
  * `pacman -S xfce4 xfce4-godies`

[prior installation - some may be retained]
* JDK11, JDK8, JDK7
  * `pacman -S jdk11-openjdk`
  * `pacman -S jdk8-openjdk`
  * `pacman -S jdk7-openjdk`
  * Install **SubEC** components from this repository into JDK7
* Install Eclipse (Java IDE) via installer in Xfce4
  * Download installer, extract, and launch
* Install archive utilities:
  * `pacman -S zip unzip`
  * `pacman -S xarchiver`
* Install SdkMan:
  * `curl -s "https://get.sdkman.io" | bash`
* Install Gradle:
  * `sdk install gradle`
* Install Mercurial (version control client):
  * `sudo -u scoba yay -S mercurial`
* ... with which I downloaded the source for OpenJDK 7
* ... so that I could build a JAR from the SunEC source files
* ... which I dropped into the jre/lib/ext folder of java-7-openjdk
* I duplicated the 'libsunec.so' native library from JDK8 to JDK7, which hasn't blown up yet.

NOTE: The ability to drop an unsigned encryption library into the JRE without complaint was more than a bit surprising. Although this required file-system access, this seems like a bit of a security vulnerability to me.

* Clone and build `git-cinnabar` extension:
  * `git clone https://github.com/glandium/git-cinnabar.git`
  * `cd git-cinnabar`
  * `make`
  * Add `git-cinnabar` folder to the **PATH**
* Install LLVM and CLang
  * `pacman -S llvm`
  * `pacman -S llvm-libs`
  * `pacman -S clang`
  * `pacman -S python-pip`
  * `pacman -S cbindgen`
  * `pacman -S nodejs npm`
* Clone the Selenium Foundation repository:
  * `git clone https://github.com/sbabcoc/Selenium-Foundation.git`
* Install GeckoDriver:
  * `pacman -S geckodriver`

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
