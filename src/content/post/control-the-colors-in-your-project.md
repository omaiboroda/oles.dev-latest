---
title: 'Control the colors in your project'
description: 'Audit and fix color definitions'
publishDate: '2019-02-08'
heroImage: '/blog-placeholder-3.jpg'
---

Sometimes you may face this kinds of projects:

![Example](https://i.imgur.com/667goev.png)
<figcaption>Hard case, huh?</figcaption>
which bring you a little discomfort in the stomach. Today we'll talk about colors in particular and how they can be organized.

## Audit

One of the great options is to use online tools, like cssstats.com

![Example](https://i.imgur.com/mNhdfLX.png)
<figcaption>dev.to on cssstats.com</figcaption>

The issue with it that it shows only the colors that appear on the single page, so if you have code-splitting, some colors might be missing.
    I had a need something that would just show the colors used in the source files, but there wasn't anything, so I had to write a small utility [colors-in](https://github.com/omaiboroda/colors-in). It traverses the folder end extracts used colors:

![colors-in](https://i.imgur.com/X7ayPGN.png)
<figcaption>Colors in src folder</figcaption>

## Fix

Having this you can look at your the designs UI Kit or talk to your design to define the colors you're gonna use across the project.

At that point, we'd a need to "merge" similar colors and there's one approach you can do it. Since all colors are living in 3-dimensional `Red, Green, Blue` space, we can calculate the distance between them:

![colors-in](https://wikimedia.org/api/rest_v1/media/math/render/svg/06cdd86ced397bbf6fad505b4c4d91fa2438b567)
<figcaption>Euclidean distance</figcaption>

From my empirical feeling, those colors that are having less than 10% color difference can be deprecated in favor of more often used in a pair.

![colors-in](https://i.imgur.com/eQ7DHi7.png)
<figcaption>Difference between two shades of gray with npmjs.com/package/color-difference</figcaption>

Other things you can do:

* Unify color format to hex;
* Agree on 6 digits hex format;
* Remove unnecessary opacity layer;

In our case we were able to reduce the number of unique color definitions from 29 to 14:
Before:

![colors-in](https://i.imgur.com/zmwhwPP.png)
<figcaption>29 color definitions</figcaption>
After:

![colors-in](https://i.imgur.com/9BDCvPg.png)
<figcaption>14 color definitions</figcaption>

So if you have that "zoo" - make an Audit. Or if you keep your colors in an organized way - check if you forgot to use it somewhere else with `>colors-in src`;
