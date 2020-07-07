# Enable AMDGPU (and use Vulkan), mid-2015 rMPB 15"

### Using kernelstub to edit your boot and kernel parameters to use AMDGPU and Vulkan

Tech specs for [MacBook Pro (Retina, 15-inch, Mid 2015)](https://support.apple.com/kb/SP719):
* MacBook Pro (Retina, 15-inch, Mid 2015)
* Model Identifier: MacBookPro11,4
* Part Number: MJLQ2xx/A
* CPU: Intel Core i7-4870HQ @ 2.50GHz
* GPU: AMD Radeon R9 M370X
* RAM: 16 GB


I found the solution to enable Vulkan on my Macbook Pro running Linux (Kubuntu 19.10), via a [Level1Techs forum post](https://forum.level1techs.com/t/vulkan-with-amds-gcn-1-0/131427/32).

My setup is a "use entire disk" installation and UEFI (no Grub during doot). Therefore, it's apparantly not straightforward to add boot parameters, such as enabling AMDGPU (and thus be able to use Vulkan) instead of running Radeon.

_____

**Before starting, I would urge you to take a backup of your files, in case something goes awry. At least, that's my gut reaction when doing something with which touches on anything with "boot" or "kernel". Better safe than sorry** ¯\\\_(ツ)\_/¯

_____

Use `sudo lshw -c video` to check what driver you're using:

    *-display                 
    description: VGA compatible controller
    product: Venus XT [Radeon HD 8870M / R9 M270X/M370X]
    vendor: Advanced Micro Devices, Inc. [AMD/ATI]
    physical id: 0
    bus info: pci@0000:01:00.0
    version: 83
    width: 64 bits
    clock: 33MHz
    capabilities: pm pciexpress msi vga_controller bus_master cap_list rom
    configuration: driver=amdgpu latency=0
    resources: irq:56 memory:80000000-8fffffff memory:b0c00000-b0c3ffff ioport:3000(size=256) memory:b0c40000-b0c5ffff

Line 11 (next to last) show that I'm currently running amdgpu (previously it was showing radeon).

Install [Kernelstub](https://github.com/isantop/kernelstub):

* You can either build it yourself (as detailed on the kernelstub github page), or grab a .deb package from the [release page](https://github.com/isantop/kernelstub/releases).

When running `kernelstub amdgpu.si_support=1`, as per the L1T thread, it will ask for sudo, and path to kernel and initrd image.

* Kernel image: `/boot/vmlinuz`
* Initrd image: `/boot/initrd.img`

After some trial and error, I found the commands supplied under to work. Copy, paste and run each line, one at a time.

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a amdgpu.si_support=1`

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a radeon.si_support=0`

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a amdgpu.cik_support=1`

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a radeon.cik_support=0`

**Reboot.** When I rebooted, it somehow crapped out when starting over, ending up with just a black screen. It might have been just me being impatient, but I just held down the power button, forcing a shutdown, then turning it on again when the Apple-light had died.

Run `sudo lshw -c video` again to see if it worked.

____

Update 2020-07-07

## [Level1Techs](https://forum.level1techs.com/t/vulkan-with-amds-gcn-1-0/131427/30) quotes

### imrazor

In order to get amdgpu support on my GCN 1.1 laptop I had to put the following line in /etc/default/grub:

    GRUB_CMDLINE_LINUX_DEFAULT=“radeon.si_support=0 amdgpu.si_support=1 radeon.cik_support=0 amdgpu.cik_support=1 rd.driver.blacklist=radeon quiet”

If you’re still using an apt or Debian based distro, you’ll then want to run “sudo update-grub” to update grub AND your intial ramdisk.

Note that both the amdgpu AND radeon drivers will still load, but you can confirm that the amdgpu driver has taken ownership of your GPU (look for “Kernel driver in use:”) by running “lspci -knn” and examining which kernel module is listed for your GPU.

Another kernel option that might be helpful to add to /etc/default/grub is:

    radeon.modeset=0

### znaque

Hello guys. Thank you for all this awesome information. It really helped me understand the problem.

Sadly you didn’t find a solution to this problem, but I did!!

You are right that Pop_OS is based on Ubuntu, but it actually DOESN’T use grub at all.

So the kernel flags you’re using `amdgpu.si_support=1 radeon.si_support=0 amdgpu.cik_support=1 radeon.cik_support=0` aren’t recognized by Pop_OS.

To properly load them you have to do the following in your command line:

kernelstub -a KERNELFLAG for all the 4 kernel flags. So to do the desired task do the following (enter after every line):

   kernelstub -a amdgpu.si_support=1
   kernelstub -a radeon.si_support=0
   kernelstub -a amdgpu.cik_support=1
   kernelstub -a radeon.cik_support=0

Then restart.

Hope this helps!

I’m now running Vulkan on my GCN 1.0 HD7970.

Again thank you for all this good info in this thread!
