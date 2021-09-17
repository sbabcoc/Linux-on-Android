# Linux-on-Android
How to set up a Java development environment on an Android phone

Here's my installation (although not exactly sequential):
* Termux (terminal emulator)
* Arch Linux ARM (4.14.141+ aarch64)
* Xfce4 (desktop environment)
* Jump Desktop (VNC client)
* CodeBoard (on-screen keyboard)
* Eclipse (Java IDE)
* Visual Studio Code
* Git
* Man
* PRoot
* OpenSSH
* GnuPG
* Maven
* Gradle
* SdkMan
* JDK7, JDK8, JDK11
* 'htop' (process viewer)
* I build and installed 'fakeroot-tcp'
* ... which allows me to build and install other packaged
* ... Jed (editor)
* ... 'yay' (package manager)
* ... 'gpm' (console mouse driver)
* Mercurial (version control client)
* ... with which I downloaded the source for OpenJDK 7
* ... so that I could build a JAR from the SunEC source files
* ... which I dropped into the jre/lib/ext folder of java-7-openjdk
* I duplicated the 'libsunec.so' native library from JDK8 to JDK7, which hasn't blown up yet.

NOTE: The ability to an drop unsigned encryption library into the JRE without complaint was more than a bit surprising. Although this required file-system access, this seems like a bit of a security vulnerability to me.

## Hardware/Accessories
* LG Velvet (LM-G900TM)
  * Android 10
  * MediaTek Dimensity 1000C MT6883 2,00 GHz [Number of cores: 8]
  * Internal memory: 128 GB
  * RAM memory: 6 GB
* Pixio PX160 Portable Monitor
* Folding Bluethooth keyboard/trackpad
* Zippered hard-sided case for keyboard, cables, and power supply
