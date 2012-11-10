# Day 2 - exploring the build system - part 1

Today I played around the buildsystem for Carambola or [OpenWRT] (https://openwrt.org/). 

First of all I forked the carambola repository to my account at github. After that I cloned it to my workstation and added the original repository from carambola to my config to keep it in the loop.
    git clone git@github.com:jkm-2012/carambola-1 carambola-fork
    cd carambola-fork
    git remote add upstream https://github.com/8devices/carambola
    git fetch upstream
    git rebase upstream/master

The remote add command is only necessary one time. The last 2 commands fetches the updates from carambola and merge the changes in my local system.

Next step is to change the package section in feed.conf.default. This will now point to 8devices.
    cd carambola-fork
    vi feed.conf.default
My feed.conf.default only contains 3 active lines:
    src-git packages git://github.com/8devices/packages.git
    src-svn xwrt http://x-wrt.googlecode.com/svn/trunk/package
    src-git luci git://nbd.name/luci.git

After that's done you have to update the feed section. Therefor OpenWRT provides a small commandline utility to do this.
    ./scripts/feeds update -a
This command downloads the necessary information to build the packages.

If you want to install some additional software for your carambola you need to add this with the feeds command. When done, the software is available within the build system.

For example I would like to have Ruby in my carambola system, I have to do this:
    ./scripts/feeds install ruby
or if you want to make all software available for the build system
    ./scripts/feeds install -a

Now you can start to configure your desired build system. 
    make menuconfig
This starts a ncurses menue where you can decide which software you want to install, etc.
With the example of Ruby above you're now able to select Ruby from the menue lang. Without that step you will only see lua in this section, this is the basic setting from OpenWRT.
So it time to start building the image. Enter
    make -j16
to start compiling and take a cup of coffee. With my machine (2x XEON, 8 Cores, 6GB RAM, 2x SAS disk) it takes about 15 minutes. Be careful with option -j. If it too high your system can run out of memory and freeze. In my opinion setting -j to double the cores of your system is a good choice.
After several times the build step is done.
In the next part I will go deeper in the build system to show where the important files are stored or can be changed to your needs.
2012-11-10 jkm-2012
