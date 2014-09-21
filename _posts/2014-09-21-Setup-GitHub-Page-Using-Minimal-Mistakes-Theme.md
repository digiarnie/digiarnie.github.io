---
layout: post
title: Setup GitHub Page using Minimal Mistakes Theme
excerpt: "Create your own GitHub Page."
modified: 2014-09-21
tags: [jekyll, github, minimal-mistakes]
comments: true
image:
  feature: texture-feature-05.jpg
  credit: Texture Lovers
  creditlink: http://texturelovers.com
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>Overview</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

* Install Ruby if you don't have it already (at least version 1.9.3)
* Check the Ruby version:

{% highlight bash %}
ruby --version
{% endhighlight %}

* Install nodejs (here I am using yaourt for Arch but use whatever package manager you would normally use)

{% highlight bash %}
yaourt nodejs
{% endhighlight %}

* Install Jekyll

{% highlight bash %}
gem install jekyll
{% endhighlight %}

* If you get the error *ERROR: Failed to build gem native extension.* while installing Jekyll, you will need to make sure you install ruby-dev:

{% highlight bash %}
gem install ruby-dev
{% endhighlight %}

* Create a new repository on github called `*yourusername*.github.io` (obviously substitute yourusername with your actual GitHub username)
* Clone the Minimal Mistakes repo onto your machine:

{% highlight bash %}
git clone git@github.com:mmistakes/minimal-mistakes.git *yourusername*.github.io
{% endhighlight %}

* Navigate into your github repository (obviously substitute yourusername with your actual GitHub username)

{% highlight bash %}
cd *yourusername*.github.io
{% endhighlight %}

* Install Bundler and use it to install the dependencies:

{% highlight bash %}
gem install bundler
bundle install
{% endhighlight %}

 * Make your initial commit of the blog into your GitHub repository (obviously substitute yourusername with your actual GitHub username):

{% highlight bash %}
git remote set-url origin git@github.com:*yourusername*/*yourusername*.github.io.git
git push origin master
{% endhighlight %}

 * Start your Jekyll server:

{% highlight bash %}
jekyll serve
{% endhighlight %}

* Now you should be able to access your new Minimal Mistakes blog at:

{% highlight bash %}
http://localhost:4000
{% endhighlight %}

For more information about setting up the theme for usage, head over to [Minimal Mistakes' Theme Setup](http://mmistakes.github.io/minimal-mistakes/theme-setup/)
