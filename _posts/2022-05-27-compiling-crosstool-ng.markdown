---
title: How to compile crosstool-ng
author: eric_riff
date: 2022-05-27 18:50:00 -0300
categories: [Tutorial, Toolchains]
tags: [compile, build, crosstool, crosstool-ng, toolchain]
---

Crosstool-ng is a wonderful tool to create custom toolchains. It's open source, it has a `menuconfig` style interface and it's very flexible, intuitive and self-documented (many `help` entries). You can learn all about it [here](https://crosstool-ng.github.io/).  
But this tool has a (rather small) flaw: there are no pre-built packages, if you want to use it then you'll have to build it from sources.  
This tutorial covers a couple of ways of doing this: natively and docker-based.  

> Everything on this tutorial has been tested on Ubuntu 20.04 LTS
{: .prompt-info }

# Native builds
This is the most straightforward way of building `crosstool-ng`: just install the dependencies, run a couple of scripts and you're good to go. The main drawback of this approach is that you pollute your OS with many packages and their dependencies. If you don't mind installing packages then you can continue on this path, otherwise use the docker-based builds.

## Getting the sources
You can do this by cloning the repo or downloading a tarball. I'll use the former method, so I'll end up in `master`. There's nothing inherently wrong with building out of master, but it is recommended to use a release instead. To do this we will list all available tags and choose the latest one.

```bash
# Start by cloning and cd-ing into the repo
git clone https://github.com/crosstool-ng/crosstool-ng
cd crosstool-ng

# Figure out which release is the latest.
# Ignore the ones with the -rc suffix, those are release candidates.
git tag -l --sort=-version:refname

# Checkout the latest release. At the time of writting that is crosstool-ng-1.25.0.
git checkout crosstool-ng-1.25.0
```

## Installing requirements
As mentioned before, in order to build natively you need to install many packages. Since this is an Ubuntu-focused tutorial we will use `apt` for that:

```bash
sudo apt update
sudo apt install -y gcc g++ flex autoconf texinfo xz-utils unzip help2man file patch gawk make libtool libtool-bin libncurses5-dev bison
```

## Building and Installing
To build we just need to run the `booastrap` script and then `configure` and `make`. Select a proper install folder with `--prefix`, I'll use `/opt/crosstool-ng`.
```bash
./bootstrap
./configure --prefix /opt/crosstool-ng
make all
sudo make install
```

## Running it.
The tool has been built and installed, but it is not yet (easily) accessible since is not on your PATH. There are many solutions for this, choose the one that you prefer:

```bash
# 1. Use the absolute path
/opt/crosstool-ng/bin/ct-ng menuconfig

# 2. Use the PATH env variable
export PATH=/opt/crosstool-ng/bin/:$PATH
ct-ng menuconfig

# 3. Automatically export PATH when you open up a new terminal
nano ~/.bashrc
# Add export PATH=/opt/crosstool-ng/bin/:$PATH at the end of this file
source ~/.bashrc
ct-ng menuconfig

# 4. Create a symlink to the executable on a place that's already visible on PATH
sudo ln -s /opt/crosstool-ng/bin/ct-ng /usr/bin/ct-ng
ct-ng menuconfig
```

## Conclusion
Now you're ready to start building custom toolchains. Happy hacking!


# Docker-based builds.
Under construction.