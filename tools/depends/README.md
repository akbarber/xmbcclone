![Kodi Logo](../../docs/resources/banner_slim.png)

# Kodi's Unified Depends Build System
This builds native tools and library dependencies for platforms that do not provide them. It is used on our continuous integration system, **[jenkins](http://jenkins.kodi.tv/)**. A nice side effect is that it allows us to use the same tools and library versions across all platforms.

In terms of build system usage, largest percentage is Autotools, followed by CMake and, in rare cases, hand crafted Makefiles. Tools and libraries versions are picked for a reason. If you feel the urge to start bumping them, be prepared for robust testing. Some tools and libraries need patching, most do not.

That said, we try to stay fairly current with used versions and send patches upstream whenever possible.


* **Autotools driven tools and libraries** tend to just work **provided** the author(s) followed proper Autotools format. Execute `./bootstrap`, followed by `./configure --[...]` and you're all set. If `./configure --[...]` gives you problems, try `./autoreconf -vif` before `./configure --[...]`.
Some authors do silly things and only a `config.site` can correct the errors. Watch for this in the config.site(.in) file(s). It is the only way to handle bad Autotools behaviour.

* **CMake driven tools and libraries** also tend to just work. Setup CMake flags correctly and go. On rare cases, you might need to diddle the native CMake setup.

* **Hand crafted Makefiles driven tools and libraries** typically require manual sed tweaks or patching. May give you nightmares.

## Usage Examples
Paths below are examples. If you want to build Kodi, follow our **[build guides](../../docs/README.md)**.
### All platforms
`./bootstrap`
### Darwin
**macOS (i386)**  
`./configure --host=i386-apple-darwin`

**macOS (x86_64)**  
`./configure --host=x86_64-apple-darwin`

**iOS (armv7)**  
`./configure --host=arm-apple-darwin`

**iOS (arm64)**  
`./configure --host=arm-apple-darwin --with-cpu=arm64`

**tvOS**  
`./configure --host=arm-apple-darwin --with-platform=tvos`

**NOTE:** You can target the same `--prefix=` path. Each setup will be done in an isolated directory. The last configure/make you do is the one used for Kodi/Xcode.
 
### Android
**arm**  
`./configure --with-tarballs=$HOME/android-tools/xbmc-tarballs --host=arm-linux-androideabi --with-sdk-path=$HOME/android-tools/android-sdk-linux --with-ndk-path=$HOME/android-tools/android-ndk-r20 --prefix=$HOME/android-tools/xbmc-depends`

**aarch64**  
`./configure --with-tarballs=$HOME/android-tools/xbmc-tarballs --host=aarch64-linux-android --with-sdk-path=$HOME/android-tools/android-sdk-linux --with-ndk-path=$HOME/android-tools/android-ndk-r20 --prefix=$HOME/android-tools/xbmc-depends`

**x86**  
`./configure --with-tarballs=$HOME/android-tools/xbmc-tarballs --host=i686-linux-android --with-sdk-path=$HOME/android-tools/android-sdk-linux --with-ndk-path=$HOME/android-tools/android-ndk-r20 --prefix=$HOME/android-tools/xbmc-depends`

### Linux
**ARM (codesourcery/lenaro/etc)**  
`./configure --with-toolchain=/opt/toolchains/my-example-toolchain/ --prefix=/opt/xbmc-deps --host=arm-linux-gnueabi`

**Raspberry Pi**  
`./configure --with-platform=raspberry-pi --host=arm-linux-gnueabihf --prefix=/opt/xbmc-deps --with-tarballs=/opt/xbmc-tarballs --with-toolchain=/opt/rbp-dev/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf --with-firmware=/opt/rbp-dev/firmware --build=i686-linux`

**Native**  
`./configure --with-toolchain=/usr --prefix=/opt/xbmc-deps --host=x86_64-linux-gnu`

Cross compiling is a PITA.