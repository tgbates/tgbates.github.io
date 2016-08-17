---
layout: post
title: Upgrade from Linux Mint 17.1 MATE to 17.3
date: 2016-08-17 08:00:00 -0600
category: linux
tags: [linux, mint, servicenow]
---

## Update

I moved West to start a new job and during the chaos wasn't updating my blog.  
I'm getting settled in and have been learning a lot so I should be able to throw up some more content here.  
I have also posted [a few more ServiceNow scripts in my snow repo.](https://github.com/tgbates/snow)

## New Office

I had my desktop packed up until I could set up an office at my new place.
After unpacking it, reinstalling my hard drives and plugging in my monitors, I couldn't get them working right.
Either my primary monitor would work with the right resolution but the secondary wouldn't display anything,
or the secondary would only be a clone of the primary, which was at such a low resolution it was one step above useless.
I also have to admit that at one point I thought I needed an adapter (was I secretly wishing for an excuse to make my first trip ever to a Fry's?) when I really only needed my DVI cable which was tangled up with all my other cables.
And I also plugged in my DVI and HDMI into different video cards (my previous setting, if documented with a picture in Evernote before I packed for the move, wasn't consulted).

## Emptying the Trash

Oh, I forgot to mention that I was doing some last-minute backups before the move and managed to fill up /dev/sda1.

Before worrying about the dual monitors I had to boot to a command prompt and clean out the trash.
[I found a little tip on how to prevent that from happening again.](https://forums.linuxmint.com/viewtopic.php?f=90&t=225222#p1187583)

## Black Screens and Elephants

I looked at my notes on the last time I had this situation back in 2015.  

```    ctrl+alt+f1 to get to terminal  ```<br>
```    $ sudo service mdm stop  ```<br>
```    $ sudo X -configure  ```<br>
```    $ sudo service mdm start  ```<br>

This generated a new xorg.conf file that I could move to /etc/X11 but all it really accomplished for me was getting resolution right for 1 monitor and nothing for the secondary monitor.  After having two monitors, you can't go back to only one.

I'm sorry for not having more screenshots but I wasn't in a position to capture anything but text and will refrain from pasting huge blocks of output.

During this monitor process (I will skip recounting my dead ends playing with cvt, xrandr, get-edid | parse-edid, modelines, and editing xorg.conf), 
I had to blindly resort to the [Raising Elephants Is So Utterly (Obtuse/Boring) trick a few times.](http://www.howtogeek.com/119127/use-the-magic-sysrq-key-on-linux-to-fix-frozen-x-servers-cleanly-reboot-and-run-other-low-level-commands/)

## Time to Upgrade

By this point I had been flailing around for about 3 hours, so I decided it was time to try something else.
Upgrading to Mint 17.3 wasn't really on the weekend agenda, but it did promise better multi-monitor support, so it was worth a shot.
I didn't see how it could make things worse, and it was only a minor upgrade and was quick and painless.  This is why I installed Linux Mint in the first place!
It wasn't a silver bullet, but at least it helped an aggravating problem I was having with windows missing their menu bars and spawning partially off my screen in the top left corner.

## Use the --force

After that I thought updating my graphics drivers might help.
Another tip: If you use proprietary graphics drivers downloaded from AMD's website and --force the installation you will break synaptic and have to fix it.

**BAD IDEA**---> ```    $ sudo ./amd-driver-installer-14.20-x86.x86_64.run --force  ```

Worse than that, I broke X completely when I uninstalled fglrx from the command line.  

When I saw the results of that I reinstalled it but by then it was time to leave for church and I didn't have time to try it out.

...a couple hours later, I booted up and the monitors were detected (they had been "unknown" all this time in Mint) and I was able to set up resolutions, set primary/secondary, get my panel set up correctly, etc. 

## Fixing Synaptic

Synaptic couldn't heal itself and Warned   
    ```Uninstall : inst_path_default or inst_path_override does not exist in /etc/ati.```

![broken Synaptic](/assets/20160815_monitorsb.jpg "broken synaptic")

[I found a bug workaround posted that pointed a way to the solution.](https://bugs.launchpad.net/ubuntu/+source/fglrx-installer/+bug/565407)

##Use the --force again and Reinstall the AMD driver!

```
    $ sudo ./amd-driver-installer-14.20-x86.x86_64.run --force  
```<br>
```
    $ sudo apt-get -f install  
```

...


```
    restore of system environment completed
```

Now Synaptic is working properly again.

## Lessons Learned

* I should have been more patient.  Impatience could have led to reinstalling my hard drives incorrectly, along with the incorrect initial monitor setup. I'm fairly certain I took a picture of my cables before the move but didn't take the time to look for it.  
* I should have uninstalled fglrx properly before installing the AMD driver (without using the --force option, which the AMD installer recommended not using).  
* I didn't always think before I acted when running commands.  This is only my personal computer, but it is still a my most important personal computer, and a useful tool that I don't want to break irreparably.  
* I ended up spending hours on this when maybe I could have only had to uninstall and reinstall fglrx if plugging the cables in right hadn't prevented this from happening in the first place.



