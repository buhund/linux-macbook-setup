# How to enable AMDGPU (and thus Vulkan), mid-2015 rMPB15

Tech Specs: [MacBook Pro (Retina, 15-inch, Mid 2015)](https://support.apple.com/kb/SP719)
* MacBook Pro (Retina, 15-inch, Mid 2015)
* Model Identifier: MacBookPro11,4
* Part Number: MJLQ2xx/A
* CPU: Intel Core i7-4870HQ @ 2.50GHz
* GPU: AMD Radeon R9 M370X
* RAM: 16 GB


I found the solution to enable Vulkan on my Macbook Pro running Linux (Kubuntu 19.10), via a (Level1Techs forum post)[https://forum.level1techs.com/t/vulkan-with-amds-gcn-1-0/131427/32].

My setup is a "use entire disk" installation and UEFI (no Grub during doot). Therefore, it's not straightforward to add boot parameters, such as enabling AMDGPU (and thus be able to use Vulkan) instead of running Radeon.


Install Kernelstub: https://github.com/isantop/kernelstub

  You can either build it yourself, or grab a .deb package from the (release page)[https://github.com/isantop/kernelstub/releases]





`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a amdgpu.si_support=1`

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a radeon.si_support=0`

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a amdgpu.cik_support=1`

`sudo kernelstub --initrd-path /boot/initrd.img --kernel-path /boot/vmlinuz -a radeon.cik_support=0`
