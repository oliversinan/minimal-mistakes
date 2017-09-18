---
header:
  teaser: assets/images/jekyll_logo.png
categories:
  - jekyll
  - github
tags:
  - jekyll
  - github pages
---
In this guide, we will learn how to set up a github user page by forking a given Jekyll template from github. The website will contain a landing page, blog posts (including categorization and tags), additional pages and a navigation bar.
{% include toc %}

Requirements:
- Github account without a pages respository

### Important

This guide only scratches the surface of this templates capabilities and shall only illustrate a bare minimum setup and highlight a few functionalities. For a complete guide and documentation, visit the [minimal-mistakes github page](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/).

## Template Forking

For this website, I used the template `minimal-mistakes`.
First, fork the [template repository](https://github.com/mmistakes/minimal-mistakes) (you have to be logged into github). To be able to reach the forked repository, we have to rename it. Github requires the repository to be named as follows:
```
GITHUB_USERNAME.github.io
```
In my case, I had to rename the forked repository to `oliversinan.github.io`. This will be the url to reach our website. Optionally, you can specify another domain in the github repository.

## Settings
Delete the directories `docs` and `test` as they don't contain necessary files for our use case of the template.
The `_config.yml` file in the root directory contains most of the options for the template. First, we update the url, in my case:
```yaml
url: "https://oliversinan.github.io"
```
You can set additional parameters such as `title`, `name` and `description` of the website. Much more information on the possible options and parameters can be found [here](https://mmistakes.github.io/minimal-mistakes/docs/configuration/).

## Adding content
### Landing Page
First off, we update the `index.html` file in the root directory to display an image, some custom text and set it up to be the root page of our website.
```yaml
---
layout: splash
permalink: /
header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/banner.jpg
excerpt: '<b>Guides to state of the art machine learning <br>techniques and applications.</b>'
---
```
A banner image has to be placed in the path specified in `overlay_image`, the text written in `excerpt` will be overlayed on the image.
Additionally, we're going to display the most recent blog posts by adding following code:
```liquid
{% raw %}
<h3 class="archive__subtitle">{{ site.data.ui-text[site.locale].recent_posts | default: "Recent Posts" }}</h3>

{% for post in paginator.posts %}
  {% include archive-single.html %}
{% endfor %}

{% include paginator.html %}
{% endraw %}

```
The number of displayed posts per page can be changed in `_config.yml`.
### Blog Posts
Now, we'll create a post which will be displayed on our home page. Create a subdirectory named `_posts` in the root directory of the repository. Jekyll will look inside this directory for blog posts. The files in this subdirectory have to adhere to a specific naming convention, containing the date of the post and a title slug. For example, the file name for this blog post is:
```
2017-09-18-intro-to-jekyll-on-github-pages.md
```
The style of the files in the `_posts` directory has already been specified at the bottom of our `_config.yml` file. This is called the Front End Layout and saves us from repeating the same layout choices in every posts.

The following code shows an example post defining categories, tags and a little text to display.
```yaml
---
categories:
  - jekyll
  - github
tags:
  - jekyll
  - github pages
---
This is a quick demo post to check the functionality of the uploaded jekyll template to github pages.
More content will follow shortly!

```
The url to the page will be automatically generated, depending on the used categories, tags and slug, in this case it is: https://oliversinan.github.io/jekyll/github/intro-to-jekyll-on-github-pages/. You can also set the url manually by adding the `permalink` tag (e.g. see Landing Page).
### Pages
Adding a page is similar to adding a post. First, we have to create a subdirectory named `_pages` in the root directory of the repository. We will now add a page displaying all posts grouped by category. Create a file named `category_archive.html` with following content:
```yaml
---
layout: archive
permalink: /categories/
title: "Posts by Category"
author_profile: true
---
```
```liquid
{% raw %}
{% include group-by-array collection=site.posts field="categories" %}

{% for category in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ category | slugify }}" class="archive__subtitle">{{ category }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
{% endraw %}

```

### Navigation Bar
To have easy access to our landing and category page, we will modify the navigation bar. Update the file `_data\navigation.yml` to contain following code:
```yaml
- title: "Home"
  url: /
- title: "Categories"
  url: /categories/
```

Congratulations, you've set up a bare minimum version of this template. Check out the template [repository](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/) and [page](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/) for more information on how to modify your website.
