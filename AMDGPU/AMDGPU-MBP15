# Enable AMDGPU (and use Vulkan), mid-2015 rMPB 15"

## Option 1: Using kernelstub to edit your boot and kernel parameters to use AMDGPU and Vulkan

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
