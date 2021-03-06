---
title: Static sites should be the default
date: 2018-04-23
tags:
  - development
  - tools
---

Let's define a "static" site to mean that each page is just a regular old HTML file sitting in directory. In other words, if you visit `/about-us` in a browser, then you're sent the HTML file that lives at `/about-us/index.html` which contains the entire page.

Obviously, nobody wants to sit there and edit each HTML file individually, so fabulous things called [static site generators](https://www.staticgen.com/) exist that can build the directories and HTML files for you, based on markdown files or data from an API or any number of things.

I believe that the vast majority of the web as it exists today could and should still be powered by these generated static sites. In my opinion, static sites should be the default, meaning that we should try to make each site a static site unless we have a very good reason not to (more on that below).

### What's so special about static sites?

The most obvious benefit here is performance. A static site has no DB queries, no backend language chugging or server processing, no waiting on external APIs to return, none of that. It does all of that ahead of time, so that it can send you a file that it already has sitting there. Because of this, static sites tend to be insanely, blazingly fast. Some generators take this even further and build in things like pre-loading of pages linked to from the current page, code splitting, inlining of CSS/JS, etc.

Static sites are also very secure. They are a collection of HTML files, so the surface area for attack is much smaller. The biggest security risk becomes the hosting provider or the security of any third-party services you're embedding on your site, rather than the site itself.

The hosting and infrastructure requirements are also about as simple as can be. You can host a static site anywhere - Amazon S3, Github Pages, static hosts like [Netlify](https://www.netlify.com) or [Surge](http://surge.sh/), there are even services that let you host them from your Dropbox. This means that hosting is super cheap, easy to set up, and almost effortless to maintain. If you need to switch to a new host at some point, just move the files. Plus, scaling is just a matter of putting your files in more places, and CDNs love static sites.

Those are, in my mind, the big 3 reasons why static sites should be the default. Here are a few more though:

* You can use whatever you language want to build them, since static site generators exist for most common languages and frameworks.
* If you use git as the data store, then you have all of the benefits of keeping everything in a single repo, such as using git commits for content revisions.
* You can do cool stuff during the build process like, as a random example, erroring upon finding any internal links that are 404'ing.

For more reading about the benefits here, check out the [PRPL Pattern](https://developers.google.com/web/fundamentals/performance/prpl-pattern/) and [JAMstack](https://jamstack.org/).

### When should I NOT use a static site?

There are certainly some exceptions here. If any of the following are true, a static site MAY not be the best option for you:

* If you have more than 10,000 or so pages on your site, then a static site build process may be annoyingly slow.
* If your site needs updates to be pushed live more than every half hour or so, then your site would constantly be rebuilding and redeploying.
* If your site has lots of user content or interactivity, such as forums or wikis or user-specific content, then you probably need a backend.
* If your site needs to support dynamic content or paths which are impossible to pre-generate, then a static site probably won't work.

Note that even with these exceptions, there are some workarounds. For example, you could only rebuild the pages that have been updated in order to support more than 10,000 pages (although you'd still be at the mercy of a full site rebuild when you do something like add a link to the main navigation), you could build hybrid/progressive pages with client-side JS to support dynamic paths or content, etc. But the point is that you'll be doing extra work to make your use case fit the constraints a static site, which is a red flag.

That said, if none of those exceptions describe your site, then it's likely a great candidate for a static site.

### But what about forms?

Most sites need at least a handful of forms, even if we're just talking about your basic contact form. With static sites, you have a few options:

* Use a 3rd party form service to embed your forms, such as [Wufoo](https://www.wufoo.com/) or [Jot Form](https://www.jotform.com/) or even just a regular old [Google Form](https://www.google.com/forms/about/).
* Create your forms in regular old HTML and just set them to submit to a service like [Formspree](https://formspree.io/) or [MailThis](https://mailthis.to/).
* Host your static sites with [Netlify](https://www.netlify.com/) which has built in [form handling](https://www.netlify.com/docs/form-handling/) by just adding a `netlify` attribute to any form.
* Create your own endpoint, either with a separate app living somewhere or go serverless with Lambda (Netlify has a built in UI for creating Lambda functions) or Google Cloud Functions, and submit the forms to that.

There are also some [other services](https://github.com/agarrharr/awesome-static-website-services#forms) that can help if you don't like any that I listed.

### But how can non-technical people update content?

You can't exactly hand a non-technical user a git repo with some markdown and say "have fun!" which is why lots of static site generators happily work alongside many common CMS'es.

You basically have two options here:

Option 1: Use a CMS built to write directly to markdown files in a git repo, such as [Netlify CMS](https://www.netlifycms.org/) or [Forestry](https://forestry.io/) or [Siteleaf](https://www.siteleaf.com/) or [many others](https://github.com/b-long/awesome-static-hosting-and-cms#content-management-systems-cms)

Option 2: Use any headless CMS that outputs an API, and consume that from your static site generator. Examples include [Contentful](https://www.contentful.com/) and [Prismic](https://prismic.io/) or you can use something self hosted like headless Drupal or headless Wordpress or [lots of other options](https://headlesscms.org/).

Either way, there are plenty of ways to give your non-technical clients the ability to add and update content without ever having to learn anything about Markdown or git.

### But what about search?

How can users search my site if it's all a bunch of HTML files? Options are slightly more limited here, but they exist.

The most common solution is to go with a commercial service such as [Algolia](https://www.algolia.com/) or a [Google Custom Search Engine](https://www.google.com/cse).

If you want to roll your own, you can use a tool like [LunrJS](https://lunrjs.com/) to build your own search index which gets loaded in client-side if the user is searching for something. There are docs on how to do this for most common static site generators, and it's fairly straightforward.

That said, and this is a pet peeve of mine in general, you may not really need a search. Check your analytics to see how often it's actually being used on your current site. There's a good chance that your typical user is just resorting to Googling if they can't find what they're looking for you in your navigation, since many users (such as myself) have been conditioned to just assume site search features are terrible.

### Ok, what are my options?

If you want to give it a shot, head over to [StaticGen](https://www.staticgen.com/) and poke around at the various static site generators that exist. You probably want to pick one that is popular (i.e., has at least a few thousand GitHub stars) and is built in a language you're familiar with or want to learn.

For the record, this blog is built with [GatsbyJS](https://www.gatsbyjs.org/) which is fantastic.

### Summary

Static sites are faster, more secure, and easier to host. The majority of websites could easily be built as static sites without losing anything. So why aren't they? If you disagree with me, feel free to [tweet at me](https://twitter.com/mcrittenden) and let me know why!
