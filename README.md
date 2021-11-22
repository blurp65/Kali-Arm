# Docker container to build arm images.

Make container docker.
```bash
git clone --single-branch --branch docker \
  https://gitlab.com/kalilinux/build-scripts/kali-arm.git docker

cd docker

./build.sh
```

Make image.
```bash
# Update git files.
docker exec -it kali-builder git pull

# Install depencecies.
docker exec -it kali-builder ./build-deps.sh

# Compile image.
docker exec -it kali-builder bash -c "debug=true ./rpi3-64.sh 2020.4"
```

Copy image out of container.
```bash
# List images compiled.
docker exec -it kali-builder sh -c "ls -la *.xz"

#Copy image out of container.
docker cp kali-builder:/kali/kali-linux-2020.4-rpi4-nexmon-64.img.xz .
```
=======
## Kali-ARM Build-Scripts

Kali Linux ARM build-scripts.

These are the same build scripts that we use to generate the pre-generated official Kali Linux ARM images, found here: https://www.kali.org/get-kali/

There are additional scripts included in this repository, supporting more devices, but these will need to be built in order for them to be used.

For more information, please see: https://www.kali.org/docs/arm/

- - -

### Building

- These scripts are tested on Kali Linux arm64, x64 and x86 installations only _(We **recommend x64**)_
- Make sure you run the `./common.d/build_deps.sh` script before trying to build an image, as this installs all required dependencies
- You will need at **least 8GB of RAM or use SWAP file**

An example workflow to build a _[Raspberry Pi 4](https://www.kali.org/docs/arm/raspberry-pi-4/) Kali Linux image_ would look like:

```
cd ~/
git clone https://gitlab.com/kalilinux/build-scripts/kali-arm
cd ~/kali-arm/
sudo ./common.d/build_deps.sh
sudo ./rpi.sh
```

- Depending on your system hardware & network connectivity, will depend on how long it will take to build _(4 core CPU, 8GB RAM, SSD inside a VM takes using a [local repo](https://www.kali.org/docs/community/setting-up-a-kali-linux-mirror/) about 100 minutes per script)_
- On x64 systems, after the script finishes running, you will have an image files located in `~/kali-arm/images/` called `kali-linux-2021.3-rpi-armhf.img.xz`
- On x86 systems, as they do not have enough RAM to compress the image, after the script finishes running, you will have an image file located in `~/kali-arm/images/` called `kali-linux-2021.3-rpi-armhf.img`
  - _Should you want to try and shrink the file to make it easier to distribute, you will need to use **your own preferred compression**_.

- - -

### Help

On any build script, add `--help`. Example:

```
$ ./rpi.sh --help
 Usage commands:
# Architectures (arm64, armel, armhf)
./rpi.sh --arch arm64 or ./rpi.sh -a armhf

# Desktop manager (xfce, gnome, kde, i3, i3-gaps, lxde, mate, e17 or none)
./rpi.sh --desktop kde or ./rpi.sh --desktop=kde

# Minimal image - no desktop manager
./rpi.sh --minimal or ./rpi.sh -m

# Slim image - no desktop manager & cli tools
./rpi.sh --slim or ./rpi.sh -s

# Enable debug & log file (./logs/<file>.log)
./rpi.sh --debug or ./rpi.sh -d

# Perform extra checks on the images build
./rpi.sh --extra or ./rpi.sh -x

# Help screen (this)
./rpi.sh --help or ./rpi.sh -h
$
```

- - -

### Custom Values

Changing [builder.txt](builder.txt.example), will allow for custom values, such as using a local LAN mirror:

```
$ echo 'mirror="http://192.168.1.100/kali"' > ./builder.txt
```

- - -