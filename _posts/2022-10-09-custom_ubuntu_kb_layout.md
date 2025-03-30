---
title: How to set up custom keyboard mapping in Ubuntu 20.04/22.04/24.04
date: 2022-10-09 13:00:00 +0800
categories: [How To]
tags: [ubuntu, guide]     # TAG names should always be lowercase
---

This guide is a quick explainer on how to add custom keyboard mappings to the standard US keyboard layout in Ubuntu 20.04/22.04/24.04 using left-alt as a layer key.

This is based on: <https://help.ubuntu.com/community/Custom%20keyboard%20layout%20definitions?action=show&redirect=Howto%3A+Custom+keyboard+layout+definitions>

There are two files to edit
- /usr/share/X11/xkb/symbols/us
- /usr/share/X11/xkb/rules/evdev.xml


## In file "/usr/share/X11/xkb/symbols/us":
After the first layout "basic" after line 57 and before the next layout add:
```
partial alphanumeric_keys
xkb_symbols "custom" {

    include "us(basic)"
    name[Group1]= "English (US, custom left alt layer)";

    key <AD07> {	[	  u,	U, grave, asciitilde		]	};
    key <AD08> {	[	  i,	I, minus, underscore		]	};
    key <AD09> {	[	  o,	O, equal, plus		]	};
    key <AD10> {	[	  p,	P, backslash, bar		]	};
    key <AC07> {	[	  j,	J, Left, Left		]	};
    key <AC08> {	[	  k,	K, Down, Down		]	};
    key <AC09> {	[	  l,	L, Up, Up		]	};
    key <AC10> {	[ semicolon,	colon, Right, Right		]	};
    key <AB06> {	[	  n,	N, bracketleft, braceleft		]	};
    key <AB07> {	[	  m,	M, bracketright, braceright		]	};
    key <AB08> {	[     comma,	less, Home, Home		]	};
    key <AB09> {	[    period,	greater, End, End		]	};
    key <AB10> {	[     slash,	question, Delete, Delete	]	};
    key <AC11> {	[ apostrophe, quotedbl, BackSpace, BackSpace	]	};

    include "level3(lalt_switch)"
};
```

## In file "/usr/share/X11/xkb/rules/evdev.xml":
Find (should be line 1334):
```
<layoutList>
  <layout>
    <configItem>
      <name>us</name>
      <!-- Keyboard indicator for English layouts -->
      <shortDescription>en</shortDescription>
      <description>English (US)</description>
```

After the `<variantList>` line, add our own custom variant:
```
<variant>
  <configItem>
    <name>custom</name>
    <description>English (US, custom left alt layer)</description>
  </configItem>
</variant>
```

## ~~In terminal (not confirmed if required)~~

~~Run~~
```
sudo dpkg-reconfigure xkb-data
```

## Reboot
Finally reboot after

## Select keyboard and delete the default

In settings, go to keyboard tab.

Under "Input Sources" click the "+" button to add a new keyboard.

Select "English (United States)".

Then scroll down to find the `<description>` provided in the evdev.xml file. In this case it's "English (custom left alt layer)".

Delete the default keyboard from "Input Sources" to let it take effect.
