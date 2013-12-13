---
layout: post
title: "Why Jekyll?"
date: 2013-12-6
categories: posts
---

In my first post, I gave a couple of vague reasons why I decided to use Jekyll
to create this website, as opposed to something like Ruby on Rails. Having 
recently used Rails for a school project, I can understand the appeal of using such
a framework. It can be used to create powerful dynamic web apps with a range of
useful tools and features. But for what I wanted out of this website, Rails would 
be like bringing a nuclear missile to a water gun fight. It's simply offers far
more features than what I need. 

Rails gives me the chance to build my own blog platform, while Jekyll _is_ a 
blog platform, which allows me to focus on the blog itself, rather than writing 
and maintaining everything. While it would give me complete
control over every aspect of my website, I am simply not interested in this
level of control for such a small website. I feel comparing
Jekyll and Rails is not fair because they are just two  different
tools that both happen to be able to accomplish the same task. 

Since I'm far more interested in showing what I like about Jekyll than I am in 
comparing and contrasting Jekyll and Rails, I will keep the comparisons to a minimum. 

Jekyll is as its website describes, "a simple, blog-aware, static site
generator", nothing more, nothing less. Since it's just a ruby script, issuing 
a simple `gem install jekyll` in a terminal is all it takes to have everything 
installed, much like Rails. Again similar to Rails, one can then generate the
scaffolding for a new blog by issuing `jekyll new blog`.

This is where the two diverge however, with Jekyll taking a simpler path, as
far as blogs are concerned. Navigating into a newly generated blog directory 
gives a simple directory structure.

`blog/`

```
_layouts/
_posts/
_site/
css/
_config.yml
index.html
```

Even at first glance this structure is fairly simple to follow. Jekyll divides
sites into three main components, layouts, posts, and CSS. Since each of these
parts are constructed very differently from one another, it was a very sane
choice to give each their own directory. 

This layout is very easy to extend as well. For example, if JavaScript is needed then all
that needs to be done is adding a `js/` directory to the root of the blog
directory, and it will be included in the final `_site/` folder after
generation.

Jekyll layouts use mainly HTML/CSS, spattered with [Liquid Markup][liquid]
that gets interpreted to HTML at build-time. Again, if this sounds somewhat similar to
how Rails works, that's because it is.

A good example of how this works is the following:

`_layouts/default.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>{{ "{{ page.title " }}}}</title>
  </head>
  <body>
    <nav>
      ...
    </nav>
    {{ "{{ content " }}}}
    <footer>
      ...
    </footer>
  </body>
</html>
```

Then in `index.html` all that is needed is:

```html
---
layout: default
title: Home
---

<p>Look at this snazzy navbar and footer!</p>
```

When `jekyll build` is run, the content of `index.html` will be combined with
`_layouts/default.html`, with the index content being placed in the `content` tag,
and the complete page(s), along with any CSS, will be placed in the `_sites/` directory. 
By just using the YAML front matter at the top of a page,
any page on the website can how have the navbar and footer, eliminating
redundancy or inconsistencies between creating multi-page websites. I found
this to be a very simple method to create consistency, while still maintaining
a certain level of control for each page.

Since a huge reason to use Jekyll is for the blog features, it helps that
creating blog posts is simple as well. Each blog post is written in markdown
and needs to be placed in the `_posts/` directory, with each following a naming
convention like `2013-12-6-why-jekyll.markdown`. Each post needs some YAML
front matter for Jekyll to construct everything properly, but the rest can be
written in pure markdown.

`_posts/2013-12-6-why-jekyll.markdown`

```
---
layout: default
title: "Why Jekyll?"
date: 2013-12-6
categories: posts
---

I dunno, _you_ tell me!

```

Jekyll makes it very simple to extend layouts for specific purposes, such as
dedicated pages for a post, using YAML front matter:

`_layouts/post.html`

```html
---
layout: default
---

<h2>{{ "{{ page.title " }}}}</h2>
<p class="meta">{{ "{{ page.date | date_to_string " }}}}</p>

{{ "{{ content " }}}}
```

Now each dedicated post page can display the title and post date in a
header just by specifying the post layout in the YAML front matter.

Of course, Jekyll has even more to offer, like built in pagination, syntax
highlighting, and countless plugins, but I'm not about to go on and on about it
for that long. Jekyll has so far been very pleasant to work with, and I can't
imagine switching to anything else any time soon.

[liquid]: https://github.com/Shopify/liquid/wiki/Liquid-for-Designers
