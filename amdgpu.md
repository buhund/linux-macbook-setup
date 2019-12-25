# How to enable AMDGPU (and thus Vulkan), mid-2015 rMPB15

Tech Specs: [MacBook Pro (Retina, 15-inch, Mid 2015)](https://support.apple.com/kb/SP719)
* MacBook Pro (Retina, 15-inch, Mid 2015)
* Model Identifier: MacBookPro11,4
* Part Number: MJLQ2xx/A
* CPU: Intel Core i7-4870HQ @ 2.50GHz
* GPU: AMD Radeon R9 M370X
* RAM: 16 GB


I found the solution to enable Vulkan on my Macbook Pro running Linux (Kubuntu 19.10), via a [Level1Techs forum post](https://forum.level1techs.com/t/vulkan-with-amds-gcn-1-0/131427/32).

My setup is a "use entire disk" installation and UEFI (no Grub during doot). Therefore, it's not straightforward to add boot parameters, such as enabling AMDGPU (and thus be able to use Vulkan) instead of running Radeon.

**Before beninning, I would urge you to backup your files, in case something goes awry. At least, that's my gut reaction when doing something with boot or kernel stuff. Better safe than sorry ¯\_(ツ)_/¯**

Use `sudo lshw -c video` to check what driver you're using

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


[Install Kernelstub](https://github.com/isantop/kernelstub)

* You can either build it yourself, or grab a .deb package from the [release page](https://github.com/isantop/kernelstub/releases).



To change boot parameters, use the following commands. Input a line, press enter, input the next, und so weiter.

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a amdgpu.si_support=1`

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a radeon.si_support=0`

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a amdgpu.cik_support=1`

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a radeon.cik_support=0`

**Reboot.** When I rebooted, it somehow crapped out when starting over, ending up with just a black screen. It might have been just me being impatient, but I just helt down the power button, forcing a shutdown, then turning it on again when the Apple-light had died.

Run `sudo lshw -c video` again to see if it worked.
