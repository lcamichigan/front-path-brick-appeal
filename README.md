# Front Path Brick Appeal

[![Build status](https://ci.appveyor.com/api/projects/status/mf73kfhxj00y5e9s?svg=true)](https://ci.appveyor.com/project/lcamichigan/front-path-brick-appeal)

This is a re-creation of resources for an appeal to repave the front path of
Sigma’s chapter house with personalized bricks.

## Contents

* [Getting Started](#getting-started)
* [Modifying an Inscription Font](#modifying-an-inscription-font)
* [Creating an InDesign File](#creating-an-indesign-file)
* [About the Letter](#about-the-letter)

## Getting Started

To edit the letter about the bricks, you need:

* [Adobe InDesign](https://www.adobe.com/products/indesign.html). Visit
  https://www.adobe.com/creativecloud/plans.html for more information.

* Letter.idml. The easiest way to get this file is to download it from
  https://ci.appveyor.com/project/lcamichigan/front-path-brick-appeal/build/artifacts,
  but you can also [create your own](#creating-an-indesign-file).

* These fonts:

  | Font                                                                                       | Download URL                                                        |
  |--------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
  | [Gillius](http://arkandis.tuxfamily.org/adffonts.html)                                     | http://arkandis.tuxfamily.org/fonts/Gillius-Collection-20110312.zip |
  | [Linux Libertine](http://libertine-fonts.org) (OpenType version)                           | http://mirrors.ctan.org/fonts/libertine.zip                         |
  | [Zapf Humanist Greek](https://www.paratype.com/btstore/fonts/Zapf-Humanist-Greek.htm) Demi | N/A                                                                 |

## Modifying an Inscription Font

Brick inscriptions sometimes included left and right quotation marks (“&nbsp;”)
and often the Greek letters Λ and Σ. However, the systems that actually inscribe
bricks may require that inscriptions use only
[ASCII](https://en.wikipedia.org/wiki/ASCII) characters. One way to permit using
non-ASCII characters in inscriptions is to modify a font to map ASCII characters
that don’t appear in inscriptions to glyphs of non-ASCII characters that do. On
macOS, you can modify a font in this way using freely-available tools.

First, you need to download and install OS&nbsp;X Font Tools. To download an
installer for OS&nbsp;X Font Tools, visit
https://developer.apple.com/download/more/?=OS%20X%20Font%20Tools (you’ll need
to log in using an Apple Developer account).

The OS&nbsp;X Font Tools installer doesn’t work on OS&nbsp;X El Capitan 10.11
and later (see
https://stackoverflow.com/questions/7535498/uibutton-custom-font-vertical-alignment/8314197#answer-8314197
for more information). To install OS&nbsp;X Font Tools on recent versions of
macOS, you must extract files from the installer (OS&nbsp;X Font Tools.pkg)
manually. To do this, enter in Terminal

```sh
xar -xf 'OS X Font Tools.pkg'
cd fonttoolbox.pkg
gunzip --decompress --stdout Payload | cpio -i
sudo mv FontToolbox.framework /Library/Frameworks
cd ../fontTools.pkg
gunzip --decompress --stdout Payload | cpio -i
mv ftxdumperfuser /usr/local/bin
```

You should now be able to modify fonts. To make things easier, define a variable
for the path to the font you’re modifying (for example,
[Zapf Humanist Greek](https://www.paratype.com/btstore/fonts/Zapf-Humanist-Greek.htm)
Demi):

```sh
fontPath=~/Library/Fonts/Bitstream\ -\ ZapfHumnstGrk\ Dm\ BT\ Demi.ttf
```

Before modifying the font, make a back-up copy of it:

```sh
mkdir support
cp "$fontPath" support
```

Dump the font’s character-to-glyph mapping to a file named cmap.xml:

```sh
ftxdumperfuser --table cmap --output cmap.xml "$fontPath"
```

Modify cmap.xml. For example, running

```sh
echo -e '
87,89c87,89
< \t\t<map charValue="0x0027" glyphRefID="10"/>
< \t\t<map charValue="0x0028" glyphRefID="11"/>
< \t\t<map charValue="0x0029" glyphRefID="12"/>
---
> \t\t<map charValue="0x0027" glyphRefID="107"/>
> \t\t<map charValue="0x0028" glyphRefID="108"/>
> \t\t<map charValue="0x0029" glyphRefID="109"/>
109c109
< \t\t<map charValue="0x003D" glyphRefID="32"/>
---
> \t\t<map charValue="0x003D" glyphRefID="111"/>
142c142
< \t\t<map charValue="0x005E" glyphRefID="65"/>
---
> \t\t<map charValue="0x005E" glyphRefID="157"/>
171c171
< \t\t<map charValue="0x007B" glyphRefID="94"/>
---
> \t\t<map charValue="0x007B" glyphRefID="164"/>
295,297c295,297
< \t\t<map charValue="0x0027" glyphRefID="10"/>
< \t\t<map charValue="0x0028" glyphRefID="11"/>
< \t\t<map charValue="0x0029" glyphRefID="12"/>
---
> \t\t<map charValue="0x0027" glyphRefID="107"/>
> \t\t<map charValue="0x0028" glyphRefID="108"/>
> \t\t<map charValue="0x0029" glyphRefID="109"/>
317c317
< \t\t<map charValue="0x003D" glyphRefID="32"/>
---
> \t\t<map charValue="0x003D" glyphRefID="111"/>
350c350
< \t\t<map charValue="0x005E" glyphRefID="65"/>
---
> \t\t<map charValue="0x005E" glyphRefID="157"/>
379c379
< \t\t<map charValue="0x007B" glyphRefID="94"/>
---
> \t\t<map charValue="0x007B" glyphRefID="164"/>
' | patch cmap.xml
```

will map ASCII characters to glyphs of non-ASCII characters like this:

Character | Code Point | Glyph
----------|------------|------
'         | U+0027     | ’
(         | U+0028     | “
)         | U+0029     | ”
=         | U+003D     | –
^         | U+005E     | Λ
{         | U+007B     | Σ

Finally, modify the font to use the new character-to-glyph mapping:

```sh
ftxdumperfuser --table cmap --datafile cmap.xml "$fontPath"
```

## Creating an InDesign File

Creating an InDesign IDML file from the files in this repository requires the
free [Zip](http://www.info-zip.org/Zip.html) utility. To install Zip on Windows:

1. Click ftp://ftp.info-zip.org/pub/infozip/win32/zip300xn-x64.zip to download
   zip300xn-x64.zip.

2. Right-click zip300xn-x64.zip, choose Extract All, and then click Extract to
   extract a folder named zip300xn-x64.

3. Right-click zip300xn-x64.zip in the zip300xn-x64 folder you just extracted,
   choose Extract All, and then click Extract to extract another folder named
   zip300xn-x64.

4. Move this second zip300xn-x64 folder to C:\Program Files.

Zip is included with macOS.

To create an InDesign file, first download this repository as a ZIP archive. To
do this, click
[here](https://github.com/lcamichigan/front-path-brick-appeal/archive/master.zip).
Unzip the archive wherever you wish. Then, `cd` to the
[Letter IDML](Letter%20IDML) folder and enter in PowerShell

```powershell
& "$env:ProgramFiles\zip300xn-x64\zip" -X0 ..\Letter.idml mimetype
& "$env:ProgramFiles\zip300xn-x64\zip" --recurse-paths --no-dir-entries -X9 ..\Letter.idml * --exclude mimetype
```

or in Command Prompt

```batch
"%ProgramFiles%\zip300xn-x64\zip" -X0 ..\Letter.idml mimetype
"%ProgramFiles%\zip300xn-x64\zip" --recurse-paths --no-dir-entries -X9 ..\Letter.idml * --exclude mimetype
```

or in Terminal

```sh
zip -X0 ../Letter.idml mimetype
zip --recurse-paths --no-dir-entries -X9 ../Letter.idml * --exclude *.DS_Store mimetype
```

## About the Letter

The letter includes an embedded image of a cross and crescent created using a
[cross-and-crescent package](https://github.com/lcamichigan/cross-and-crescent)
in LaTeX. To create this image,
[install](https://github.com/lcamichigan/cross-and-crescent#installing) the
cross-and-crescent package, and then enter in PowerShell or Terminal

```sh
latex -jobname logo -output-format pdf '\documentclass{standalone}\usepackage{cross-and-crescent}\begin{document}\begin{tikzpicture}[scale=36bp/8cm]\crossAndCrescentSetMacros\draw[line join=round,line width=1bp]\crossAndCrescentPath\end{tikzpicture}\end{document}'
```
