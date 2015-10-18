Meteor
======

## Commands

* `meteor create` stats a new app
* `meteor` runs the app
* `meteor shell` runs the meteor shell
* `meteor mongo` runs the mongo shell
* `meteor reset` to empty database
* `meteor run ios` to deploy and launch in iOS simulateor
* `meteor run ios-device` to deploy and launch Xcode

## Folder structure

* `.meteor` contains meteor core files and packages
* `client` files available for the client only
  * `client/compatibilty` js libraries not available as meteor packages
* `server` files available for the server only
* `lib` available for both, loaded before anything else
* `public` public assets

## Mongo

* `db.books.find()` returns an interactive cursor
* `db.books.findOne()` returns one
* `db.book.insert({ title: "Bara Yogoï"})` inserts

## Meteor collections

* `Books = new Books.Collection("books")` creates a mongodb collection
* `Books.find()` returns an interactive cursor
* `Books.find().fetch()` returns an array

## Publish/Subscribe

The `autopublish` package is installed by default in any new Meteor app created
with `meteor created`. Once removed, we can manage precisely what data is
shared.

```
// Server-side
Meteor.publish('books', function() {
  return Books.find();
});

// Client-side
Meteor.subscribe('books');
```
Share an entire mongo collection between the client and the server


```
Meteor.publish('books', function() {
  return Books.find({ category: 'Urban fantasy' });
});
```
Publish a filtered collection

```
Meteor.subscribe('books', function() {
  return Books.find({ author: 'Yves & Ada Rémy' });
});
```
Filter what we want to subscribe to

## Iron Router

```
Router.configure({
  layoutTemplate: 'layout',
  notFoundTemplate: 'notFound',
  loadingTemplate: 'loading',
  waitOn: function() { return Meteor.subscribe('posts'); }
});
```
Defines
* the main layout template
* the template to show on 404
* the template to show while loading data
* to wait before subscriptions are loaded

```
Router.route('/', {name: 'postsList'});
```
Defines a named route

## Sessions

* Session.set(key, valus) defines a session variable
* Session.get(key) gets it

## Tracker

By default, code executed outside of template helpers won't update if their value is updated. Tracker.autorun solves this.

Tracker.autorun( function() {
  console.log(Session.get('stored'));
});
Will be updated every time Session.set('stored') is called 