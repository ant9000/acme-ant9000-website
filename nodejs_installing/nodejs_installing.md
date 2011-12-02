# Installing NodeJS

* [Using precompiled .deb packages](?id=nodejs_installing#deb)
* [Compiling your own .deb packages](?id=nodejs_installing#deb-build)
* [Installing from source](?id=nodejs_installing#src)

<a name="deb"></a>

## Using precompiled .deb packages

Debian does not provide NodeJS within its Squeeze release, but it is available for Sid and to have it on the FoxBoard is mostly a matter or recompiling it.

To let you jump start into NodeJs development, we provide [an archive](https://github.com/ant9000/FoxNode/downloads/) of precompiled Debian packages for the FoxBoard.

Here are the instructions for downloading the archive to your FoxBoard, unpacking it and installing the debian packages:

<pre class="minicom">
wget https://github.com/ant9000/FoxNode/downloads/nodejs-foxboard-debs.tar
tar xvf nodejs-foxboard-debs.tar
cd debs
dpkg -i `ls *.deb|grep -v dbg`
apt-get -f install
node -v
</pre>

(with these instructions you will not install the *dbg* packages, which are big and mostly needed to debug NodeJS itself).

<a name="deb-build"></a>
## Compiling your own .deb packages

In order to provide the precompiled packages on Squeeze, we quite simply rebuilt Debian/Sid ones on the FoxBoard. The process does take some time to complete on your humble FoxBoard, but it is worth its while to document it in detail: Sid is a moving target, and some of you might well want to get the newer versions of NodeJS.

The first step required for rebuilding everything on the FoxBoard is to add a line

<pre class="minicom">
deb-src http://ftp.debian.org/debian sid main contrib non-free
</pre>

to your */etc/apt/sources.list* file, so that *apt-get source* will know where to grab the source packages we need.


NodeJS is based on Javascript V8, which on Debian is packaged separately as *libv8* to be shared with other packages (most notably, with Chromium).

**NB**: in the following instructions we refer to specific version numbers - these are likely going to be different by the time you need them, so please adjust the commands accordingly.

A dependency of *libv8* is *libev4*, which we are going to rebuild first:

<pre class="minicom">
export CFLAGS="-march=armv5te"
apt-get source libev/sid
apt-get build-dep libev/sid
cd libev-4.04
dpkg-buildpackage
cd ..
dpkg -i libev4*.deb libev-dev*.deb
</pre>

Then it is time for *libv8*, which is the biggest package of the batch. Compilation completes correctly but the tests default timeout of 60 seconds is too short and they fail. Compiling with tests disabled we obtain the required package - after a few hours:

<pre class="minicom">
export CFLAGS="-march=armv5te"
export DEB_BUILD_OPTIONS="nocheck"
apt-get source libv8/sid
apt-get build-dep libv8/sid
cd libv8-3.1.8.22
dpkg-buildpackage
cd ..
dpkg -i libv8-3*.deb libv8-dev*.deb
</pre>

Finally, we get to *nodejs* itself - again, tests exceed the timeouts, so we recompile without them:

<pre class="minicom">
export CFLAGS="-march=armv5te"
export DEB_BUILD_OPTIONS="nocheck"
apt-get source nodejs/sid
apt-get build-dep nodejs/sid
cd nodejs-0.4.11
dpkg-buildpackage
cd ..
dpkg -i nodejs_0.4*.deb
</pre>

In order to follow Debian policy, which looks for NodeJS modules under */usr/lib/nodejs*, we also rebuild *npm* - the node package manager:

<pre class="minicom">
apt-get source npm/sid
apt-get build-dep npm/sid
cd npm-0.2.19
dpkg-buildpackage
cd ..
dpkg -i npm*.deb
</pre>

Et voilà, les jeux sont faits!

<a name="src"></a>
## Installing from source

You might prefer building a newer version of Node or NPM, instead of relying on Debian packages which might be outdated. Since Node version 0.6.0 is just landed, here is the quick command list to compilation:

<pre class="minicom">
apt-get -y install libssl libssl-dev pkg-config
wget http://nodejs.org/dist/node-v0.6.0.tar.gz
tar -zxf node-v0.6.0.tar.gz
cd node-v0.6.0
export CFLAGS="-march=armv5te"
export CCFLAGS="-march=armv5te"
./configure --without-snapshot
make
make install
curl http://npmjs.org/install.sh | sh
</pre>

Of course, compilation on the FoxBoard takes quite some time - be patient, and you will be rewarded!
