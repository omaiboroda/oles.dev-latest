---
title: 'Guide to Side effects in Hasura'
description: "Ways to achieve similar results, yet very different in implementation"
publishDate: 'Mar 14, 2023'
heroImage: '/blog-placeholder-3.jpg'
---

When dealing with Hasura, it can quickly come to the side-effects of doing something less trivial than using only predefined upserts/inserts.

Most often, this comes to _operations with databases, calling external API, or doing hefty validations_.

Hasura offers multiple ways of achieving it, though these methods have their own advantages and use cases, here are them:

## Postgres triggers

Hasura heavily relies on the database functionality, and in the case of Postgres, there is a possibility to achieve desired functionality with [Postgres functions and triggers](https://hasura.io/docs/latest/schema/postgres/postgres-guides/functions/).

```sql
CREATE OR REPLACE FUNCTION insert_initial_album() RETURNS TRIGGER LANGUAGE PLPGSQL AS $function$
    BEGIN
      INSERT into album (artist_id, title) VALUES (new.id, 'first album');
      RETURN new;
    END;
    $function$;


CREATE TRIGGER artist_after_insert AFTER
  INSERT ON artist
  FOR EACH ROW EXECUTE PROCEDURE insert_initial_album();
```

It is also possible to write [validations](https://hasura.io/docs/latest/schema/postgres/data-validations/#pg-data-validations-pg-triggers) on a Database-level

__Pros:__ powerful flexibility, database-native functionality.
__Cons:__ calling a 3rd-party API is impossible. Maintainance and visibility of Functions and Triggers can be a hassle.

__When to use:__ you are good with SQL, a believer in "you don't need an ORM".
__When to avoid:__ at large projects where observability of SQL can be an issue. SQL is not your strength.

## Multiple Mutations in a Request

Given there's a relationship established in Hasura, there's a possibility to [modify nested objects](https://hasura.io/docs/latest/mutations/postgres/multiple-mutations/), for example:

```graphql
mutation {
    insert_album(objects: {title: "First album", artist: {data: {name: "Bono"}}}) {
        returning {
            id
            artist {
              id
            }
        }
    }
}
```

This way, Hasura would create Album and Artist, and assign artist_id to the newly created Artist

__Pros:__ out-of-the-box functionality, single transaction.
__Cons:__ lacking flexibility, if required to do more business logic on top.

__When to use:__ fits your simple use-case.
__When to avoid:__ rarely, if it does solve the problem - use it!

## Event Triggers

Another option is an [Event trigger](https://hasura.io/docs/latest/event-triggers/index/) - webhook reaction on a database change

![Creating an Event Trigger](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lebpox1dw6skh06cptv3.png)
<figcaption>Creating an Event Trigger</figcaption>

__Pros:__ out-of-the-box functionality, ability to subscribe for particular CRUD events.
__Cons:__ it's not a transaction.

__When to use:__ effects, that doesn't require integrity, for instance: analytics update, sending emails, notifications.
__When to avoid:__ no atomicity - operations are executed separately.

## Hasura Actions

[Actions](https://hasura.io/docs/latest/actions/index/) is a mechanism for customizations, where custom queries or mutations can be implemented in webhook:

![Creating new Hasura Action](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/f8nc8czbes85sb65g6dm.png)
<figcaption>Creating new Hasura Action</figcaption>

This action would instruct Hasura to invoke handler:

![Example of webhook handler](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/81g5nx2c8opczmpumebz.png)
<figcaption>Example of webhook handler</figcaption>

__Pros:__ programming language runtime, flexibility, ability to do 3rd-party calls, and access to Database.
__Cons:__ cumbersome to set up handlers, types for every action needed.

__When to use:__ you have to do a few manual operations, and you have a server in place (ie next) that can handle the requests.
__When to avoid:__ you need lots of side effects (then add Remote schema) or when it's possible to use the abovementioned solutions.

## Remote schema

[Remote schema](https://hasura.io/docs/latest/remote-schemas/index/) is a variation on Actions, but you bring your own additional Graphql Schema. Adding it to Hasura, will extend existing Schema with Queries/Mutations where you have full control.

![Creating new Remote Schema](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bn4fw4k1f8oph4a72zad.png)
<figcaption>Creating new Remote Schema</figcaption>

__Pros:__ easy integration with Hasura.
__Cons:__ you'd need to maintain your own GraphQL Server.

__When to use:__ you already have a GQL server, or the application demands lots of non-trivial code.
__When to avoid:__ your app is small and doesn't require a standalone GQL server.

## Summary

These were 5 ways of achieving similar results in Hasura:

* Postgres triggers
* Multiple Mutations in a Request
* Hasura Actions
* Event Triggers
* Remote schema

Each of them has its strengths and weaknesses, and you can use them successfully in combination.
Hope this was helpful to pick the ones that suit you the best.
