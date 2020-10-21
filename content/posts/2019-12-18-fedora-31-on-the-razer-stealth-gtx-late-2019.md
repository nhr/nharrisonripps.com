---
title: Fedora 31 on the Razer Stealth GTX (Late 2019)
author: Harrison Ripps
type: posts
date: 2019-12-18T19:49:52+00:00
url: /2019/12/18/fedora-31-on-the-razer-stealth-gtx-late-2019/
featured_image: /wp-content/uploads/2020/06/stealth-l2p-1-1024x683.jpg
categories:
  - Uncategorized

---
My hunt for a shiny new laptop always starts with the best of intentions, but well-intentioned laptops usually don&#8217;t come with gaming-ready GPUs. What do I mean by a &#8220;well-intentioned&#8221; laptop? I have a lot of respect for [System76][1] and [Star Labs][2] for their lovely Linux-first systems, and particular respect for [Purism][3]&#8216;s holistic approach to the union of open source hardware and privacy. But I&#8217;d be lying to myself if I didn&#8217;t admit that my ideal laptop is just as capable of running triple-A video games as it is of running Linux-based workloads.

With this in mind, after careful consideration of several sensible options, I ended up with [Razer&#8217;s 13&#8243; Stealth GTX][4] laptop. As far as I can tell, this is the first time that Razer has offered a discrete GPU in their 13&#8243; Stealth form factor. The result is a completely-under-the-radar ultraportable gamer that doesn&#8217;t look out of place on the desk in my office.

In order for me to make the best use of my laptop as a work system, Windows 10 wasn&#8217;t going to cut it. Even with the Windows Subsystem for Linux in place, native Linux Containers don&#8217;t work in a Windows 10 environment. Not to mention the odd looks I&#8217;d be getting from a company full of folks that, for the most part, stick with RHEL, Fedora, and MacOS.

So, here&#8217;s a walkthrough of everything I did to get a dual-booted Linux environment running on this beast of an ultrabook:

### 1. Set up Windows

Out of the box, set up the Windows environment on the Razer. This includes all of the updates of Windows, Razer drivers, the nVidia driver, etc. It will probably take a couple of hours to get through this step.

### 2. Create Space for Linux

Using the built-in Windows Disk Management utilities, shrink the largest single partition on your hard drive. Personally I wanted about 120GB for my Linux setup out of the total 512GB of space on the drive. The largest partition was like 450GB, and now it&#8217;s down to 330ish. Plenty of space for video games.

### 3. Disable SecureBoot

For the approach I&#8217;m going to use to install nVidia drivers, I needed to disable SecureBoot in the BIOS. There is a workaround if you want to go through the trouble of self-signing the nVidia drivers &#8212; Google will point you in the right direction.

### 4. Install Linux

As the title of this article implies, I installed Fedora 31 from a LiveUSB image onto the laptop. A couple of notes here:

  * I let the installer auto-partition the empty space on the laptop drive
  * I encrypted the Linux installation because it&#8217;s good form

Left to it&#8217;s own devices, the LiveUSB installer detected the Windows partition and automatically included support for booting into Windows from the grub2 boot menu.

### 5. Hardware Compatibility Adjustments

Once the installation was done and I could boot into Linux, I spent a few days of trial and error trying to figure out the right adjustments for the Razer Stealth hardware. Here&#8217;s what I found:

**Suspend / wake fixes**  
This was the main, non-intuitive problem I ran into. Everything seemed cool with the Linux installation until I closed the laptop lid for the first time. When I opened it later, the laptop seemed to un-suspend but then a moment later it was trying to suspend again.

Later on, upgrading to Linux 5.4.7 exposed a problem with the nVidia driver (more on the nVidia driver below), but props to Major Hayden for finding a solution for that.

The fixes to both of these issues involve passing arguments to the kernel command line. As root, edit `/etc/default/grub` and find the line that starts with `GRUB_CMDLINE_LINUX`. To the end of the line of arguments assigned here, add `button.lid_init_state=open pcie_port_pm=off`.

After doing this, still as root, run `grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg` to force the regeneration of the grub config menus.

**Replace Nouveau with nVidia drivers**  
Following [the wisdom of Major Hayden][5], I enabled the RPMFusion yum repos and installed the `akmod-nvidia` drivers. This approach is really slick as it not only installs functional drivers, but it also does the blacklisting of the nouveau drivers for you.

**Control that fancy keyboard**  
As of the time of this post, there is a [pull request][6] making its way through the [OpenRazer][7] project&#8217;s code review process that will enable support for this laptop model. I&#8217;ve already set up the [Fedora repo][8] and installed the `openrazer-meta` package. When that PR is merged and the RPM is rebuilt, I&#8217;ll be able to tell my keyboard to pick a color and stick with it.

### And there you have it

Even though the steps ended up being pretty straightforward, a _lot_ of trial and error went into getting this all figured out. Hopefully this will be useful to the next person who decides to go large with a Razer Stealth GTX while going Linux at the same time!

 [1]: https://system76.com/
 [2]: https://starlabs.systems/
 [3]: https://puri.sm/
 [4]: https://support.razer.com/gaming-laptops/razer-blade-stealth-13-late-2019-gtx-model/
 [5]: https://major.io/2019/12/12/thinkpad-t490-fedora-install-tips/
 [6]: https://github.com/openrazer/openrazer/pull/962
 [7]: https://openrazer.github.io/
 [8]: https://openrazer.github.io/#download