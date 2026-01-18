---
title: Licensing
aliases:
  - /Licensing
  - /licensing
  - /content-dev/licensing
---

# Licensing

A considerable number of Luanti-based games have appeared in the Android play store and Google Play. See the [list of Luanti forks](/about/luanti-forks) for examples of these.

_Please look at: [FlightGear](http://www.flightgear.org/flightprosim.html) as they have the same issue as Luanti has._

## Complying with the License

The code of both the Luanti engine and Minetest Game are licensed under [LGPL 2.1+ free software license](https://www.gnu.org/licenses/lgpl-2.1.html). Other components may be similarly licensed, however this document does not cover them. **Note that this document is only intended for informational purposes**, for the real legally binding terms you should read the [LGPL 2.1+ license](https://www.gnu.org/licenses/lgpl-2.1.html).

- You must link to the source code behind your software
  - Which must also be licensed under LGPL 2.1 or later, or a compatible license. Also see _"what if I use proprietary source code/libraries?"_.
  - If any modifications were made to the Luanti engine or Minetest Game, you must state this inside of your application, and provide a means to download the \*\*modified\*\* source code.
  - If you did not modify any source code, you must still link to the source code behind the software - however this can be the official Luanti repo if unmodified
- You must not remove any copyright notices.
- You must state significant changes to the software.
- You must not mix proprietary and LGPLv2.1+ code, see _"what if I use proprietary source code/libraries?"_.
- You should place the following attribution on any pages where the software can be downloaded, including but not limited to Google Play or a website:

> This [app/game/...] uses the Luanti engine [and Minetest Game], Copyright 2010-2018 Perttu Ahola and contributors, licensed under LGPLv2.1+

**It is an infringement to only link back to the original source code if you made modifications**

### Where should I put any links?

- Any locations where the software is downloaded, including but not limited to Google Play or a website.
- You should place a link in the main menu (for example, in the credits tab).

### What if I use proprietary source code/libraries?

- **Don't mix proprietary code with LGPL code.** For example, if you use a proprietary ad library, then you can only call it from proprietary code.
- **Any LGPL code must be replaceable.** The user must be able to use their own version of any LGPL code with your software. You must also provide documentation on how to do this.
- Any proprietary code you wrote must not forbid reverse-compilation for the purpose of debugging modifications to the LGPL code.
- You must **fully and prominently attribute** the Luanti project if your software contains any proprietary code.

This, in affect, means that you will need to completely rewrite the Java section of the app and just use Luanti as an NDK library. Make sure you include instructions on how to replace any LGPLv2.1+ code.

The legal text behind this is in [section 4](https://www.gnu.org/licenses/lgpl.html#section4). Also see [\[1\]](https://opensource.stackexchange.com/questions/4357/how-can-lgpl-and-proprietary-licenses-be-combined)

### See also

- [https://tldrlegal.com/license/gnu-lesser-general-public-license-v3-(lgpl-3)](<https://tldrlegal.com/license/gnu-lesser-general-public-license-v3-(lgpl-3)>)
- [https://www.gnu.org/licenses/lgpl-2.1.html](https://www.gnu.org/licenses/lgpl-2.1.html)
- [https://softwareengineering.stackexchange.com/questions/86142/what-exactly-do-i-need-to-do-if-i-use-a-lgpl-licenced-library](https://softwareengineering.stackexchange.com/questions/86142/what-exactly-do-i-need-to-do-if-i-use-a-lgpl-licenced-library)

## What to do if I spot a program that is possibly infringing Luanti's license?

If you think that a certain program is infringing on Luanti's LGPL 2.1 license, please check with the program's website to see if it is licensed under similar terms, and if the source code is being distributed. If the license seems to be proprietary, and if the developer alleges that the program is their own work, then contact [celeron55](mailto:celeron55@gmail.com). Alternatively, join the [Luanti IRC channel](https://web.libera.chat/?channels=#luanti) and report it there. It doesn't matter if anyone replies or not, it will be logged and the community will check on it themselves. There is also an attempt to list forks of Luanti at the "[Making a list of Luanti forks for Android](https://forum.luanti.org/viewtopic.php?p=242219#p242219)" thread in the forums. If the program you discovered is missing there you could add it.

### What not to do if a program is infringing Luanti's license

**DO NOT TAKE ANY ACTION AGAINST THE DEVELOPER** - You must report it to the Luanti devs instead.

Even if it is as clear as day that a certain program is using code and artwork from Luanti and is not distributing it under similar conditions, please do not attempt, or do the following:

1.  Do not launch a witch-hunt. This is the 21st century. Please behave appropriately.
2.  Do not attempt to crack, grief, or crash their servers.
3.  Do not attempt to contact the infringing program's developer. Any contact made with the developer shall be after consensus with respected members of the community at large and the Core Development Team.

The above has largely only resulted in a lot of drama, mostly ending with the infringing developer gaining the upper hand and getting away with using Luanti's code while violating the license. Please don't do it.
