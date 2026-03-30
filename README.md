
# Linux-on-Android
How to set up a Java development environment on an Android phone

### Step 1: Initial Installation
* Install **Unexpected Keyboard** (on-screen keyboard)
* Install **Termux** from [F-Droid](https://wiki.termux.com/wiki/Installing_from_F-Droid)
* Install **Termux:X11**:  
  To install Termux:X11, you need to install two separate components: the **Termux:X11** Android app (APK) and the companion Termux package. The installation requires Android 8 or later.
  1. **Install the Termux:X11 Android App (APK)**  
    The app is not available on the Google Play Store (which has outdated Termux versions). Instead, download the APK from the official GitHub repository's nightly releases.
    * Go to the [Termux:X11 releases](https://github.com/termux/termux-x11/releases) page on **GitHub**.
    * Download the `app-$ARCHITECTURE-debug.apk` file that matches your device's CPU architecture.
      * If you are unsure of your device's architecture, you can download the app-universal-debug.apk file, which is compatible with most devices but uses slightly more storage.
      * Install the downloaded APK file. You may need to grant permission to install apps from unknown sources in your device's settings.
  2. **Install the companion package in the Termux terminal**  
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

### Step 2: Install and Configure Debian
  * Update and Install **Proot-Distro**:  
    Open Termux and run the following commands to ensure everything is current and to install the tool that manages Linux distributions:
    ``` bash
    pkg update && pkg upgrade -y
    pkg install proot-distro
    ```
  * Install **Debian**:  
    Install the Debian distribution using the following command:
    ```bash
    proot-distro install debian
    ```
  * Login and Use **Debian**:  
    Log in to the newly installed Debian container:
    ```bash
    proot-distro login debian
    ```
    _You are now in a Debian shell, appearing as root._
  * Initial Setup (Inside **Debian**):  
    It is highly recommended to update packages and add a regular user for security:
    ```bash
    echo "nameserver 8.8.8.8" > /etc/resolv.conf
    apt update && apt upgrade -y
    apt install sudo nano adduser
    # Add a new user (replace 'scoba' with your preferred name)
    adduser scoba
    # Grant sudo permissions
    echo 'scoba ALL=(ALL:ALL) ALL' > /etc/sudoers.d/scoba
    chmod 0440 /etc/sudoers.d/scoba
    ```
  * Install **XFCE4** Desktop Environment:  
    Install XFCE4 within the proot environment:
    ```bash
    apt install xfce4 xfce4-goodies dbus-x11 -y
    ```
  * Add Configuration for  `root` Account:  
    Append configuration to `.bashrc`:
    ```bash
    cat <<EOF >> .bashrc
    export DISPLAY=:0
    export XDG_CURRENT_DESKTOP=XFCE
    export XDG_RUNTIME_DIR=/tmp/runtime-root
    if [ ! -d "$XDG_RUNTIME_DIR" ]; then
        mkdir -p "$XDG_RUNTIME_DIR"
        chmod 700 "$XDG_RUNTIME_DIR"
    fi
    
    # Disable content sandbox (required in proot)
    export MOZ_DISABLE_CONTENT_SANDBOX=1
    EOF
    ```
  * Switch to Regular User Account:
    ```bash
    su - scoba # replace 'scoba' with regular user name
    ```
  * Add Configuration for  Regular Account:
    Use `nano` to add configuration to `.bashrc`:
    ```bash
    # If not running interactively, don't do anything
    case $- in
        *i*) ;;
          *) return;;
    esac
    
    export LANG=en_US.UTF-8
    export LANGUAGE=en_US.UTF-8
    export LC_ALL=en_US.UTF-8
    
    export PATH="$PATH:$HOME/.local/bin"
    export SE_MANAGER_PATH="$HOME/code/selenium/rust/target/debug/selenium-manager"
    
    export DISPLAY=:0
    export XDG_CURRENT_DESKTOP=XFCE
    export XDG_RUNTIME_DIR=/tmp/runtime-scoba
    if [ ! -d "$XDG_RUNTIME_DIR" ]; then
        mkdir -p "$XDG_RUNTIME_DIR"
        chmod 700 "$XDG_RUNTIME_DIR"
    fi
    
    # Disable content sandbox (required in proot)
    export MOZ_DISABLE_CONTENT_SANDBOX=1
    ```
  * Create Utility Scripts:
    1. Create utility scripts directory:
       ```bash
       mkdir -p ~/.local/bin
       ```
    2. Create script to launch **XFCE4**:
       ```bash
       cat <<EOF > ~/.local/bin/start-xfce
       #!/bin/bash
       export DISPLAY=:0
       unset WAYLAND_DISPLAY
       
       # Kill any leftover XFCE processes
       pkill -9 -f xfce4-session 2>/dev/null
       pkill -9 -f xfwm4 2>/dev/null
       pkill -9 -f xfdesktop 2>/dev/null
       pkill -9 -f xfce4-panel 2>/dev/null
       pkill -9 -f thunar 2>/dev/null
       
       # Remove stale sessions
       rm -rf ~/.cache/sessions
       
       # Ensure runtime dir
       export XDG_RUNTIME_DIR=/tmp/runtime-xfce
       if [ ! -d "$XDG_RUNTIME_DIR" ]; then
           mkdir -p "$XDG_RUNTIME_DIR"
           chmod 700 "$XDG_RUNTIME_DIR"
       fi
       EOF
       chmod +x ~/.local/bin/start-xfce
       ```
    3. Create wrapper script for **Visual Studio Code**:
       ```bash
       cat <<EOF > ~/.local/bin/code-proot
       #!/bin/bash
       export DISPLAY=:0
       unset WAYLAND_DISPLAY
       
       export XDG_RUNTIME_DIR=/tmp/runtime-vscode
       if [ ! -d "$XDG_RUNTIME_DIR" ]; then
           mkdir -p "$XDG_RUNTIME_DIR"
           chmod 700 "$XDG_RUNTIME_DIR"
       fi
       
       exec code --no-sandbox --disable-gpu "$@”
       EOF
       chmod +x ~/.local/bin/code-proot
       ```
    4. Create wrapper for **Mozilla Firefox**:
       ```bash
       cat <<EOF > ~/.local/bin/firefox-proot
       #!/bin/bash
       export DISPLAY=:0
       unset WAYLAND_DISPLAY
       
       # Disable content sandbox (required in proot)
       export MOZ_DISABLE_CONTENT_SANDBOX=1
       
       # Force software rendering to suppress GPU warnings
       export LIBGL_ALWAYS_SOFTWARE=1
       export MOZ_X11_EGL=0
       
       # Isolated runtime directory
       export XDG_RUNTIME_DIR=/tmp/runtime-firefox
       if [ ! -d "$XDG_RUNTIME_DIR" ]; then
           mkdir -p "$XDG_RUNTIME_DIR"
           chmod 700 "$XDG_RUNTIME_DIR"
       fi
       
       # --- Launch Firefox ---
       exec firefox "$@”
       EOF
       chmod +x ~/.local/bin/firefox-proot
       ```
  * Install Developer Tools:
    ```bash
    sudo apt update
    sudo apt install build-essential
    sudo apt install git
    sudo apt install maven
    sudo apt install gdb
    sudo apt install autoconf automake
    ```
  * Install **SDKMan!**:
    ```bash
    apt install curl zip unzip -y
    curl -s "https://get.sdkman.io" | bash
    source "$HOME/.sdkman/bin/sdkman-init.sh”
    ```
  * Install **Visual Studio Code**:
    1. Update your package index and install dependencies:
       ```bash
       sudo apt install gnupg2 software-properties-common apt-transport-https
       ```
    2. Import the Microsoft GPG key:
       ```bash
       curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
       ```
    3. Add the Visual Studio Code repository to your system's sources:
       ```bash
       echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
       ```
    4. Update the package cache and install Visual Studio Code:
       ```bash
       sudo apt update
       sudo apt install code
       ```
    5. Create the root folder for **VSCode** configuration:
       ```bash
       mkdir -p ~/.config/Code
       ```
    6. Define `git` configiration:
       ```bash
       cat <<EOF > ~/.gitconfig
       [core]
       	editor = code-proot --no-sandbox --wait
       [diff]
       	tool = vscode
       [difftool "vscode"]
       	cmd = code-proot --no-sandbox --wait --diff $LOCAL $REMOTE
       [merge]
       	tool = vscode
       [mergetool "vscode"]
       	cmd = code-proot --no-sandbox --wait --merge $REMOTE $LOCAL $BASE $MERGED
       [mergetool]
       	keepBackup = false
       [difftool]
       	prompt = false
       [user]
       	email = scoba@hotmail.com
       	name = Scott Babcock
       [credential]
       	helper = store
       EOF
       ```
  * Exit **Debian** and Return to **Termux** Shell:
    ```bash
    exit # from Debian regular user account
    exit # from Debian 'root' user account
    ```
* Assemble **Debian/X11** Bootstrap:
  1. **Create a Termux X11 Configuration Script**:
     ```bash
     cat <<EOF > ~/termux-x11-env.sh
     #!/data/data/com.termux/files/usr/bin/bash
     
     # Start X server if not running
     if ! pgrep -f termux-x11 >/dev/null; then
         termux-x11 :0 >/dev/null 2>&1 &
         sleep 1
     fi
     
     export DISPLAY=:0
     export XDG_RUNTIME_DIR=$TMPDIR
     EOF
     chmod +x ~/termux-x11-env.sh
     ```
  2. **Create a Debian Launch Script**:
     ```bash
     cat <<EOF > ~/start-debian.sh
     #!/data/data/com.termux/files/usr/bin/bash
     source ~/termux-x11-env.sh
     proot-distro login debian --shared-tmp --user scoba
     chmod +x ~/start-debian.sh
     ```

### Step 3: Launch Debian with XFCE Desktop
* Launch the **Termux:X11** Android application. A blank screen will appear, indicating the server is running.
* Switch back to the **Termux** terminal and launch **Debian/X11**:
  ```bash
  ~/start-debian.sh
  ```
  This command starts the **X** server on display :0 and launches the **XFCE** session.
  Switch back to the **Termux:X11** app to see your running graphical environment.

### Step 4: Install Mozilla Firefox
  * Open a Terminal session from the Applications menu
  * Install and register Firefox ESR:
    ```bash
    sudo apt install firefox-esr
    echo "WebBrowser=firefox-proot" >> ~/.config/xfce4/helpers.rc
    mkdir -p ~/.local/share/xfce4/helpers
    code-proot ~/.local/share/xfce4/helpers/firefox-proot.desktop
    ```
  * Paste this helper definition:
    ```ini
    [Desktop Entry]
    Name=Firefox (proot)
    X-XFCE-Category=WebBrowser
    X-XFCE-Commands=/home/scoba/.local/bin/firefox-proot
    X-XFCE-CommandsWithParameter=/home/scoba/.local/bin/firefox-proot "%s"
    Exec=/home/scoba/.local/bin/firefox-proot "%u"
    Terminal=false
    Type=X-XFCE-Helper
    Icon=firefox
    Categories=Network;WebBrowser;
    MimeType=text/html;text/xml;application/xhtml+xml;x-scheme-handler/http;x-scheme-handler/https;
    StartupNotify=true
    ```
  * Save the helper definition
  * Register Firefox in the desktop and MIME databases:
    ```bash
    update-desktop-database ~/.local/share/applications
    xdg-settings set default-web-browser firefox-proot.desktop
    mkdir -p ~/.local/share/mime/packages
    update-mime-database ~/.local/share/mime
    ```
* Install **JDK8**, **JDK11**, **JDK17**, and **JDK21**:
  ```bash
  sdk install java 8.0.482-tem
  sdk install java 11.0.30-tem
  sdk install java 17.0.18-tem
  sdk install java 21.0.10-tem
  ```
* Add **JDK&#42;_HOME** exports to `.bashrc`:
  ```bash
  export JDK8_HOME="$HOME/.sdkman/candidates/java/8.0.482-tem"
  export JDK11_HOME="$HOME/.sdkman/candidates/java/11.0.30-tem"
  export JDK17_HOME="$HOME/.sdkman/candidates/java/17.0.18-tem"
  export JDK21_HOME="$HOME/.sdkman/candidates/java/21.0.10-tem"
  ```
* Install/create Maven/Gradle/GitHub artifacts:
  * ~/.m2
    * settings.xml
    * settings-security.xml
  * ~/.gnupg
    * secring.gpg
    * pubring.gpg
  * ~/.gradle/
    * gradle.properties
* Create Maven toolchains configuration: `~/.m2/toolchains.xml`
  ```xml
  <?xml version="1.0" encoding="UTF8"?>
  <toolchains>
    <!-- JDK toolchains -->
    <toolchain>
      <type>jdk</type>
      <provides>
        <version>8</version>
      </provides>
      <configuration>
        <jdkHome>/home/scoba/.sdkman/candidates/java/8.0.482-tem</jdkHome>
      </configuration>
    </toolchain>
    <toolchain>
      <type>jdk</type>
      <provides>
        <version>11</version>
      </provides>
      <configuration>
        <jdkHome>/home/scoba/.sdkman/candidates/java/11.0.30-tem</jdkHome>
      </configuration>
    </toolchain>
    <toolchain>
      <type>jdk</type>
      <provides>
        <version>17</version>
      </provides>
      <configuration>
        <jdkHome>/home/scoba/.sdkman/candidates/java/17.0.18-tem</jdkHome>
      </configuration>
    </toolchain>
    <toolchain>
      <type>jdk</type>
      <provides>
        <version>21</version>
      </provides>
      <configuration>
        <jdkHome>/home/scoba/.sdkman/candidates/java/21.0.10-tem</jdkHome>
      </configuration>
    </toolchain>
  </toolchains>
  ```

## Hardware/Accessories

* Motorola ThinkPhone (XT2309-3)
  * Android 15
  * Qualcomm Snapdragon 8+ Gen 1 [Number of cores: 8]
  * Internal memory: 256 GB
  * RAM memory: 8 GB
  * 2400 x 1080
* Pixio PX160 Portable Monitor
  * 1920 x 1080
* JSAUX USB-C to HDTV adapter
* Folding Bluethooth keyboard/trackpad
* Zippered hard-sided case for keyboard, cables, and power supply

## Stuff you probably won't need

* Install `wget`:
  ```bash
  sudo apt install wget
  ```
* Install Bazelisk:
  ```bash
  wget https://github.com/bazelbuild/bazelisk/releases/latest/download/bazelisk-linux-arm64
  chmod +x bazelisk-linux-arm64
  mv bazelisk-linux-arm64 ~/.local/bin/bazelisk
  ```
* Install `buildifier`:
  ```bash
  wget https://github.com/bazelbuild/buildtools/releases/latest/download/buildifier-linux-arm64
  chmod +x buildifier-linux-arm64
  mv buildifier-linux-arm64 /usr/sbin/buildifier
  ```
* Install `jed` editor:
  * NOTE: This package must be built locally, because no one appears to have published it for the `aarch64` architecture.
    ```bash
    sudo apt install slang libxt
    git clone https://github.com/jedsoft/jed.git
    cd jed
    ./configure --prefix=/usr/local
    make clean
    make
    make xjed
    make install
    ```
#### LLVM and CLang
```bash
sudo apt install llvm
sudo apt install llvm-libs
sudo apt clang
sudo apt install python-pip
sudo apt install cbindgen
sudo apt install nodejs npm
```

#### To install JDK7 with SunEC support:
```bash
sudo apt install jdk7-openjdk
```
* Install **SubEC** components from **Linux-on-Android** repository into JDK7

#### To build the SunEC JAR:
* Install Mercurial (version control client):
  ```bash
  sudo -u scoba yay -S mercurial
  ```
* ... with which I downloaded the source for OpenJDK 7
* ... so that I could build a JAR from the SunEC source files
* ... which I dropped into the jre/lib/ext folder of java-7-openjdk
* I duplicated the 'libsunec.so' native library from JDK8 to JDK7, which hasn't blown up yet.

NOTE: The ability to drop an unsigned encryption library into the JRE without complaint was more than a bit surprising. Although this required file-system access, this seems like a bit of a security vulnerability to me.

#### To clone the `geckodriver` repo, you need the 'git-cinnabar' extension:
* Clone and build `git-cinnabar` extension:
  ```bash
  git clone https://github.com/glandium/git-cinnabar.git
  cd git-cinnabar
  make
  ```
* Add `git-cinnabar` folder to the **PATH**

> Written with [StackEdit](https://stackedit.io/).
