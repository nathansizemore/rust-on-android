# Rust on Android

1. Download and install the latest [Android NDK](https://developer.android.com/tools/sdk/ndk/index.html)
2. Setup a standalone toolchain for your specific Android [API Level](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
3. Clone the [Rust Repository](https://github.com/rust-lang/rust)
4. Configure rustc to be built with NDK toolchain

---

### Document Examples
The examples in this document were tested with the following software versions:

~~~
OS X: 10.10.2 (14C1510)
Android API: 14 (Ice Cream Sandwich)
Android NDK: r10d
rustc: 1.0.0-dev (46f649c47 2015-03-18) (built 2015-03-18)
cargo: 0.0.1-pre-nightly (07cd618 2015-03-12) (built 2015-03-13)
~~~

The same commands will also work for Linux.  Additional dependencies may be needed for working with the Android build chain on 64-bit Linux distros. [This](http://stackoverflow.com/questions/2710499/android-sdk-on-a-64-bit-linux-machine) posting covers most major distribution dependencies.

---

### Android NDK
Use NDK version r9b or later.  Jemalloc may not be able to build with previous NDK versions.

---

### NDK Standalone Toolchain
The following command builds an NDK toolchain for Android API 14, arm architecture in the ~/bin/ndk-standalone-14-arm directory.

~~~bash
~/android-ndk-r10d/build/tools/make-standalone-toolchain.sh --platform=android-14 --arch=arm --install-dir=~/bin/ndk-standalone-14-arm
~~~

---

### Clone Rust Repository
~~~bash
git clone https://github.com/rust-lang/rust
~~~

---

### Configuring and Building rustc
~~~bash
cd rust
mkdir build
cd build
../configure --target=arm-linux-androideabi --android-cross-path=~/bin/ndk-standalone-14-arm/
make
sudo make install
~~~

---

### Compiling for ARM
#### rustc
~~~bash
rustc --target=arm-linux-androideabi -C linker=~/bin/ndk-standalone-14-arm/bin/arm-linux-androideabi-gcc main.rs
~~~
#### cargo
Cargo needs the linker information when passing `--target=arm-linux-androideabi`

Create a `.cargo/config` file in the project root directory

~~~bash
cargo new some-project --bin
cd some-project
mkdir .cargo
touch .cargo/config
~~~

Place the linker path in the `config` file

~~~
[target.arm-linux-androideabi]
linker = "~/bin/ndk-standalone-14-arm/bin/arm-linux-androideabi-gcc"
~~~

Build

~~~
cargo build --target=arm-linux-androideabi
~~~
