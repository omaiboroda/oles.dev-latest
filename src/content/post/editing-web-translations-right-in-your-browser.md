---
title: 'Editing web translations right in your browser'
description: "Give your translator more context by enabling In-Context-Editor"
publishDate: 'Jan 9, 2019'
heroImage: '/blog-placeholder-3.jpg'
---

Sometimes GIF replaces 1000 words:

![In-Context Editor](https://i.imgur.com/PPDZrBn.gif)
<figcaption>looks interesting, right?</figcaption>

[DEMO](https://demo.phraseapp.com/)

Having this, you give your translator 100% understanding of the translation context.

The feature is usually called `In-Context Editor` (for instance in [Locize](https://docs.locize.com/more/incontext-editor) or [Phraseapp](https://help.phraseapp.com/translate-website-and-app-content/use-in-context-editor-to-translate/translate-directly-on-your-website)).

It's worth to say you need PhraseApp Pro subscription for it. In pair to that, we also use `lingui-js` as a translation library.

## How to enable

All you need to is

1. Add PhraseApp In-Context Editor snippet to your page
2. Format messages into the proper format.

If you're in the Javascript world, I have wrote a [lingui-phraseapp](https://www.npmjs.com/package/lingui-phraseapp), which helps you to do these two steps, so your integration may look alike

```js
import { initializePhraseAppEditor, initializePhraseAppEditor } from "lingui-phraseapp";
import catalog from './locales/de/messages.js';

const configInContextEditor = {
  projectId: REACT_APP_PHRASEAPP_PROJECT_ID,
  autoLowercase: false,
  prefix: "--__",
  suffix: "__--",
  phraseEnabled: !!localStorage.getItem("inContextEditor");
};

...
<!-- Add snippet -->
initializePhraseAppEditor(configInContextEditor);

<!-- Format messages -->
const catalogFormatted = configInContextEditor.phraseEnabled
  ? transformCatalog(catalog, configInContextEditor)
  : catalog;

const i18n = setupI18n({ catalogs: { de: catalogFormatted });
...
```

As you can see from above to enable Editor set `inContextEditor: true` in the LocalStorage. After that, the modal for username and password will appear.

There are also implementations for other popular translation libraries [react-intl-phraseapp](https://github.com/phrase/react-intl-phraseapp), [react-i18next-phraseapp](https://github.com/phrase/react-i18next-phraseapp), although integration itself is pretty straightforward and simple.

## The most important question

Now to the things that really matter:

> Does this brings value at all?

The translator we're working says she wanted it for 2 years, although we haven't started active translation phase yet. And you can't say for sure until you'll work with it on daily basis.

I will give an update on this feature, stay tuned!
