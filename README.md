# Debian Packaging for CEF 117

This is Debian packaging repository for the [Chromium Embedded Framework
v117](https://bitbucket.org/chromiumembedded/cef).

## Binary Packages

Precompiled Debian packages can be found here:
[ppa:ppa-verse/casparcg](https://launchpad.net/~ppa-verse/+archive/ubuntu/casparcg).

## Building from Source

#### Download and unpack source files:

```shell
p=chromium-embedded-117
v=117.2.5+gda4c36a

# Download CEF tarballs
for n in amd64:64 arm64:arm64 armhf:arm; do
    a=`echo $n | cut -d: -f1`
    b=`echo $n | cut -d: -f2`
    wget -cO ${p}_${v}.orig-${a}.tar.bz2 \
        https://cef-builds.spotifycdn.com/cef_binary_${v}+chromium-117.0.5938.152_linux${b}_minimal.tar.bz2
done

# Download Debian packaging
git clone -b master https://github.com/dimitry-ishenko-casparcg/${p}.git
cd $p
git ls-files -z :^debian | tar -caf ../${p}_${v}.orig.tar.gz --null -T-

# Unpack CEF tarballs
for a in amd64 arm64 armhf; do
    mkdir $a
    tar -xjC $a --strip-components=1 -f ../${p}_${v}.orig-${a}.tar.bz2
done
```

#### Build source and binary packages:

```shell
# Create source package
dpkg-buildpackage --build=source --no-check-builddeps --no-pre-clean -sa

# Build locally...
sudo pbuilder build ../*.dsc

# ... or upload to PPA
dput ppa:<user-name>/<ppa-name> ../*.changes
```

Share and enjoy.

## Authors

* **Dimitry Ishenko** - dimitry (dot) ishenko (at) (gee) mail (dot) com

## License

This project is distributed under the GNU GPL-2 license.
