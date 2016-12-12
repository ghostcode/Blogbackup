title: 'dp&sp&dpi&pt'
date: 2016-12-11 20:10:23
categories:
tags:
---

dp

>	Density-independent Pixels - An abstract unit that is based on the physical density of the screen. These units are relative to a 160 dpi (dots per inch) screen, on which 1dp is roughly equal to 1px. When running on a higher density screen, the number of pixels used to draw 1dp is scaled up by a factor appropriate for the screen's dpi. Likewise, when on a lower density screen, the number of pixels used for 1dp is scaled down. The ratio of dp-to-pixel will change with the screen density, but not necessarily in direct proportion. Using dp units (instead of px units) is a simple solution to making the view dimensions in your layout resize properly for different screen densities. In other words, it provides consistency for the real-world sizes of your UI elements across different devices.

sp
>	Scale-independent Pixels - This is like the dp unit, but it is also scaled by the user's font size preference. It is recommend you use this unit when specifying font sizes, so they will be adjusted for both the screen density and the user's preference.

pt
>	Points - 1/72 of an inch based on the physical size of the screen, assuming a 72dpi density screen.

px
>	Pixels - Corresponds to actual pixels on the screen. This unit of measure is not recommended because the actual representation can vary across devices; each devices may have a different number of pixels per inch and may have more or fewer total pixels available on the screen.

mm
>	Millimeters - Based on the physical size of the screen.

in
>	Inches - Based on the physical size of the screen.

单位换算

https://www.w3.org/Style/Examples/007/units.en.html

>1in = 2.54cm = 25.4mm = 72pt = 6pc
