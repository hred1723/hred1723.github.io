---
layout: post
title:  "Jekyll site setup notes"
date:   2021-10-29 14:49:00 -0700
categories: jekyll update
---
Setting up this site with Jekyll and Github pages was fairly straight-forward,
but there were some issues I had to figure out since I don't come with
experience using `ruby` and `gem` specifically.

## Using lessons learned from JavaScript land

Having worked with **node.js**, I had experience with a similar sort of thing
to Ruby's `bundler` and `gem`. Using node, I've done stuff like,

{% highlight bash %}
npm run start
{% endhighlight %}

...a lot making use of a `package.json` startup script.

## Following the documentation

I kept tabs open with the following documents:

- [Creating a Github Pages site with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)
- [Using Jekyll with Bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/)

We prefer to work with Bundler rather than the alternative, a global
installation of Jekyll and our project dependencies. This is so it will
be easier to download this project and work on it on different machines,
collaborate with others, and all the other reasons why modern developers
create and maintain these dependency management systems.

Depending how you're using your Jekyll site, you may need to work with a
different [publishing
source](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#choosing-a-publishing-source).
For example, if you were using a Jekyll site to document the progress of
working on another project, you might put it in some subdirectory or a
different branch.

For this site, I am using a branch called `gh-pages`; the entirety of my site
is the Jekyll site so I don't need to bother making a sub-directory called
`docs/` or anything like that.

Once stuff is installed, you can run the development server with `bundle exec
jekyll serve`. As you edit and save changes (e.g. in your `_posts/` directory)
Jekyll will re-build your project.

## Minimal Viable Product

Before doing fancy stuff like changing up my theme, I've gone ahead and
deployed this site to make sure that all the essential components are in place.
