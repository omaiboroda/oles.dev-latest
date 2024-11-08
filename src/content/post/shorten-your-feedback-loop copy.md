---
title: 'Shorten your feedback loop'
description: 'One of the key satisfiers in the development experience is knowing the results of your actions immediately.'
publishDate: '2020-12-18'
heroImage: '/blog-placeholder-3.jpg'
---

One of the key satisfiers in the development experience is knowing the results of your actions immediately.
That's why apart from the constant aim of simplifying everything as a software developer, there can be an aim of a constant shortening the feedback loop.

A short feedback loop is a fundamental idea in some core programming practices, such as TDD.

In the perspective of frontend development, here's what you can consider:

**1. Shorten UI feedback during development.**

Pay attention to the time it takes between the UI change in development mode and its reflection in the browser.
Ensure you're leveraging [react-refresh](https://www.npmjs.com/package/react-refresh) and use proper [webpack devtool config](<https://webpack.js.org/configuration/devtool/>) if you're on this ecosystem.

**2. Know how to simulate the state of the app, i.e. to stay on the same page on refresh during development.**

With the growing complexity of the application, the number of states, in which this application can is growing as well.
It is worth to consider a mechanism for state simulation, like [make your own dev tools](https://kentcdodds.com/blog/make-your-own-dev-tools/)
so you can quickly jump to a certain app state, and/or stay in it during development.

**3. Know how to Deploy the application on the test environment from your local machine.**

Depending on your setup, this can save you from running build or test steps on CI just to quickly check some ideas.
Even better: know how to simulate the test environment locally.

**4. Know how to run and scope tests locally.**

Rather than waiting on CI to check if the tests are green, know how to run your unit/integration tests and scope them to certain files/suits for faster outcomes.

**5. Know how to test things without redeploying the app.**

Sometimes, to validate ideas, there's no need to deploy the app on a test environment - leverage browsers' devtools features like blocking API requests or local overrides. This way you got some answers to your questions immediately.

**6. Ensure IDE assists you.**

Seeing fast lint-check or type-check errors right in IDE gives you great (probably the fastest) feedback about potential problems. Make sure your lint & type-check solutions are integrated tightly with the IDE.

For a delightful developer experience, speed of ideas validation is a key player for your mental health and sometimes sets the mood for the whole day. Make "shorten your feedback loop" a motto for your setups, don't be lazy to think about how can you validate ideas faster and enjoy your work even more.
