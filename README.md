# Linux-on-Android
How to set up a Java development environment on an Android phone

## Revised installation

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
