---
title: "Need more light? Get a monitor."
date: 2022-02-08T09:17:47-05:00
draft: true

tags:
- blog
- about
keywords:
- tech blog
- blogging
- engineering
- hacking
- monitor mod
- lightpanel
- LED
- photography equipment
- cheap lightpanel
- DIY light panel
---

Since I am building a blog I figured I will need to improve my photo capabilities. And first thing that needs improvement is getting a good source of soft light. Checking what can be such a source You can find information that for basic light sources used in photography can be LED strips, LED panels and such. Since monitors have a strong backlight made with CFLs or more modern monitors with LEDs. Maybe we can use old, busted monitor to convert it into LED panel?

#### Preparing to hack.

First of all we need to do some research. It is with all hacks/ mods or any DIY projects really. We need to check if anyone already tried it and what information we can get from it. Quick google search will yield quite a few projects. From these we can learn about how we can approach the hack, what materials we probably need and some more details like how to dissasemble a monitor and do the mods. Without this reasearch our mod would take much more time. Armed with this knowledge we can plan next steps.

#### Setting up the goal for the mod.

Since we already know a bit what are the possibilities we can set a goal for our hack. My goal would be to take a monitor with LED backlight and convert it to LED light panel. I aim to use the powersupply built into monitor to power the LEDs. I do not need to regulate light intensity, for now it is enough for me to have it plugged into mains and shine with full power. We can consider what would be needed to add switching panel ON/OFF and regulating light intensity later.

#### A safety warning!

Before attempting this mod or anything similar be warned. Monitor power supply is mains operated. Mains is highly dangerous when incorrectly handled. If You are not educated to handle mains, consider asking someone with such knowledge for help. 

#### Getting to know what we are working with.

First step is to get a monitor for yourself to hack. The disassemble it and learn how it is built as every model is a bit different. You can try finding a video or other blog for detailed dissasembling proces for You model. General rule is that You take all the visible screws out and pry the plastic frame from panel with a plastic picks or flat screw driver. You obviosuly need to unplug the monitor first and maybe wait some time for capacitors to discharge properly. While dissasembling You may want to take a picture from time to time (just in case You want to assemble it back again correctly). Once You have the plastics taken off You are probably left with metal casing. Before moving it away from panel You should try to unplug every connector You can. To know if we have a panel with LED backlight it is usually enough to google its model number (it is usually on a sticker in upper part of panel). Once we confirm the backlight type we can take the metal casing with electronics and have a look.

Most of monitors are built similarly. They usually have a power supply board with back light driver, a logic board, small board with switches or capacitive touch buttons and a wide board connected to panel LCD. For our hack we will aim to leave only the power board with backlight driver but in order to pull it off we need to do some recon. 

#### Tour de monitor

I will start with checking out the backlight driver to see if I can force it to be on all the time. Usually The logic board sends commands to this driver in form of certain signals that control standby and light intensity. After googling out the part number of this chip I found only a one page manual for this XXXXXXX IC. It is very general, holds pinout and names for signals and a few sentences of functionality. It mentions that only that light intensity can be controlled via external PWM, external analog signal or some internal PWM. No word on how to know which one is used, or what are the PWM frequencies, voltage levels etc. Looking at the board there seem to be no usefull clues. Let's stop here and jump to connection to logic board. 

Connector for logic board is on the other side of power board. It holds a 7-pin connector and conveniently its signals are labeled on the PCB. We have 5V, GND, EN, PWM, MUTE, SOUND. Last two are routed to other connector which is missing from my board - it is not present in my model as it has no buit-in speakers. First two are power lines that are powering the logic board. And middle two seem to be controlling the backight driver, being EN - enable and PWM - signal that controlls light intenstiy. Quick check with a digital multimeter (DMM) confirms that these two lines trace to the backlight driver we started with. Neat so this is a good place to do the hack but there is a problem. How do we know what signals we need to supply to those two pins? Driver datasheet does not mention any details. We can check the logic board and see if its chip is better described. 

Logic boards heart is a YYYYYY. It is esay to find datasheet for that. It is a massive chip with all the capabilities to process video and drive LCD screens. We are interested in how it operates the backlight. Looking through its specification it may be difficult to find anything detailed as well. In this case it would be best to take out an oscilloscope and DMM and do the measuring our selves. A word of caution on that it requires You to measure things on live and working monitor but taken appart. There is a risk of shoking Yourself or your equipment with mains in case of some mistake. Since I do not have an oscilscope or a quiet place at home to perform such measurements I try some assumptions. 

#### Formulating the hack

Based on the done research on the monitor, let's assume that the backlight driver is actually driven by some logic level EN signal and a PWM of some unknow frequency. We do not know the voltage of the logic levels. It may be 5V or 3,3V or even 1,8 as YYYYYY spec mentions. I think we can rule out 1,8V as I think it is pretty low for a monitor. 5V is supplied by the power board to logic board, but the spec seems to mention operation on 3,3V and also mentions TTL levels for logic. TTL levels are basically a standard for when a signals is considered high or low and is based on voltages driven by transistors. It means that anything above 2,0V should be recognised as high logic level. I would assume the same for PWM signal and additionaly a constant high signal is basically the same as PWM with 100% duty, meaning 100% of backlight intensity. 

Now that we have some educated guesses as to what signals are needed to hack the backlight driver we can perform the hack. As mentioned we need a constant signal with voltage from 2,0-3,3V range. For this we will use the 5V which powerboard supplies to the logic board. We can get rid of a logic board and use a voltage divider to divide 5V into 2,5V with a divider of 1/2 ratio. For that I take two resistors with value 68k and solder them as in the picture (excuse the soldering). When soldering You may need to apply a bit of fresh solder to the joints of the connector to lower the melting point and allow easy soldering. This way we are providing around 2,5V to EN and PWM as soon as the power supply is connected to mains. It should be according to the goal we setup earlier. Providing it works of course.

Once soldering is complete we can try if that works, just remember to at least partially assemble the boards, metal casing and LED connectors which should be on lower and upper part of a panel. In my case it worked fine, once the mains were connected the LEDs turned on with what seemed to be full power. To finish the build we need to remove the LCD layer from the panel. It is done by carefuly lifting up the metal frames from the panel itself and once the frame is removed we can remove the LCD layer. Leave the other layers as they will difuse and direct the light evenly from the panel. Once that is done You can put everything back together with the exception of LCD layer and logic board which are not needed now.

Hope You enjoyed this writeup and me giving some thought process during such a hack. Care to make You own?