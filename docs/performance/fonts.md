---
sidebar_label: 'How to make fonts more performant'
sidebar_position: 1
---


1. Use .woff files not .ttf.
2. Use Font subsetter to get a subset of the needed fonts.
    https://everythingfonts.com/subsetter
3. Always add 2 back up fonts. https://www.cssfontstack.com/


## Use .woff not .ttf
True Type Font (ttf) files take much more space than woff files do. 

The book Sustainable Web Design by Tom Greenwood talks about how converting one of their font files from .ttf to .woff reduced the size by 212kb.

There are several font standards used on a site. There will usually be one for regular, italics, and bold at a minimum. 


## Use Subsets of fonts

A font face will come with many more symbols than a website will probably need. It might have the needed characters to support Cyrillic, Russian, Greek, Devanagari and many more.

Developers should evaluate the needs of the site that they are building and remove the extraneous characters if possible.

Removing extra characters can take a font fron 80kb to 20kb.

Devs can use https://everythingfonts.com/subsetter to achieve this.

Generally, picking 'Basic Latin' and 'Advanced Punctuation' will be enough for English language sites.

## Back up fonts.

Fonts might not always load, or they might be slow to load. In this case, we still want users to be able to view the text. So, to allow for this we declare atleast 2 back up fonts. Backup fonts must be fonts that are already pre-installed in many operating systems.

Devs should pick backup fonts from this list:
https://www.cssfontstack.com/

Fonts like Georgia, Times New Roman and Arial that have above 90% adoption in both Mac and Windows are a good choice.

