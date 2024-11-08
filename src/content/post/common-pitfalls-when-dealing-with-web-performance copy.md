---
title: 'Common pitfalls when dealing with web performance'
description: '1. Not understanding there are different kinds of performance - animation, interaction, load, returning.'
publishDate: '2020-12-20'
heroImage: '/blog-placeholder-3.jpg'
---

Common pitfalls when dealing with web performance

1. Not understanding there are different kinds of performance - animation, interaction, load, returning.

Often, when the word "performance" is said, it is understood as the timing of the initial load. Although, this is not the only kind of timing that can exist in a web application.
Apart of the initial load, there can be a performance for returning users (leveraging assets cache), performance after user interaction, or UI animation performance etc.

_Tip: take a look a the RAIL model <https://web.dev/rail/>_

2. Taking into account uncompressed assets.

Descent amount of assets like .html, .css & .js, .json are able to be significantly compressed with gzip or brotli. Sometimes this can lead to significant asset size reduction if the contents of the files are repetitive.
The fact that assets are compressed is sometimes overlooked and may lead to the wrong conclusions.
Naturally, this is applicable for transferring the files over the network. Processing the assets (which also has a significant impact) ie script evaluation happens after the assets are decompressed.

_Tip: Pay attention to gzipped sized first, uncompressed second, for example on <https://bundlephobia.com/>_

3. Comparing Lighthouse results

Lighthouse tests are run in a simulation mode, so every result may vary for different people and even for the same user within subsequent runs.

_Tip #1: Try to achieve "constant" results by running Lighthouse in incognito and/or get multiple runs at <https://lighthouse-metrics.com/>_

_Tip #2: Understand the reasons of variability <https://developers.google.com/web/tools/lighthouse/variability>_

4. Not knowing the network waterfall

When optimizing the initial load performance, it's important to understand the full picture of network requests and act knowing the order, size & processing duration, otherwise may lead to winning in one, loosing in another.

_Tip: Use <https://www.webpagetest.org/> to understand the connections on your website_

5. Using Desktop on cable profile as a reference.

Talking about performance on Desktop may lead to mistakes since most of the pages are downloaded & executed fast on desktop CPUs & Cable networks so lots of the time Desktop profile is not a bottleneck.

_Tip: Take a habit to talk about performance within the reference device, for instance, in Lighthouse it's a MOTO G4 on a "slow 4G" connection_

Happy optimizations!
