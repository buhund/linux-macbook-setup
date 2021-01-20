# Setup for Linux on a mid-2015 Macbook Pro

My setup for running Linux on a 15" Macbook Pro (mid-2015)

Tech specs: [MacBook Pro (Retina, 15-inch, Mid 2015)](https://support.apple.com/kb/SP719)
* MacBook Pro (Retina, 15-inch, Mid 2015)
* Model Identifier: MacBookPro11,4
* Part Number: MJLQ2xx/A
* CPU: Intel Core i7-4870HQ @ 2.50GHz
* GPU: AMD Radeon R9 M370X, 2GB GDDR5
* RAM: 16 GB

Distro of choice: [Kubuntu](https://kubuntu.org/)



### Fixing AirPods 2 dropping out
From the Wizards of Arch (Link)

Choppy/distorted sound

This can result from an incorrectly set sample rate. Try the following setting:

/etc/pulse/daemon.conf

avoid-resampling = yes #(Needs PA11 or higher)
default-sample-rate = 48000
