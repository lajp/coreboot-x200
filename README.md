# Coreboot X200

This repository includes my coreboot .config file for the ThinkPad X200 as well as
instructions to install it. Additionally I also include some small scripts I use
on my corebooted X200.

This coreboot build is based on the a36b8472eb35d4725b3d9e296fd24c878114a989 commit and utilizes
the free GBE drivers provided by the libreboot project.

## Important notes

Please note that this is by no means an official guide and everything included in this repo shall
be read and executed with extra care. If you are new to coreboot I highly recommend refering to the
official guides hosted on [their website](https://coreboot.org)!

This "guide" doesn't include documentation of the initial flashing process but merely instructions
to updating an already core/librebooted machine. If you are looking to core/libreboot your X200 for the
first time I highly suggest you reading [this excellent guide on reddit](https://www.reddit.com/r/libreboot/comments/7dajn6/x200_libreboot_tutorial_for_raspberry_pi_with).

## Installation

I recommend not building the image on the X200 as it will take a loong while.

My config has been generated and tested on the following commit: a36b8472eb35d4725b3d9e296fd24c878114a989

The installation steps would be somewhat similar to this:
```sh
git clone https://github.com/lajp/coreboot-x200
# Download the coreboot repository
git clone https://github.com/coreboot/coreboot
cd coreboot
git checkout a36b8472eb35d4725b3d9e296fd24c878114a989
cp -f ../coreboot-x200/.config .

# Build the coreboot image
make crossgcc-i386 CPUS=$(nproc)
# Generate the rest of the .config by opening menuconfig and saving the config
make menuconfig
make -j$(nproc)

# Generate and add the libreboot gbe drivers to the image
git clone https://notabug.org/libreboot/ich9utils.git
cd ich9utils
make -j$(nproc)
./ich9gen --macaddress XX:XX:XX:XX:XX:XX
cd ..
dd if=ich9utils/ich9fdgbe_8m.bin of=build/coreboot.rom bs=1 count=12k conv=notrunc

# Finally flash the image on the X200 (This guide assumes you're already running coreboot/libreboot on your X200)
flashrom -p internal:laptop=force_I_want_a_brick -w build/coreboot.rom
```

## Other stuff in this repo

This repo also includes a [misc](misc/README.md) folder with some stuff I use on my X200.

Please note that the scripts included in the misc folder are really badly written
and might not work for you. The main reason why I even include them here is so that I can find them later
if I'm working on something similar. But if you find them useful, do use them :).
