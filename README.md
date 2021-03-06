# sails-hook-slugs

[![Build Status](https://travis-ci.org/jledentu/sails-hook-slugs.svg?branch=master)](https://travis-ci.org/jledentu/sails-hook-slugs) [![dependencies Status](https://david-dm.org/jledentu/sails-hook-slugs/status.svg)](https://david-dm.org/jledentu/sails-hook-slugs) [![npm](https://img.shields.io/npm/v/sails-hook-slugs.svg)](https://www.npmjs.com/package/sails-hook-slugs)

This [Sails.js](https://github.com/balderdashy/sails) hook generates URL-friendly slugs in your models.

```
http://www.myblog.com/post/233987
-> http://www.myblog.com/post/title-of-my-blog-post
```

## Installation

Add this hook to your Sails app:

```shell
$ npm install sails-hook-slugs
```

That's all!

## Usage

Add an attribute of type `slug` in a model:

```js
module.exports = {

  attributes: {
    title: {
      type: 'string',
      required: true,
      unique: true
    },
    content: {
      type: 'text'
    },
    slug: {
      type: 'slug',
      from: 'title',
      blacklist: ['search']
    }
  }
};
```

Configure your slug attribute:

* `from`: name of the attribute from which the slug will be defined (required)
* `blacklist`: A list of reserved words to not use as this slug (optional)


A `slug` attribute is automatically set when you create a record:

```js
Post.create({
  title: 'This is a new post!!!',
  content: 'Post content'
})
.then(function(post) {
  console.log(post.slug); // 'this-is-a-new-post'
});
```

If a record of the same model has the same slug, a UUID is added at the end of the new slug:

```js
Post.create({
  title: 'This is a new post!!!!',
  content: 'A new post again'
})
.then(function(post) {
  console.log(post.slug); // 'this-is-a-new-post-a50ec97e-9ae1-44a5-8fb2-81c665b61538'
});
```

Like any other attribute, you can use dynamic finders:

```js
Post.findOneBySlug('this-is-a-new-post')
.then(function(post) {
  // Use the post
})
.catch(function(err) {
  // ...
});
```

## Configuration

These parameters can be changed in `sails.config.slugs`:

Parameter      | Type                | Details
-------------- | ------------------- |:---------------------------------
lowercase | `boolean` | Whether or not the generated slugs are lowercased. Defaults to `true`.
blacklist | `Array<string>` | A list of reserved words to not use as slugs in your application. Defaults to `[]`.

## License

MIT © 2017 Jérémie Ledentu
