# Linux-on-Android
How to set up a Java development environment on an Android phone

Here's my installation (although not exactly sequential):
* Termux (terminal emulator)
  * Latest, installed from GitHub
* Arch Linux with Xfce4
  * installed in PRoot via 'Andronix'
* Create new user:
  * `useradd -m scoba`
  * `passwd scoba`
  * `sudo visudo`
  * Add this line after `root`:
    * `scoba ALL=(ALL:ALL) ALL`
  * `<esc>:wq`
* Switch to user 'scoba':
  * `sudo su - scoba`
* Install 'man':
  * `sudo pacman -S man`
* Install Git:
  * `sudo pacman -S git`
  * `git config --global user.email "john.smith@email.com"`
  * `git config --global user.name "John Smith"`
  * `git config --global credential.helper store`
  * Perform a "push" to GitHub, entering username and PAT (personal access token)
* Install developer tools:
  * `sudo pacman -S base-devel`
* Install 'fakeroot-tcp':
  * Add Arch4Edu repository: 
    * https://github.com/arch4edu/arch4edu/wiki/Add-arch4edu-to-your-Archlinux
  * `sudo pacman -S fakeroot-tcp`
* Install 'yay' package manager from this repository
* Install 'jed' editor from this repository
* Jump Desktop (VNC client)
* CodeBoard (on-screen keyboard)
* JDK11, JDK8, JDK7
  * `sudo pacman -S jdk11-openjdk`
  * `sudo pacman -S jdk8-openjdk`
  * `sudo pacman -S jdk7-openjdk`
  * Install **SubEC** components from this repository into JDK7
* Install Eclipse (Java IDE) via installer in Xfce4
  * Download installer, extract, and launch
* Visual Studio Code:
  * `git clone https://aur.archlinux.org/visual-studio-code-bin.git`
  * `makepkg -si`
* Gradle
* SdkMan
* 'htop' (process viewer)
* I built and installed 'fakeroot-tcp'
* ... (NOTE: fakeroot is require by makepkg. Originally, this utility required SYSV, which Termux doesn't support. Newer releases may have fixed this issue, so building a specific TCP version may no longer be necessary.)
* ... which allows me to build and install other packages
* ... Jed (editor)
* ... 'yay' (package manager)
* ... 'gpm' (console mouse driver) - NOTE: There appears to be no "mouse" device for this driver to interact with.
* Mercurial (version control client)
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
  * `sudo pacman -S llvm`
  * `sudo pacman -S llvm-libs`
  * `sudo pacman -S clang`
  * `sudo pacman -S libffi`
  * ... (others will be encountered building Mozilla Central)
* Clone the Mozilla Central repository:
  * `git clone hg::https://hg.mozilla.org/mozilla-central`

## Hardware/Accessories
* LG Velvet (LM-G900TM)
  * Android 10
  * MediaTek Dimensity 1000C MT6883 2,00 GHz [Number of cores: 8]
  * Internal memory: 128 GB
  * RAM memory: 6 GB
* Pixio PX160 Portable Monitor
* Folding Bluethooth keyboard/trackpad
* Zippered hard-sided case for keyboard, cables, and power supply
