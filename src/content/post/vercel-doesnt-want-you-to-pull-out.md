---
title: 'Vercel Doesn’t Want You to Pull Out'
description: "I have been working with Vercel for years and love it, although it's started to frustrate me over the last few months"
publishDate: 'Aug 6, 2024'
heroImage: '/blog-placeholder-3.jpg'
---

I have been working with Vercel for years and love it, although it's started to frustrate me over the last few months. Here are some points that made us decide to pull out, even though Vercel doesn't want you to.

## Struggles with Vercel

### Weird CDN

The TTFB occasionally hiccups to 250ms+. These requests were responding `x-vercel-cache: HIT`, and it's the same page. This slow, which is quite weird.

![CDN quirks](https://i.imgur.com/53c3kLs.png)
<figcaption>Looks like CDN goes to origin, yet still responds with HIT</figcaption>

### Overwhelming of pay-per-request

I know it's the essence of SaaS, but it's overwhelming how many items you can be billed for, including [horror stories](https://x.com/shoeboxdnb/status/1643639119824801793).

As for the latest changes, Vercel is rolling out Data Cache (which is a great feature), starting to bill it, and [changing the default data cache policy from cache-all to no-cache](https://nextjs.org/blog/next-15-rc#caching-updates) between Next.js 14 and Next.js 15. This doesn't contribute to my confidence.

![Vercel pay-per-request](https://i.imgur.com/hFmtwHk.png)
<figcaption>Overwheling amount of billing parameters at vercel.com</figcaption>

### Support has not been great

I have submitted three support requests over the last few months (a bug in log drain exports, elaboration on data/deployment cache), and none of them were satisfactory. After message ping-pong on every ticket, being responded to by multiple support engineers, I just gave up on my hopes of being helped. At some point, I see that further discussion is pointless and close the ticket. I hope this is not a support strategy!

### Logs filtering DX

Personally, I find the Logs tab in Vercel confusing, with limited capabilities to filter and occasional very similar records going side-by-side.

## What if we self-host?

All these points led us to pull out from Vercel and self-host Next.js. But it's not amazing - `Cache-Control` header is optimized only for Vercel.

### Cache-Control headers are locked-in

You simply [cannot set `Cache-Control`](https://github.com/vercel/next.js/issues/22319) header on pages. Next.js locks them in for you, saying they know what's best—optimizing for deployment on Vercel:

> Cache-Control headers set in next.config.js will be overwritten in production to ensure that static assets can be cached effectively.

### For ISR, default max-age is one year

Next.js casually [sets a one-year(!) cache](https://github.com/vercel/next.js/issues/22319) for your HTML if you don't explicitly specify the `revalidate` value.

### stale-while-revalidate was introduced not per spec until Next.js 14

`stale-while-revalidate` directive was provided [without a value](https://github.com/vercel/next.js/issues/51823). This led to incompatibility with other CDNs—only Vercel could understand it.

## Can we?

We have managed to overcome this and moved to self-hosting, SWR, and Cloudfront - a setup which I will share soon.
