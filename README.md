# static-linked-tools
This is a collection of static linked binaries for x86_64 Linux systems. These
are used as "known good" copies when doing forensics or rescuing a system with
broken libraries.

Below are how I have compiled/obtained these. I encourage you to do this
yourself instead of trusting me to do it for you.

## coreutils
These were made on a Ubuntu 16.04 system:

- install gcc, make, and all that stuff you need to compile C programs
- obtain source from https://ftp.gnu.org/gnu/coreutils/

```
cd /usr/lib/gcc/x86_64-whatever/version_here
cp crtbeginT.o crtbeginT.o.bak
cp crtbeginS.o crtbeginT.o
cd back/to/coreutils-8-xx
./configure
make SHARED=0 CFLAGS="-static -static-libgcc -static-libstdc++ -fPIC"
find src -type f -perm 755 -exec mv {} destination_directory/ \;
```

## binutils
These were made on a Ubuntu 16.04 system:

- install gcc, make, and all the dependencies for this
- obtain source from https://ftp.gnu.org/gnu/binutils

```
cd binutils-VERSION
./configure
make LDFLAGS="-all-static"
for file in $(find . -type f -perm 775 -exec file {} \; |grep ELF |cut -d : -f 1); do cp $file ~/OUTPUT-DIRECTORY; done
```

## bash
I just stole this from Ubuntu:

```
apt install bash-static
cp /bin/bash-static /destination/bash
```

