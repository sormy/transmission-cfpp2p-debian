# Transmission cfpp2p mod for Debian

This is Transmission 2.77 cfpp2p mod for Debian. It can also be used for other
debian-like distributions, for example, armbian/ubuntu.

It contains Transmission 2.77 with cfpp2p mod covering daemon and web client and
bug fixes (qt/gtk clients are not included while they might work).

Code in this repo is forked from
http://deb.debian.org/debian/pool/main/t/transmission/transmission_2.94-2+deb10u2.debian.tar.xz
that is written against
http://deb.debian.org/debian/pool/main/t/transmission/transmission_2.94.orig.tar.gz
and corrected to work with 2.77 cfpp2p mod.

## Features

Here is a list of features comparing to regular Transmission:

- streaming mode
- cheating mode
- skip verify

## Installation

```sh
# install build dependencies
apt install debhelper dh-autoreconf intltool libcurl4-openssl-dev libevent-dev libminiupnpc-dev libnatpmp-dev libnotify-dev libsystemd-dev zlib1g-dev

# clone last supported versions of cfpp2p mod
git clone https://github.com/cfpp2p/transmission.git -b 2.77plus14734-19 transmission-cfpp2p
git clone https://github.com/cfpp2p/transmission.git -b 'Windows_Daemon_&_Clients' transmission-cfpp2p-clients
git clone https://github.com/sormy/transmission-cfpp2p-debian.git -b 'Windows_Daemon_&_Clients' transmission-cfpp2p-debian

# build
rsync -a --exclude=.git transmission-cfpp2p/ transmission-build/
cp -rfv transmission-cfpp2p-clients/web-client-cfp transmission-build/
cp -rfv transmission-cfpp2p-debian/debian transmission-build/debian
cd transmission-build
debuild -i -us -uc -b

# install
cd ..
apt install ./transmission-daemon_2.77plus14734-19+cfp2p_arm64.deb \
            ./transmission-common_2.77plus14734-19+cfp2p_all.deb \
            ./transmission-cli_2.77plus14734-19+cfp2p_arm64.deb

# prevent updates for installed packages
echo "transmission-daemon hold" | sudo dpkg --set-selections
echo "transmission-common hold" | sudo dpkg --set-selections
echo "transmission-cli hold" | sudo dpkg --set-selections

# check hold locks
dpkg --get-selections | grep transmission

# cleanup
rm -rf transmission-cfpp2p transmission-cfpp2p-clients
```
