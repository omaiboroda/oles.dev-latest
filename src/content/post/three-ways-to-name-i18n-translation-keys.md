---
title: 'Three ways to name i18n translation keys'
description: 'Teams do internationalization of applications either from the very beginning or after delivering a primary language. Usually, the translation flow looks similar to the following:'
publishDate: '2021-07-08'
heroImage: '/blog-placeholder-3.jpg'
---

Teams do internationalization of applications either from the very beginning or after delivering a primary language. Usually, the translation flow looks similar to the following:

![Translation flow](https://i.imgur.com/zbnBkXs.png)
<figcaption>Translation flow</figcaption>

And from a developer perspective, it contains 3 steps:

1. Receive content from designs, create keys and populate initial "en" translation;
2. Announce keys to translators;
3. Occasionally fetch latest translations.

It is pretty easy to create translation keys, but hard to do it in a way that it

* Makes clear for translators what do they mean;
* Makes clear for developers what do they mean;
* Is looking good in source files;
* Avoids often context switching between translation files and sources;
* Helps developers to map code blocks with "physical" representations on the page.

There are already good i18n guidelines written by [Phraseapp](https://phraseapp.com/blog/posts/ruby-lessons-learned-naming-and-managing-rails-i18n-keys/) and [MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Localization/Localization_content_best_practices), but they do not answer how should we name the translation keys themselves.

To abstract from implementation, we'll agree on translation function `t`, which takes a translation key and returns message:

```js
    t('things') // => "How things work"
```

### Option 1  - Interesting

You might have only custom defined keys, without defaults. For instance:

```js
    t('landing_page.how_things_work') // => "How things work"
```

As for the initial translation, there will be a need to add initial messages to a translation file.

**Pros**

* Doesn't take much space in source files
* Gives enough context to a translator
* Ready to change over time

**Cons**

* In case if the key is too generic hard to track the relationship between the source and actual place of display on the page
* To set the initial message, you would need to switch from sources to the translation file

### Option 2  -  Bulletproof

You can specify both - translation key and default message:

```js
t({
    key: 'landing_page.how_things_work', 
    defaultMessage: "Things explained"
}) // => "How things work"
```

**Pros**

* Flexible - you can change your default message without being afraid it will cause issues;
* You have a connection between sources and actual visual representation.

**Cons**

* May face duplication in short keys, for example `t({ key: 'general.errors.empty', defaultMessage: 'empty'})`;
* Bloats sources size with the id and message.

### Option 3 - Straightforward

You can use the same text as your message in English:

```js
t('How things work') // => "How things work"
```

This way we're eliminating the necessity to maintain default messages and create keys for them.

**Pros**

* Developers maintain single entity;
* Gives translator instant clarity what should be translated;
* You can use them as default messages.

**Cons**

* It worth to add more context for a translator. This can be hints to the source code's filename (as in .po format) or in-context-editor;
* Sometimes you have to translate entire paragraphs, which make key too long;
* Requires extra solution for those English words, which have multiple meanings i.e. *to cry*.

As you might already understand, there's no 100% correct to define the translation keys and the decision should be made based on your project size, how often you change translations and team preference.

At the current project, we've decided to give a shot for "#3 Straightforward" approach for sake of simplicity, ease of maintenance and less context switching.

But how do you cook i18n keys?
