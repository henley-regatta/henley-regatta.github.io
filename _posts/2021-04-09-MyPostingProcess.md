---
layout: post
title:  "QuickRef: My posting process"
date:   2021-04-09 17:12:00 +0100
categories: howto
tags: siteadmin jekyll howto
---
Because otherwise I'll forget....

Now I've got Jekyll and Github setup properly[^1], here's what I need to
go through to generate a new blogpost:

## From Home
1. Prep environment:
    1. Log in to `hotboi.local`
    1. Prepare github key (`source ~/.ssh/prep_github_key.sh`)
    1. `cd henley-regatta.github.io`
    1. `git checkout gh-pages` to switch to the correct branch
    1. `git pull` and check all updates apply correctly.
2. Create a post
    1. Create a file in `_posts` with format `YYYY-MM-DD-SomePostTitle.md`
    1. Edit the file in Your Favourite Editor.
    1. Set the correct metadata by ensuring the following is set at the TOP of the file:
    ```yaml
    ---
    layout: post
    title:  "Title of the Blog Post Hopefully Matching SomePostTitle.md Filename"
    date:   2021-04-09 15:36:00 +0100
    categories: howto
    tags: siteadmin jekyll howto
    ---
    ```  
    (Adjust all except `layout: post` to match what's being written about and when)
    1. Add the blog entry in [Markdown](https://www.markdownguide.org/tools/jekyll/)
    below the second `---` YAML delimiter
    1. Save the file.
3. Test locally
    1. From a shell on **hotboi**:
        1. `cd ~/henley-regatta.github.io`
        1. `bundle exec jekyll serve  -H hotboi`
    2. From a Web Browser: [http://hotboi.local:4000/](http://hotboi.local:4000/)
        1. Navigate to new content page from the index, verify contents.
    3. Re-edit the file and change until the contents render as desired[^2]

4. Commit and Publish from **hotboi**:
    1. `cd ~/henley-regatta.github.io`
    1. `git add _posts/*` (if you're brave; `git add _posts/YYYY-MM-DD-SomePostTitle.md` if you're not)
    1. `git commit -a -m "Added new blog post SomePostTitle"`
    1. `git push` to publish to github and thence the world.
5. Verify publish in a Browser by visiting [https://www.guided-naafi.org/](https://www.guided-naafi.org/) and
navigating to the post[^3].

## From Github.com

1. Login at [Github](https://github.com/)
2. Switch to [`henley-regatta.github.io`](https://github.com/henley-regatta/henley-regatta.github.io) repository
3. Go to the [`_posts` folder](https://github.com/henley-regatta/henley-regatta.github.io/tree/gh-pages/_posts)
4. Make sure the **gh-pages** branch is set to current.
5. Use the _Add Files_ button to add a Markdown file with the format `YYYY-MM-DD-SomePostTitle.md`
6. See "Create a Post" above to create.
7. Make sure to **Commit** the file with an appropriate comment to the **gh-pages** branch
8. Open [https://www.guided-naafi.org/](https://www.guided-naafi.org/) and
navigate to the post to check.
1. (Repeat steps 6-8 until you've got it right.)[^3]

## Common Factors

### Images
By convention, external assets for a page such as images are put into the `assets`
sub-directory (`~/henley-regatta.github.io/assets` on **hotboi**, the [assets](https://github.com/henley-regatta/henley-regatta.github.io/tree/gh-pages/assets)
folder of the `henley-regatta.github.io` repository on github).

Within a post, it's probably best to use the reference-link form of embedding
to make post reading easiest. For a picture in `assets` called `MyGnarlyPortrait.jpg`
a suitable markdown fragment would be:
```markdown
Hey I wanted to show you this completely radical picture I found:
![Gnarly Portrait Of Me]

blah blah blah
blah rest of post blah


[Gnarly Portrait Of Me]: (/assets/MyGnarlyPortrait.jpg)
{:style="display: block; margin: auto; width: 50%;"}
```

Note that the text in the initial reference is used as ALT text for the picture
so should be descriptive rather than referential.

### Favicon
Shouldn't need to worry about this but the process for adding a Favicon makes
me wonder whether I'm going to get into trouble with it later.
I started from [the last answer from StackOverflow](https://stackoverflow.com/questions/30551501/unable-to-set-favicon-using-jekyll-and-github-pages)
to achieve this with my preferred PNG icon:

![A tight crop of my best shot of Comet NeoWise from July 2020](/assets/favicon.png)
{:style="display: block; margin: auto; width: 50%"}

...And discovered that the documented process is _in transition_ at the minute so
instructions for Minima 2.50 are too complex but 3.xx don't work. So the additional
steps required were:
1. Create directory `~/henley-regatta.github.io/_includes`
1. `cp ~/gems/gems/minima-2.5.1/_includes/head.html ~/henley-regatta.github.io/_includes/head.html`
1. Add the line `<link rel="shortcut icon" type="image/png" href="{{ site.url }}/assets/favicon.png">`
directly under the `<head>` tag in `head.html`
1. Commit and republish

***
[^1]: I hope....
[^2]: The local Jekyll server instance can be left running if remote editing is being performed; it'll automatically sense file changes and re-build as required
[^3]: Apparently, all of this is *so much easier* than editing and publishing raw HTML. Or using Blogger. Or a Wiki.... So I'm told anyway.
