---
layout: post
title:  "Is this on now?"
date:   2021-04-09 13:52:00 +0100
categories: jekyll
tags: siteadmin jekyll noise
---
So I think I've got a minimally-working site using Jekyll. The instructions
I was using were incomplete and WRONG in at least one place - the github page [Adding a new page to your site](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-content-to-your-github-pages-site-using-jekyll#adding-a-new-page-to-your-site) excludes the _vital_ information that the YAML frontmatter **must** be delimited by `---` before and after. Between that and
mis-copying or getting the Theme wrong (leading to blank pages) it wasn't the 3-clicks-and-you're-done I was led to believe it might be. But, I guess, it works...

Here's my Hello World in proper markup this time:
{% highlight python %}
#!/usr/bin/python3
import code-my-blog

srcdir="blog"
try:
    code-my-blog.publish(srcdir)
except PublishError:
    print("Publishing failed")
    exit(1)

print("Publishing complete")
exit(0)
{% endhighlight %}

This, apparently, should render as a table:

|Date|Location|Result|
|:-:|:--|--:|
|2021-04-25|Birmingham|Ugly|
|2021-03-13|Bristol|Beautiful|
|2021-02-14|London|Bad|
|2020-12-09|Pill,<br> Pill|We Love Thee<br> Still|
|====|
|Christmas|North Pole|Ho Ho Ho!|

[See Also This Link]({% post_url 2021-04-09-MyPostingProcess %})

(dude... this is _fun_ but it's *not* simpler than just editing some HTML....)
