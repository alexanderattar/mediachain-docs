VISIT THE HOSTED SITE: https://mediachain.github.io/mediachain-docs 
----
# Mediachain Documentation site

This repo contains the system-level documentation for the
[Mediachain project](https://www.mediachain.io).

The docs are hosted on Github Pages at https://mediachain.github.io/mediachain-docs and automatically generated by Jekyll when new content is
posted to the `master` branch.

There's a few quirks about Jekyll and our specific setup that you should keep in mind when adding
or updating docs, which I'll go over a bit below.

## Adding Content

All content goes into the `_docs` directory (the underscore is required by jeykll).  

### Front matter

Every page *must* have a YAML "front matter" block at the top, or it won't be included in the
output.  Here's an example:

```
---
title: Installing Mediachain
layout: page
category: setup
order: 1
---
```

The `title` is used as the link text in the navigation menu, and also is inserted as an `H1`
before the page content.  So don't add your own `H1` title in the content itself, or you'll end
up with two!

The `layout: page` tells Jekyll which template to use.  There's only one set up, so always use
`layout: page` or it'll be busted.

#### Categories

The `category` field groups docs in the navigation menu by category.  This is a required field,
although we may end up adding a default category at some point.

Right now there's only one category, `setup`, which shows up in the nav menu as
"Installation and Setup".  This is defined in [`_config.yml`](./_config.yml) as the
`sections` array:

```
sections: [
    ['setup', 'Installation and Setup']
]
```

To create an new section in the menu, add a new two-element array to the `sections` array.
The first element should be the short name used in the front matter (e.g. `category: setup`),
and the second is the heading you want to appear in the nav menu.  Then you can tag your doc
with the new short name, and it will show up in the new section.

#### Ordering in the menu
The `order` field is used to sort the items for each category in the nav menu, so they don't show
up in the default order (alphabetical by markdown filename).  It's not required, but if you omit
the `order`, that doc will be sorted to the top of the nav menu, I guess because the empty value
sorts before anything else.  Fixing that requires a lot of template hacks that don't seem worth it.

### Table of contents

Jekyll's markdown processor [kramdown](https://kramdown.gettalong.org/), will generate a table of
contents from the headers in your doc.  Add this to the top of your doc:

```
* TOC
{:toc #table_of_contents}
```

Side note: There's a little bit of jQuery that pulls the generated TOC from the page body and
slips it into the navigation menu.  If javascript is disabled, you won't see the TOC, since it's
initially rendered with `display: none` so you don't see it flash onscreen before the jQuery hack.  
The hack is needed because github pages doesn't support Jekyll plugins that would give us more
control over the TOC rendering.

### Links Between Docs

When you link to another doc, you should use this (pretty terrible) bit of Liquid template cruft.
Assuming you want to link to `getting-started.md`:

```markdown
Let's [get started]({{ site.baseurl }}{% link _docs/setup/getting-started.md %})
```

Inside the `{% link %}` helper, you need to use the full path name from the repo root, including
the `_docs` directory and `.md` extension. You also need the `{{ site.baseurl }}` bit before the
helper to get the correct absolute url on the deployed site.

### Markdown quirks

The markdown processor supports GitHub-flavored markdown pretty well, but there's one quirk.
Special "blocks" like triple-backtick code blocks, unordered lists, etc *must* be preceded by an
empty line.

So this won't render correctly:

    Here's a code snippet that will look terrible:
    ```
    boo();
    ```

But this will:

    This one works thanks to the empty line after the paragraph:
    
    ```
    hooray();
    ```

## Changing site layout, styles, etc

TK
