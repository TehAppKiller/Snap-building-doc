# Building
## Build from GitHub (Cannonical Launchpad automatized building)
[Canonical Launchpad build farm](https://snapcraft.io/docs/build-from-github) allows to compile for every port automaticaly.\
But **BEWARE**, the farm has not always last snapcraft version and may fail on some ports while it will work locally (w/ or w/o cross-compilation).
## Build locally
Go in base directory and build:
```
snapcraft -v
```
## Troubleshoot
To resolve problems, you may check [snapcraft documentation](https://snapcraft.io/docs).\
Easy resolution is often to perform a clean:
```
snapcraft clean
```
## Cross compilation with Core20
Cross compilation requires target architectures repositories to be able to find the snap's required packages.

Add target architecture sources (armhf, arm64):
```
sudo dpkg --add-architecture armhf
sudo dpkg --add-architecture arm64
```
Add package repository sources at end of sources.list.d:
```
sudo nano /etc/apt/sources.list.d/ubuntu.sources
```
You should notice `Architectures: amd64` added to the 2 original parts to avoid non-resolved ports\
(armhf and arm64 does not exist in http://archive.ubuntu.com/ubuntu ).\
Noble (Core24) and Focal (Core20) repositories are then added\
(armhf, arm64 and other different ports are in http://ports.ubuntu.com/ubuntu-ports ).
```
# This is the content of /etc/apt/sources.list.d/ubuntu.sources

# System repository (amd64 system here)
Types: deb
URIs: http://archive.ubuntu.com/ubuntu
Suites: noble noble-updates noble-backports
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
Architectures: amd64

Types: deb
URIs: http://security.ubuntu.com/ubuntu
Suites: noble-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
Architectures: amd64

# X-Compilation noble (armhf, arm64)
Types: deb
URIs: http://ports.ubuntu.com/ubuntu-ports
Suites: noble noble-updates noble-backports
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
Architectures: armhf arm64

Types: deb
URIs: http://ports.ubuntu.com/ubuntu-ports
Suites: noble-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
Architectures: armhf arm64

# X-Compilation focal (armhf, arm64)
Types: deb
URIs: http://ports.ubuntu.com/ubuntu-ports
Suites: focal focal-updates focal-backports focal-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
Architectures: armhf arm64
```
- [ ] TODO: Add library dependencies checking procedure
      
Build with:
```
snapcraft --enable-experimental-target-arch --target-arch=armhf --destructive-mode
snapcraft --enable-experimental-target-arch --target-arch=arm64 --destructive-mode
```

## Cross compilation with Core20
- [ ] TODO: Fill section

# Versionning
Current versionning of my snaps follows the exact same one from the source.\
If a re-build is needed, new build version appends source version with a '-v2' ; following builds increment this one '-v3', '-v4',... etc\
Even if not ideal, to simplify the management with snapcraft, re-build increment is based on current stable published version for now (i.e. there can be several different -v2 rc/beta/edge releases published ; but each stable release has a different version)
- [ ] TODO: Modifiy if there is a way with snapcraft to have build versionning and update with final version
