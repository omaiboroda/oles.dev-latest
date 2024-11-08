---
title: "Stale-while-revalidate and it's usage with Next.js"
description: "Stale while revalidate (SWR) is an established pattern for quick data display while refreshing data in the background"
publishDate: 'Aug 22, 2024'
heroImage: '/blog-placeholder-3.jpg'
---

Stale while revalidate (SWR) is an established pattern for quick data display while refreshing data in the background. It is popularised by SWR library <https://www.npmjs.com/package/swr>, but it roots comes from [HTTP spec](https://datatracker.ietf.org/doc/html/rfc5861)

Example:

```
Cache-Control: max-age=600, stale-while-revalidate=30, stale-while-error=30
```

This pattern fits well for content that doesn't update often like blogs, static pages, etc.

## SWR in Next.js

Next.js promotes ISR, which is an alternative to SWR. SWR is not supported in Vercel CDN, and this is the reason you might want to [pull out from Vercel](https://dev.to/omaiboroda/vercel-doesnt-want-you-to-pull-out-2047).

Next.js itself requires some configuration.

## SWR delta

To make SWR work in Next.js, you'd need to specify `swrDelta` in next.config.js, which is available from next@14

```js
experimental: {
 swrDelta: 31536000,
}
```

This is the value, for how long does server considers the content _stale_ and responds immediately to it. It is a common pattern to have this value high (ie 1 year), so at any point, we can react quickly and prefetch the latest content in the background.

It's essentially our `stale-while-revalidate` value in seconds.

## Max-age

This value specifies how long the server would respond without even checking the origin server.

In next.js, it is set via the `revalidate` constant, as with ISR.

``` js
export const revalidate = 3600;
```

It's essentially our `max-age` value in seconds.

As you can't modify `cache-control` in next.js, this is the only way to set the value.

This value is relatively short 1 hour to 1 day, depending on how active development is - in case of new features or bugfixes you still want to deliver quickly and propagate the changes.

With `stale-while-revalidate` you can afford not to invalidate the CDN cache on redeploy! After redeploying, your changes will propagate in `max-age` time (ie 1hr).

## Boosting with CDN

As abovementioned, Vercel doesn't support SWR, so there are some popular alternatives:

Fastly: supports SWR ✅ <https://www.fastly.com/blog/stale-while-revalidate-stale-if-error-available-today/>

Cloudflare doesn't support SWR ❌ <https://community.cloudflare.com/t/support-for-stale-while-revalidate/496788>

Cloudfront: supports since 2023 ✅ <https://aws.amazon.com/about-aws/whats-new/2023/05/amazon-cloudfront-stale-while-revalidate-stale-if-error-cache-control-directives/>

## Example on Cloudfront

No setup is required, as CDN either recognizes headers from origin or not and just respects the instructions in cache control.

Let's check now the first visit and response from Cloudfront:

```
x-cache: Miss from cloudfront
ttfb: 0.332219
```

Subsequent visit:

```
age: 642
x-cache: Hit from cloudfront
ttfb: 0.066109
```

Visit when content is considered stale:

```
age: 4001
x-cache: RefreshHit from cloudfront
ttfb: 0.024514
```

With `RefreshHit` Cloudfront responds immediately and makes call to the origin server asynchronously.
