SimpleIT Hugo Theme
========================

Responsive Hugo theme displaying articles by directory. 

Homepage displays the full directory tree structure and each
section/"subsection" its own articles and nested sections.

[![license](https://img.shields.io/github/license/marcanuy/simpleit-hugo-theme.svg?style=for-the-badge)](https://github.com/marcanuy/simpleit-hugo-theme/blob/master/LICENSE)


![SimpleIT Hugo Theme screenshot](https://raw.githubusercontent.com/marcanuy/simpleit-hugo-theme/master/images/screenshot.png)

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [SimpleIT Hugo Theme](#simpleit-hugo-theme)
- [Features](#features)
    - [Nested subcategories](#nested-subcategories)
- [Adding content](#adding-content)
    - [Adding posts without images](#adding-posts-without-images)
    - [Adding posts with images](#adding-posts-with-images)
    - [Adding images used in many posts](#adding-images-used-in-many-posts)
    - [Adding pages in multiple languages](#adding-pages-in-multiple-languages)
- [Installation](#installation)
    - [As a git submodule](#as-a-git-submodule)
    - [If you don't have Hugo with git](#if-you-dont-have-hugo-with-git)
- [Updating the theme](#updating-the-theme)
- [Run locally](#run-locally)
- [Theme config](#theme-config)
    - [Website in subdirectory](#website-in-subdirectory)
- [Customizing styles for your website](#customizing-styles-for-your-website)
- [Notes for developers](#notes-for-developers)
    - [Build](#build)
- [Screenshots](#screenshots)
- [Contributing](#contributing)
- [Websites using theme](#websites-using-theme)
- [Reference](#reference)
- [License](#license)

<!-- markdown-toc end -->


# Features

Features:

- mobile-first design
- two main layouts: 
  - single page 
	- Automatic TOCs based in content headers placed in sidebar
    - Choose Share Buttons type
      - static buttons optimized for loading speed
	  - generated with https://sharingbuttons.io/ (fast, minimal footprint)
	- Commenting system
		- Disqus
  - directory/category lists
	- If `/static/img/cover.jpg` exists, it is added as the background
      of the homepage header section
- SEO
  - Hugo internal templates
	- Open Graph
		- `{{- template "_internal/opengraph.html" . -}}`
    - Schema metatags
		- `{{- template "_internal/schema.html" . -}}`
		- When an article is updated, it automatically generates
          `dateModified` *itemprop* and shows the Last Updated info in
          the article. It obtains the data from the [git repo](https://gohugo.io/variables/git/#lastmod).
    - Twitter Cards
	  - `{{- template "_internal/twitter_cards.html" . -}}`
  - produces
	- minified HTML
	- SASS compiled in one *CSS* file
	- One *javascript* file
  - **Multilingual**: 
	- In `head`: Each page specifies other languages versions in *<head>* meta as specified in [Tell
		Google about localized versions of your
		page](https://support.google.com/webmasters/answer/189077?hl=en)
		like:
			- `<link rel="alternate" hreflang="lang_code" href="url_of_page" />`
	- In `footer`: Show alternative page translations, falling back to
      site available languages if a specific page doesn't have translation
- Analytics
  - Google Analytics
  - Bing
- Monetization:
  - Google Adsense:
	- Page level
    - Automatic ads just setting up config key
  - Optional support buttons in articles:
	- [Buy me a coffee](https://buymeacoffee.com)
    - [Ko-fi id](https://ko-fi.com)
- Content in tree directory structure of arbitrary depth supported
  - nested directories, each directory having its own articles and
    possible other directories
- Third-party apps
  - Twitter Bootstrap 4
  - Icons: Fontawesome 5
- Author box for each article.
  - Only shown in pages if `bio` is set at `[params.author]` in configuration.
  - Can be hidden with frontmatter: `hide_author: true`
- About me page in footer if `page_about_me_relurl` set in
  configuration for each language.

       [languages]
         [languages.en]
         page_about_me_relurl = "/about/"
- Previous/Next article buttons when displaying a single article.
- Generates an `rss+xml` [Atom](https://tools.ietf.org/html/rfc4287)
   file, an XML-based Web content and metadata syndication format, for
   each *directory/subdirectory*.
  - Example for URL: `https://simpleit.rocks/python/django/index.xml"`
    includes the `link`: `<link href="https://simpleit.rocks/python/django/index.xml" rel="alternate" title="Simple IT ???? Rocks" type="application/rss+xml"/>`

## Nested subcategories

The nested subcategories (aka.:sections/nested sections) get the same
URLs as the directory structure, for example, the following
directory/files would be available at these URLs:

~~~
PATH                                            URL

exampleSite/content/
|-- en
|   |-- _index.md                               /
|   |-- a-root-level-article.md                 /a-root-level-article/
|   |-- about.md                                /about/
|   |-- bar-first-level-section                 /bar-first-level-section/
|   |   `-- bar-page-in-section.md              /bar-first-level-section/bar-page-in-section/
|   |-- baz-first-level-section                 
|   |   `-- _index.md                           /baz-first-level-section/
|   `-- foo-first-level-section                 
|       |-- _index.md                           /foo-first-level-section/
|       |-- foo-second-level
|       |   |-- _index.md                       /foo-first-level-section/foo-second-level/
|       |   |-- foo-third-level
|       |   |   |-- _index.md                   /foo-first-level-section/foo-second-level/foo-third-level/
|       |   |   `-- page-at-foo-third-level.md  /foo-first-level-section/foo-second-level/foo-third-level/page-at-foo-third-level/
|       |   `-- page-at-foo-second-level.md     /foo-first-level-section/foo-second-level/page-at-foo-second-level/
|       `-- page-at-section-level.md            /foo-first-level-section/page-at-section-level/
`-- es
    |-- _index.md                               /es/
    `-- foo-seccion-primer-nivel
        |-- _index.md                           /es/foo-section-primer-nivel
        `-- foo-segundo-nivel                   
            |-- _index.md                       /es/foo-section-primer-nivel/foo-segundo-nivel
            `-- pagina-en-segundo-nivel.md      /es/foo-section-primer-nivel/foo-segundo-nivel/pagina-en-segundo-nivel/
~~~

# Adding content

```
$ hugo new <sections>/<article>.md
```

There are two types of Posts, with and without images. The cleanest
way to have them is to create a directory for those posts that have
images, and put all post's images in that directory.

## Adding posts without images

A post without a cover image should be created with the slug as the
filename inside the directory you want, for example, to create the
post
`http://example.com/foo-first-level-section/page-at-foo-first-level`
you should create the file `/content/foo-first-level-section/foo-second-level/page-at-foo-second-level.md`

*slug* will be obtained from post's filename

~~~
---
title: "Post example without images"
date: 2019-01-10
subtitle: ''
description: ''
---

## Overview

Here I don't use any image!
~~~

## Adding posts with images

Posts with images can have two types of images:

- Cover image
  - Displayed at the top section of the post
  - Used in *Facebook's OpenGraph* / *Twitter cards* metadata (a.k.a.
    the
    image that will be displayed when sharing the post in those Social
    Networks)
  - Should be defined at post's frontmatter.
- Content images
  - Images used along the post content

For example, to create the post
`http://example.com/foo-first-level-section/page-at-foo-first-level`
you should create the directory with the *slug* name and content
inside the directory as an `index.md` file
`/content/foo-first-level-section/foo-second-level/page-at-foo-second-level/index.md`.

All images used by this post goes inside the above directory (`/content/foo-first-level-section/foo-second-level/page-at-foo-second-level/`)

And *slug* will be obtained from post's directory name.

To add the cover image, so it can be displayed by OpenGraph and
Twitter, add the metadata to the **resources** frontmatter section and
put the image in the newly created directory
`/content/foo-first-level-section/foo-second-level/page-at-foo-second-level/victor_hugo.jpg`.

The featured image resource **name** must be **cover** so it will be
 used as the featured image of the article.

This follows the standard name name defined by Hugo for [Twitter Cards
featured image](https://gohugo.io/templates/internal/#twitter-cards).
 
Rest of images should be configured in an **images** array in front
matter, being the first one the featured image. This will be used in
Facebook's OpenGraph metadata and Twitter Cards image section, so it
is displayed when sharing them.

In *images* array, the full path after *content* directory preppended
to image name should be used, as Hugo loads them preppending the
website's domain.

Then other images in same directory and used in content `/content/foo-first-level-section/foo-second-level/page-at-foo-second-level/dcu_models.jpg`

~~~
---
title: "Post example with images in cover and content"
date: 2019-01-10
subtitle: ''
description: ''
images:
- foo-first-level-section/foo-second-level/page-at-foo-second-level/victor_hugo.jpg
- foo-first-level-section/foo-second-level/page-at-foo-second-level/dcu_models.jpg
resources:
- name: cover # this should be the name if you want it to appear as article's featured image and used by OpenGraph and Twitter cards
  src: victor_hugo.jpg
  title: "Portrait photograph of Victor Hugo"
  params: #can be ommited
    license: "Public Domain"
    original: "https://commons.wikimedia.org/wiki/File:Victor_Hugo_by_%C3%89tienne_Carjat_1876_-_full.jpg"
---

## Overview

Hey! I have an image that it is not featured, look:

<img class="img-fluid" alt="model diagram" src="dcu_models.jpg" /> 

~~~

Tree directory structure of
`/content/foo-first-level-section/foo-second-level/page-at-foo-second-level/`
would look like:

~~~
.
|_ index.md
|_ dcu_models.jpg
|_ victor_hugo.jpg
~~~

## Adding images used in many posts

You can also add images to site's root at `/assets/img/` and then use
it directly referring them with full path `/img/....`.

For example, for `my_image.png` at `/assets/img/`
 then it can be used in multiple posts:
`<img class="img-fluid" alt="example image" src="/img/my_image.png" />`.

## Adding pages in multiple languages

To add a new language, just specify it in the `[languages]` section of
`config.toml` and create a subdirectory in `/content` as specified at
[Translation by content directory
](https://gohugo.io/content-management/multilingual/#translation-by-content-directory).

Then for each page or section:

Set [translationKey](https://gohugo.io/content-management/multilingual/#bypassing-default-linking) for `_index` and *pages* and their slug as
directory or filename. I use the slug of the section/page I am
translating as the translation key, for example for
`/content/en/foo/_index.md`:
   
   - EN: `/content/en/foo/`
   
         ----
		 translationKey = "foo"
		 ----
   
   - ES: `/content/es/foo-slug-in-spanish/`

         ----
		 translationKey = "foo"
		 ----

Same process for pages.
  
Then each level of the hierarchy will have its corresponding page in
the other language, and *subdirectories* will also be translated.



# Installation

## As a git submodule

	$ git submodule add -f https://github.com/marcanuy/simpleit-hugo-theme themes/simpleit-hugo-theme

## If you don't have Hugo with git

Inside the folder of your Hugo site run:

    $ cd themes
    $ git clone https://github.com/marcanuy/simpleit-hugo-theme

# Updating the theme

After installing the theme, if you added it as a `git submodule`, you
can update it with:

    git submodule update --remote --merge

# Run locally

In order to see your site in action, run Hugo's built-in local server.

    $ hugo server -t simpleit-hugo-theme

Now enter [`localhost:1313`](http://localhost:1313) in the address bar of your browser.

Or just set `theme="simpleit-hugo-theme"` in your configuration.

# Theme config

Have a look at
[/exampleSite/config.toml](https://github.com/marcanuy/simpleit-hugo-theme/blob/master/exampleSite/config.toml)
for theme customization options.

## Website in subdirectory

If the website is located in a subdirectory like `example.com/blog/`
then URLs should be generated with an absolute path with the following
configuration:

~~~
canonifyURLs = true
~~~


# Customizing styles for your website

If you want to change some styling to fit your own website needs, copy
the following files to your website instance and edit them:

- `assets/sass/custom_variables.scss`: customize variables used by
  Bootstrap.
- `assets/sass/styles.scss`: your main website styles with all
  Bootstrap variables avaiable.

Or include them with a SCSS include:

- for `custom_variables.scss`:

~~~ sass
@import "../../themes/simpleit-hugo-theme/assets/sass/custom_variables.scss";
~~~

and for `styles.scss`

~~~ sass
@import "../../themes/simpleit-hugo-theme/assets/sass/styles.scss";
~~~

# Notes for developers

If you plan to change something or contribute to theme development
keep in mind that this theme uses
[npm](https://docs.npmjs.com/getting-started/what-is-npm) to handle
packages, to install its dependencies:

	$ cd themes/simpleit-hugo-theme
	$ make install

All Bootstrap and Fontawesome SASS variables are available to

- customizing the variables 

  In [assets/sass/custom_variables.scss](https://github.com/marcanuy/simpleit-hugo-theme/blob/master/assets/sass/custom_variables.scss)
  you can customize Bootstrap.
  
- use variables in [assets/sass/styles.css](https://github.com/marcanuy/simpleit-hugo-theme/blob/master/assets/sass/styles.scss)
  
  For example, using Fontawesome variables like `$fa-var-twitter`, in `assets/sass/styles.scss`: 
  
  ~~~
  .twitter {
	  @include fa-icon;
	  @extend .fab;

		&:before {
			content: fa-content($fa-var-twitter);
		}
	}
  ~~~
  
  or using Bootstrap variables like `$kbd-bg` , in `assets/sass/styles.scss`:
  
  ~~~
  .shell {
    background-color: $kbd-bg;
  }
  ~~~

## Build

Building or serving the website:

	$ cd themes/simpleit-hugo-theme
	$ make build

Serving the theme with changes:

	$ cd themes/simpleit-hugo-theme
	$ make serve
  
# Screenshots

Single page screenshot:

![SimpleIT Hugo Theme single page screenshot](https://raw.githubusercontent.com/marcanuy/simpleit-hugo-theme/master/images/single.png)

# Contributing

Did you found a bug or got an idea for a new feature? Feel free to use the [issue tracker](//github.com/marcanu/simpleit-hugo-theme/issues) to let me know. Or make directly a [pull request](//github.com/marcanuy/simpleit-hugo-theme/pulls).

# Websites using theme

Hugo websites using this theme:

- https://simpleit.rocks
- https://therunningsecret.com

Add your own website here with a Pull Request!

# Reference

- Github repo: https://github.com/marcanuy/simpleit-hugo-theme.git
- Page at Hugo themes site: https://themes.gohugo.io/simpleit-hugo-theme/
- Hierarchical content demo: https://marcanuy.github.io/simpleit-hugo-theme/
- Flat demo: https://themes.gohugo.io/theme/simpleit-hugo-theme/

# License

This theme is released under the MIT License. For more information read the [License](//github.com/marcanuy/simpleit-hugo-theme/blob/master/LICENSE).


