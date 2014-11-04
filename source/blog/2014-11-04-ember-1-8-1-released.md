---
title: Ember.js 1.8.1 and 1.9 Beta Released
author: Matthew Beale
tags: Releases, Recent Posts
---

Today we are releasing Ember.js 1.8.1, a patch-level release of Ember that
fixes several minor regressions introduced in the 1.8 release.

### Throw assertion when `attributeBindings` includes `class`

Prior to 1.8, it was possible to make `class` part of `attributeBindings` and have
those values merged with `classNameBindings`. For example, with the following template
and code "from-template" and "from-class" would be merged into the DOM node's class
list.

```hbs
{{foo-bar class="from-template"}}
```

```js
App.FooBarComponent = Ember.Component.extend({
  classNameBindings: [':from-class'],
  attributeBindings: ['class']
});
```

The intent of this code is unclear and the pre-1.8 behavior was unintentional. In Ember
1.8.1 an assertion is thrown for including `class` in `attributeBindings`.

### View instances 

Passing view instances to the `{{view` helper was broken in Ember.js 1.8. This behavior
has been restored.

### Work-around provided for more iOS8 ARM bugs

iOS8 has introduced a severe bug in optimized ARM code. In 1.8.1 even more potential
occurrences of this bug have been protected against.

(note: link to issue at JSC)

### Support rendering of null-prototype objects

meta-data objects in Ember-Data are null-prototype, a special kind of object created
with `Object.create(null)`. In 1.8 these objects could not be rendered. 1.8.1 repairs
this.

### Support non-string unescaped content 

In Ember 1.8 rendering an unescaped value that was not a string `{{{someNumberLiteral}}}`
would raise an exception. This has been fixed.

### Support multiple option-rendering template blocks

In Ember 1.8, template strings are parsed into DOM via `innerHTML`. Creating a stand-alone
option tag this way results in that tag being considered "selected" even if it is not
explicitly so. Consequently the last option of a select would often be selected upon
render instead of the first.

We've restored support for templates like this:

```hbs
<select>
  <option>First option is selected at render in 1.8.1</option>
  {{#each item in items}}
    <option>{{item.name}}</option>
  {{/each}}
</select>
```

### Known whitespace issues in Chrome

Some templates may result in missing whitespace in Chrome. For example,
if the following template renders initially with no names, then updates
via data-binding later, the space between names may not be visible.

```hbs
{{firstName}} {{lastName}}
```

In Ember.js 1.9 we expect to land a refactor to the morph library that
updates dynamic DOM content, and this should alleviate this issue. Merging
the refactor into a point release is
too risky, so we have labelled this issues a wont-fix.

The Chrome team has been notified of this bug and you can track
progress [here](https://code.google.com/p/chromium/issues/detail?id=428313).

## Changelogs

+ [Ember.js 1.8.0 to 1.8.1 commit log](https://github.com/emberjs/ember.js/compare/v1.8.0...stable)
+ [Ember.js 1.8.1 CHANGELOG](https://github.com/emberjs/ember.js/blob/v1.8.1/CHANGELOG.md)