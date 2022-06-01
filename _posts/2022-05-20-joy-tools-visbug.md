---
layout: post
title: Joy Tools - VisBug
categories: [JavaScript, VS Code, HTML, CSS]
---
The Google Chrome Dev YouTube channel recently posted video detailing a lot of exciting CSS features that will help developers exercise more control over the styling, but the one that really caught my attention was the `color-contrast()` property that is [currently (May 2022) unsupported by *any* browsers](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/color-contrast). 

Watch it [here.](https://www.youtube.com/watch?v=Xy9ZXRRgpLk&t=636s) 

Even if we only consider the aesthetic experience of a nonexistent "average" user, proper contrast between foreground text and background elements can be a tricky challenge even for experienced designers, but proper contrast acquires a new mandate when we consider the need to make our sites and apps as accessible as possible by adhering to [WCAG guidelines](https://www.w3.org/TR/WCAG21/). 

> [Understanding Web Accessibility Color Contrast Guidelines and Ratios - CSS Tricks](https://css-tricks.com/understanding-web-accessibility-color-contrast-guidelines-and-ratios/)

Since we can't currently use this cool feature, the issue of contrast has been a relentless thought worm for me, which in turn led me to the subject of this post: 

### VisBug

* VisBug extension for Chrome: [https://chrome.google.com/webstore/detail/visbug/cdockenadnadldjbbgcallicgledbeoc?hl=en](https://chrome.google.com/webstore/detail/visbug/cdockenadnadldjbbgcallicgledbeoc?hl=en)
* VisBug playground/tutorial: [https://visbug.web.app/](https://visbug.web.app/)
* VisBug GitHub repository: [https://github.com/GoogleChromeLabs/ProjectVisBug](https://github.com/GoogleChromeLabs/ProjectVisBug)

Like a lot of web developers, when I'm working on the front end, I spend a gigantic chunk of time in my browser's dev tools looking at my elements/layouts tabs and combing through the output to chase down styles and classes on my UI elements. While that is still necessary, of course, VisBug puts common use cases right at the touch of my fingertips with a few keystrokes, obviating the need for all the intermediary steps I'd otherwise use to find out style information. 

At first glance, the functions that drew me to this extension are quite simple:
* On any webpage, press `Shift+Alt+D` to slide out the VisBug control panel. 
> nb. Some sites override this keyboard shortcut, notably YouTube. I discovered this when I was putzing around the internet looking to try VisBug on Just Any Random Site. Anyway, it has to do with the page in question, not VisBug. 
* **Guides - G** - With the panel open, press the hotkey `G` to see the page guides. With this option active, VisBug will draw the box outline for any element you hover your mouse over, and if you click on an element, it will display measurements based around the selected element and the box element you then mouse over. 
[!](https://i.imgur.com/1S6HzBm.png)i
* **Inspect - I** - With the panel open, press the hotkey `I` to inspect the element under your cursor, which will show a lovely little tooltip with the styles applied to that element. Clicking an element will pin the tooltip for that element to the screen as an overlay while the mouse-over tooltips keep popping up for other elements you run mouse over. 
[!](https://i.imgur.com/uAskOJf.png)
* **Accessibility - X** - With the panel open, press the hotkey `X` to see the text color and background color of the element under the mouse cursor, as well as the WCAG contrast rating! 
[!](https://i.imgur.com/Q59INoz.png)

This list barely scratches the surface of what VisBug can do. I've only touched on what it can *observe*, but it has a wealth of commands to change elements on the DOM without adjusting them from the dev tools. While there's a bit of a learning curve for the controls, the playground (linked above and here) is maybe one of the coolest interactive tool tutorials I've seen in years, and with about a dedicated half hour you'll be whizzing through the VisBug hotkeys in no time to not only inspect page elements but experiment with layout adjustments on the fly, all without ever opening your dev tools! 

Happy coding! 
