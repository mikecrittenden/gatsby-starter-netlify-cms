---
templateKey: blog-post
title: In Defense of Functional CSS
date: 2018-06-07
description: Tailwind is the future
tags:
  - development
  - tools
---

Functional CSS (or Atomic CSS, or Utility-first CSS) is the latest thing to have blown my mind. This post is my attempt to explain why I think it's amazing and why all of the common arguments against it are misguided or rooted in outdated ideas.

### What the heck is it?

Functional CSS basically means that you have a ton of tiny, single purpose classes that are named based on their visual function.

For example, instead of something like this:

```html
<div class="profile-card">
  ...
</div>

<style>
  .profile-card {
    padding: 20px;
    margin: 20px;
    color: #eee;
    background: #333;
    border: 1px solid #555;
  }
</style>
```

You'd instead just do this:

```html
<div class="m-5 p-5 text-gray-light bg-gray-darker border border-gray-light">
  ...
</div>
```

So instead of actually writing any CSS, you just apply the utility classes for each of the CSS rules that you would normally have written.

### But I don't want to sit there and write a bajillion utility classes

And you don't have to. Lots of frameworks exist, such as:

* [Tailwind](https://tailwindcss.com/) (my favorite)
* [Tachyons](http://tachyons.io/)
* [Basscss](http://basscss.com/)

There are also [lots more](https://css-tricks.com/need-css-utility-library/) if none of those are doing it for you.

### Isn't that basically the same thing as inline styles?

Not quite, for a few reasons. Inline styles don't respect media queries, and they aren't limited to pre-defined options, and they cause specificity issues, etc.

Utility classes fix all of these things. I've written [more about this in the past](https://mikecr.it/ramblings/utility-classes-vs-inline-styles).

### What about separation of concerns and semantic markup?

This can be tough to get past, but if you really think about it, why is semantic markup valuable?

Is it because it's useful to be able to tell what something is based on the class name? That's the easy answer, but has that ever really been valuable to you? Have you looked at a thing with `class="profile-card"` and said "oh this is a profile card", when it wouldn't have been just as easy to tell that based on the context or what it contains?

Furthermore, is your existing markup even semantic? If you're using classes like `card` or `form__submit--disabled`, then it probably isn't. If it IS semantic, then what are you doing in situations where `profile-card` and `contact-block` need to look exactly the same? How are you sharing those styles? Or are you sharing them at all? Is semantic markup possibly hurting more than it's helping?

In my opinion, semantic markup is something that has been burned into our brains because of the history of CSS (trying to get people away from using tables, sites like CSS Zen Garden, etc.) and it's not necessarily very useful anymore. In fact, I think that it's actually much more useful to have classes which tell you what something looks like, rather than what something is.

If you're skeptical, take some time and read [this post](https://adamwathan.me/css-utility-classes-and-separation-of-concerns/) which really was a light bulb for me.

### OK, but the DOM still looks like a dumpster fire

Again, this is tough to get past, but once you get used to it, there's a chance you might just learn to love it. That dumpster fire tells you exactly what something will look like, and doesn't give you any information you don't need.

Anyways, it looks better than it would if you were using many of the common CSS in JS solutions, which give you a ton of crazy randomly-generated class names.

### I don't want to have to repeat the same 20 classes on every single button

That's understandable. I will say that there's a chance that repeating those 20 classes is actually somewhat valuable, because when you get into a situation where one of the buttons needs to have slightly more margin-top than the others, then it's easy to fix.

That said, for situations where you are absolutely positively sure that you need the EXACT same styles and that won't ever change, then some of the Functional CSS frameworks have solutions for this. Tailwind for example, [allows creating components](https://tailwindcss.com/docs/extracting-components/) which are composed of utility classes. This allows you to do things like this:

```html
<button class="btn-blue">
  Button
</button>

<style>
.btn-blue {
  @apply .bg-blue .text-white .font-bold .py-2 .px-4 .rounded;
}
.btn-blue:hover {
  @apply .bg-blue-dark;
}
</style>
```

### Won't I end up with thousands of classes that I don't even need?

Since the framework generates so many classes, plus versions of those classes for hover state or responsive breakpoints, you really end up with a lot of classes unless you do something about it.

This may not be a huge deal, since (as an example), all of Tachyons is only [15kb when compressed and minified](https://medium.com/@philipardeljan/15kb-of-css-is-all-youll-ever-need-%EF%B8%8F-634da7258338), and most non-trivial sites have at least that size in custom CSS.

Either way, the frameworks have solutions for this too. Tachyons for example has [a generator](https://github.com/tachyons-css/generator) which allows you to specify which classes you need or don't need, and Tailwind has [configuration support](https://tailwindcss.com/docs/configuration) for it as well. For example, this allows you to say "I don't need responsive versions of box-shadow classes", or "I only need classes for these 4 colors."

### What if I need things (colors, fonts, padding options) that don't exist in the pre-made classes?

Frameworks to the rescue again. The same configuration options I mentioned above give you the ability to specify what you want your padding options to be, or which font families to use for which font stacks, or what your color options are, etc.

You can basically build your own list of utility classes, to the point that it's theoretically possible that you shouldn't need to write a single line of custom CSS.

### Ok fine, but what are the benefits?

There's really a lot to love here, I think.

* You don't have to write any CSS of your own (which, to me, is fantastic)
* You can likely build things faster (obviously non-scientific, but anecdotally I've seen many people confirm this)
* You don't ever have to think about naming things
* You can tell what something looks like by just reading the markup for it
* You don't ever have to worry that changing the styles for one thing will break something else (which may make visual regression testing irrelevant)
* You never have to deal with one instance of a thing needing a slightly different style than the other instances, which screws up your reusable classes.
* Your CSS always stays the same size rather than expanding over time
* Rendering speed performance is supposedly improved (though I have seen no proof of this)

### Still not convinced?

Here's a [giant list of links defending Functional CSS](https://johnpolacek.github.io/the-case-for-atomic-css/) that you may find interesting.

Also, feel free to [tweet at me](https://twitter.com/mcrittenden) and debate!