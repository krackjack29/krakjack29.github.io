---
title: 'Cascadia Code!! A beautiful font for developers'
header: 
    image: /assets/images/cascadia-code.png
    caption: "Cascadia Code"
comments: true
date : 2019/09/24
categories:
    - Tech
tags:
    - fonts
    - microsoft
    - tools
---

Microsoft recently shipped their new monospaced font called 'Cascadia', and this is the same font that MS ships with its new [Terminal](https://github.com/microsoft/terminal). If you haven't used the new [Terminal](https://github.com/microsoft/terminal) yet, please do!!! It supports multi tabs, copy paste and also customization to create your own tabs. 

I fell in love with cascadia as soon I looked at it in the terminal. Now that microsoft has shipped it, it has become by default font for all the code editors that I use i.e Best IDE in the world Visual Studio Code and not so good anymore Visual Studio. 

Cascadia code supports ligatures too, which are great for glyphs which combine characters to create symbols. like the following

![Glyph](/assets/images/glyph.gif)

## Install Cascadia

You can download Cascadia directly from github.
[https://github.com/microsoft/cascadia-code/releases](https://github.com/microsoft/cascadia-code/releases).

## Cascadia in VS Code

1. Open the VS Code settings.  File -> Preferences -> Settings
2. Look for the font settings. User -> Font
3. Add *'Cascadia Code'* to the font family, to enable ligatures check the 'Enable/Disables font ligatures' check box.

![VS Code settings](/assets/images/cascadia/vscode.png)

## Cascadia in Visual Studio

1. Open Visual studio settings. Tools -> Options
2. Open the Fonts and Colors section. Environment -> Fonts and Color
3. Select Cascadia Code from the drop down list.

![Visual Studio settings](/assets/images/cascadia/vs.png)

## Jekyll and Cascadia code

As you can see in my other blogs, the code blocks are using Cascadia font. Follow the below steps to integrate Jekyll (with minimal mistakes theme) to use Cascadia font in the code blocks or even as universal font for your website.

1. Create a webfont using [Webfont generator](https://www.fontsquirrel.com/tools/webfont-generator).
2. Copy the 'woff' and 'woff2' files to your assets folder. I have copied it to /assets/fonts/cascadia folder. 
3. Update the _variables.scss to load the webfont and add the following code

```css
/* install cascadia font */
@font-face {
   font-family: 'cascadia_coderegular';
   src: url('/assets/fonts/cascadia/cascadia-webfont.woff2') format('woff2'),
        url('/assets/fonts/cascadia/cascadia-webfont.woff') format('woff');
   font-weight: normal;
   font-style: normal;

}

/* system typefaces */
$cascadia: 'cascadia_coderegular',-apple-system, BlinkMacSystemFont, "Roboto", "Segoe UI",
   "Helvetica Neue", "Lucida Grande", Arial, sans-serif !default;
```

If you want to update only the code block just update the $monospace variable to pick $cascadia as the default font.

```scss
$monospace: $cascadia, Monaco, Consolas, "Lucida Console", monospace !default;
```

I am so much in love with Cascadia font that even my notepad uses the same font. And Thank you Microsoft!!!
Happy Coding!!!