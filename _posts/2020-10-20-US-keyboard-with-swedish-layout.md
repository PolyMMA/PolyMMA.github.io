---
title: US keyboard with Swedish soft layout
author: D.D.
date: 2020-10-20 01:00:00 +02:00
categories: [Tutorials, Linux]
tags: [customizing]     # TAG names should always be lowercase
---

Recently I have started to code more than usual and finally gave in to the trend and got myself a mechanical keyboard. It landed on a [Royal Kludge 71](http://www.rkgaming.com/en-US/article.php?id=111) (budget version of [Calibur V2 TE RGB](https://www.drevo.net/product/keyboard/calibur-v2-te)?), a mechanical Bluetooth 71% keyboard. All'n'all, pretty good stuff for a good price but there were some deviating issues.

Anywho... Thing is that the [layout](https://en.wikipedia.org/wiki/Keyboard_layout#Physical_layouts) is ANSI-INCITS, while in it is more common in Europe to use the ISO/IEC layout. But it is just the annotation on the keys, right? No big deal, just don't look at it... Well, that is true and for the most part it is OK. The issues arise when you use another layout than the one the physical layout is adjusted for. Especially an ISO layout on an ANSI physical keyboard. There is a key missing...

Take a look at this [comparison image](https://upload.wikimedia.org/wikipedia/commons/b/b2/Physical_keyboard_layouts_comparison_ANSI_ISO_KS_ABNT_JIS.png) and compare the ANSI and the ISO. You will notice that, besides the layout difference of the keys in the **Return** region, the ANSI keyboard lacks a button to the left of **Z**. In the ISO layout this button holds the three  **\< \> \|** symbols which are quite nice to have if you are coding. 

One natural way to solve this is of course to stick to the US keyboard layout and everything is in its place to begin with, but since I am in Sweden I am in dire need of **Å Ä Ö** for daily communication and also need the **\< \> \|** for professional and semi-professional reasons, a key re-mapping is in order. 

This tutorial is for the Linux OS, and has only been tested on Ubuntu 18.04 (Debian) and Manjaro 19 (Arch), though it is probably applicable to other Linux flavors.


Using a US keyboard with Swedish layout
=======================================

In order to facilitate this in a fairly OK way I decided to remap these symbols as 3rd function on the **, . -** keys. Having one physical key for each symbol is a significant improvement over the original ISO standard where I never remember which of the **\< \>** is **Shift**- and not. 

Add the custom layout
=====================

Open the following file `/usr/share/X11/xkb/symbols/se` add add the following piece of code as a modification of the basic Swedish keyboard layout. Does not really matter where you place the code. At the bottom is fine. 

```bat
// Swedish US_keyMod
partial alphanumeric_keys
xkb_symbols "se_uskeymod" {

    include "se(basic)"

    name[Group1]="Swedish (US key-mod)";

    key <AB08> { [     comma,  semicolon, less,  dead_ogonek ] };
    key <AB09> { [    period,   colon, greater, dead_abovedot ] };
    key <AB10> { [     minus, underscore, bar, dead_abovedot ] };
};
```

Update rules with the new layout and name
=========================================

Then locate and open `/usr/share/X11/xkb/rules/evdev.xml` and locate the `<variantList>` of the Swedish layout. Observe the code layout and notice how the different variants are structured. Squeeze in a reference to your latest variant:
```xml
        <variant>
          <configItem>
            <name>se_uskeymod</name>
            <description>Swedish (US key-mod)</description>
          </configItem>
        </variant>
```


Update the layout list so that the listing is correct in GUI
------------------------------------------------------------

Open the file  `/usr/share/X11/xkb/rules/evdev.lst` and find the line `! variant`. Look within its tree structure to find Swedish layout variants. When you find them squeeze in the following line somewhere there

```yaml
se_uskeymod	  se: Swedish (US key-mod)
```

Verify the result
=================

Verify intended result with 
```console
$ gkbd-keyboard-display -l "se(se_uskeymod)"
```
final flag stands for `layout(variant)`. The **AltGr**-mod of keys **, . -** should generate symbols **\< \> \|** in text.


And finally...
--------------

Save, close, change keyboard layouts, (maybe log out, reboot or similar?... don't remember...)


Optionally...
=============

....add SWERTY layout from [Johan Gustavsson](http://johanegustafsson.net/projects/swerty/). Exists for Win, Mac, Linux



