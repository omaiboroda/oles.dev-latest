---
title: 'Try to update your dependencies once a month'
description: 'Keeping dependencies up-to-date is an important part of healthy projects, but often overlooked in Frontend (due to a lower security risk?) compared to backend or infrastructure.'
publishDate: '2020-12-08'
heroImage: '/blog-placeholder-3.jpg'
---

Keeping dependencies up-to-date is an important part of healthy projects, but often overlooked in Frontend (due to a lower security risk?) compared to backend or infrastructure.

Having the latest versions of libraries gives you and your teammates

* Security confidence
* Performance optimizations
* Bugfixes
* Access to latest features & new APIs

Often though, its importance is overlooked, which may lead up to critical level problems, when the project cannot work in the required environment (new browsers, or new requirements from app stores in case of mobile application).

Dependency update strategy though may vary according to the size of your team, kind of the product you're working on, etc. In some larger companies, there can be also special departments and private repositories for dependencies.

Working in a middle-sized team of developers (and non-health/wealth risk projects) we have tried 2 approaches for the updates.

### Per library, live updates

First what comes to mind is Dependabot, now a "native" solution from Github. Dependabot will open PRs once a new version of the library is in.

_How to enable:_ visit [app.dependabot.com](app.dependabot.com) and select repositories you want to add.

_Pricing:_ free of charge.

Once enabled, Dependabot will start to open PRs for new versions.

![Imgur](https://i.imgur.com/c9hBaTD.png)
<figcaption>dependabot in Pull requests tab</figcaption>

Dependabot will open PRs with the following contents

![Imgur](https://i.imgur.com/lG2FlCd.png)
<figcaption>dependabot's Pull request description</figcaption>

### Batch update, once a month

Soon we've realized live updates per library are too verbose.
In the case of Dependabot there was no chance to set up PRs to be open only for major updates, therefore another attempt with Renovate:

_How to enable:_ visit [renovatebot.com](renovatebot.com) and select repositories you want to add.
_Pricing:_ free of charge.

It is possible to ask Renovatebot to open batch PR once a month
![Imgur](https://i.imgur.com/w2R9gJg.png)
<figcaption>renovate in Pull requests tab</figcaption>

With the following configuration, Renovate will open monthly PR with the following contents
![Imgur](https://i.imgur.com/9vTRp2m.png)
<figcaption>renovate's Pull request description</figcaption>

Here's renovate.json configuration we settled:

```js
{
  "extends": [
    "config:base",
    "group:all",
    // Regular updates
    "schedule:monthly"
  ],
  // Random assignee of PR
  "assigneesFromCodeOwners": true,
  "assigneesSampleSize": 1,
  "separateMinorPatch": true,
  "packageRules": [
    {
      // Only major and minor updates
      "updateTypes": ["patch"],
      "enabled": false
    }
  ],
  // Encrypted NPM token for private packages
  "encrypted": {
    "npmToken": "w/0FYXCXO1aYkUeivEFbB7yG/CPM4xC2ksGVLLua0rNMYyEF8bxlrYqMtJqriuG53oecATlrSzgh06/U3sYkA2ZNgHabyoinZWc+fI0rchCCgSH0EXIeRifZH+nRNY69e5EleMOT0wNrkYlSuWN0U0uOU53ZZB+bojKPEpWhULZTOC6nXsN5WOrYdP0T1Tw=="
  }
}
```

Some other recommendations for the deps updates duties:

* Find a balance so updates are regular, but not too verbose.
* Random assign update PR to one of the contributors.
* Check libraries that got a major update for new APIs and features.
* Write update log, if some libraries were impossible to update.
