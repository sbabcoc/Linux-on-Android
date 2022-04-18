# Linux-on-Android
How to set up a Java development environment on an Android phone

Here's my installation (although not exactly sequential):
* Termux (terminal emulator)
* Arch Linux ARM (4.14.141+ aarch64)
  * installed via 'setupTermuxArch'
* Create new user:
  * `useradd -m scoba`
  * `passwd scoba`
  * `sudo visudo`
  * Add this line after `root`:
    * `scoba ALL=(ALL:ALL) ALL`
  * `<esc>:wq`
* Switch to user 'scoba':
  * `sudo su - scoba`
* Install Git:
  * `sudo pacman -S git`
  * `git config --global user.email "john.smith@email.com"`
  * `git config --global user.name "John Smith"`
  * `git config --global credential.helper store`
* Install GCC:
  * `sudo pacman -S gcc`
* Install scripts (includes 'chroot')
  * `sudo pacman -S arch-install-scripts`
* Xfce4 (desktop environment)
  * `sudo pacman -S xfce4`
* Jump Desktop (VNC client)
* CodeBoard (on-screen keyboard)
* Eclipse (Java IDE)
* Visual Studio Code (installed via 'yay')
* Man
* PRoot (installed by 'setupTermuxArch')
* OpenSSH
* GnuPG
* Maven
* Gradle
* SdkMan
* JDK7, JDK8, JDK11
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
  * `pacman -S llvm`
  * `pacman -S llvm-libs`
  * `pacman -S clang`
  * `pacman -S libffi`
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
