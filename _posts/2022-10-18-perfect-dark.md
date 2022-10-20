---
layout: post
title: Perfect Dark Source
hide_title: false
feature-img: "assets/images/posts/perfect-dark-source.png"
color: rgba(17,55,138,255)
bootstrap: false
tags: [nintendo, n64, perfect dark, source code, how-to]
---
Being a child of the late 80's it's hard not to be impacted by the proliferation of video games in our culture. I know I spent countless hours playing everything from the very common Galaga and Space Invaders to less common but still widespread Dance Dance Revolution. At least the latter required some exercise to play. 

Still, none of these games captured my mind like Goldeneye and it's spiritual successor Perfect Dark did for the Nintendo 64. These two games are responsible for countless hours spent with my friends and me playing split screen together doing everything from all out death matches to capture the flag sessions and more. The thrill I got booting up Perfect Dark for the first time in the year 2000 can't even be put into words. It honestly was one of the most thrilling movements of my youth and will forever be a fond memory. Even then, I knew this game was special and it would be something that I would carry with me for a long time. 

As the years went on, and Perfect Dark occupied a slightly smaller space in my brain than it did in 2000, my thoughts began to shift. During it's heyday, I would spend countless hours with friends messing with Gameshark codes to see what we could get the game to do and unlocking the hidden and secret aspects of the game that the developers didn't want us to see. It added that much extra thrill to the game and having that power over it only deepened my understanding and love for the code that drove one of my favorite entertainment experiences of all time. What if I could get the source code that the developers wrote to make the game? I could change anything about the game I wanted.

Fast forward to 2022 and thanks to the incredible work of [Ryan Dwyer](https://github.com/RyanDwyer) we have the entire source code of the game decompiled. It's hosted on [GitHub](https://github.com/n64decomp/perfect_dark) and is entirely buildable. I wanted to build it myself so I launched into research mode on how to setup a development environment and was actually really surprised with how much information was available about setting up the N64 SDK and toolchain. In fact, there is a bit too much information - if that is ever a thing. There are a lot of options for the toolchain and it's not always clear how to go about setting everything up. I wanted to build natively on my Mac but in the interest of time I turned to our trusty friend, an Ubuntu VM, to get going quickly.

Here is a quick run through of what I needed to do to get this working. I am sure there are more ideal ways to get this going but in my fever to get it working this the the path that got me there the quickest.

I fired up Virtual Box and downloaded the [Ubuntu 20.04 LTS](https://ubuntu.com/download/server) server ISO. I whipped up a quick VM and did a minimal install to keep the VM small. I can install whatever tools I need after the VM comes up anyway.

After Ubuntu was installed and running I [setup SSH](https://linuxhint.com/setup-enable-ssh-ubuntu-virtual-box/) and logged into the VM. From here, I used the VM totally headless.

I installed some packages:
`sudo apt install binutils-mips-linux-gnu build-essential gcc-mips-linux-gnu libc6-dev-i386 libcapstone-dev make cmake curl wget net-tools git vim`

Not all of these are strictly required but they make using the minimal install of the VM more convenient.

Then I built `armips` from source and copied it to my part of my PATH:
```
mkdir tools
cd tools
git clone --recursive https://github.com/Kingcom/armips.git
cd armips/
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
sudo cp ~/tools/armips/build/armips /usr/bin/
```

The build will take a few minutes to complete depending on how many resources you provided your VM.

Now lets grab the actual Perfect Dark source code:
```
cd ~/
mkdir code
cd code/
git clone https://github.com/n64decomp/perfect_dark.git
cd perfect_dark/
```

Now, to build you will need a legal copy of the game in ROM form. I suggest you grab one from [eBay](https://www.ebay.com/sch/i.html?_nkw=perfect+dark+n64) if you don't already have a cartridge. Once you have a copy of the game, take the ROM and put it in the `~/code/perfect_dark` folder named `pd.ntsc-final.z64`. You will then need to run `make extract` to pull all the assets from the game ROM into your local copy of the repo. 

The output of the extract will be pretty basic but don't be fooled a lot of content is now in place.
```
n64@n64dev:~/code/perfect_dark$ make extract
ROMID=ntsc-final tools/extract
n64@n64dev:~/code/perfect_dark$ 
```

From here, you could start messing with the source code and build your own modified ROM of the game but it makes more sense to try and build and make sure everything is working. To do that we need to run a couple more commands:

```
git submodule update --init --recursive
make -j
```

The git command pulls a dependency and only needs to be run once. The make command should be self explanatory at this point. Once you run the make command you will start to see all of the game files scrolling by as the system assembles them into the game ROM.

Once it's complete, you can compare the output of the original ROM with a simple MD5 hash:
```
n64@n64dev:~/code/perfect_dark$ md5sum build/ntsc-final/pd.z64 pd.ntsc-final.z64 
e03b088b6ac9e0080440efed07c1e40f  build/ntsc-final/pd.z64
e03b088b6ac9e0080440efed07c1e40f  pd.ntsc-final.z64
```

They match! We have successfully built the same ROM file that is burned onto the retail NTSC cartridges. Awesome! 

This is a quick and dirty setup so some fine tuning would go a long way in building a more optimized version of the game with modern compilers. I am honestly too excited to have the source to do anything major with it at this point. I am enjoying just reading it over and getting to know it for the time being. 

In the future, I would love to be part of the community that modifies and modernizes this code base for future generations of gamers to enjoy. Even if we just leave the game as designed and apply some modern optimizations to the code I think that would be a major step forward for this classic. Time will tell what the community at large can bring to the table and I look forward to enjoying it all and pitching in where I can. 
