---
layout: post
title:  "How to create a blog with Jekyll 4 and host it for free on GitHub"
date:   2021-12-14
tags: [jekyll, blog, github, website]
---

Today I want to note down how I created this very blog using GitHub Pages and Jekyll 4 – a static site generator.

Why? Because [GitHub uses Jekyll 3.9.0][gh-pages-versions], whereas the current version of Jekyll is 4.2.1,
see [all Jekyll releases][jekyll-releases].

To do this I had to go through various sources and here is the complete guide.

<!--more-->

## Contents
  - [Prerequisites](#prerequisites)
  - [Setting up GitHub repository](#setting-up-github-repository)
  - [Setting up local environment](#setting-up-local-environment)
  - [Setting up deployment using GitHub Actions](#setting-up-deployment-using-github-actions)
  - [(Optional) Setting up custom domain](#optional-setting-up-custom-domain)
  - [Example](#example)

### Prerequisites

* GitHub account
* Installed Git locally
* Basic knowledge of HTML and CSS – later for styling your blog
* (Optional) Money for the custom domain

### Setting up GitHub repository

First, create a *public* GitHub repository. The name of the repository should be `username.github.io`.
For example, if your username is `octocat`, repository should be named `octocat.github.io`.

Clone your repo locally

{% highlight bash %}
$ git clone git@github.com:username/username.github.io.git
$ cd username.github.io
{% endhighlight %}

### Setting up local environment

1. Install Ruby. MacOS has Ruby installed by default but let's get a new one with [Homebrew][homebrew].

{% highlight bash %}
$ brew install ruby
{% endhighlight %}

Find out where brew ruby and gems are

{% highlight bash %}
$ brew info ruby
{% endhighlight %}

Here's what I have here:

{% highlight bash %}
...
By default, binaries installed by gem will be placed into:
  /usr/local/lib/ruby/gems/3.0.0/bin

You may want to add this to your PATH.

...


If you need to have ruby first in your PATH, run:
  echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> /Users/myusername/.bash_profile
...
{% endhighlight %}

Add both of them to your PATH
{% highlight bash %}
echo 'export PATH="/usr/local/opt/ruby/bin:/usr/local/lib/ruby/gems/3.0.0/bin:$PATH"' >> ~/.bash_profile
{% endhighlight %}

Relaunch terminal and verify you use the brew ruby now
{% highlight bash %}
$ which ruby
/usr/local/opt/ruby/bin/ruby
$ ruby -v
ruby 3.0.3p157 (2021-11-24 revision 3fb7d2cadc) [x86_64-darwin20]
{% endhighlight %}

Now install jekyll and bundler

{% highlight bash %}
$ gem install --user-install bundler jekyll
{% endhighlight %}

Add `.gem` to your PATH

{% highlight bash %}
echo 'export PATH="$HOME/.gem/ruby/3.0.0/bin:$PATH"' >> ~/.bash_profile
{% endhighlight %}

Initiate a new Jekyll site (`--force` option is here in case you already have some files in your directory that you don't need and wish to overwrite)

{% highlight bash %}
$ jekyll new . --force
{% endhighlight %}

Build and run the site locally

{% highlight bash %}
$ bundle exec jekyll serve
{% endhighlight %}

Open [http://127.0.0.1:4000][localhost] in your browser – you should see the website there.

### Setting up deployment using GitHub Actions

Jekyll has [almost working instructions][jekyll-gh-actions] on how to do that.

Here's what to do to make it work correctly.

1. Create in your project file `.github/workflows/github-pages.yml`
2. Copy the following contents into that file

{% highlight sh %}
name: Build and deploy Jekyll site to GitHub Pages

on:
  push:
    branches:
      - master

{% raw %}
jobs:
  github-pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: helaili/jekyll-action@v2
        with:
          target_branch: 'gh-pages'
          token: ${{ secrets.ACTIONS }}
{% endraw %}
{% endhighlight %}

NOTE: `target_branch` is missing from Jekyll docs and token name can't start with `GITHUB`, 
so I choose the name `ACTIONS`.

#### Setup the token in GitHub

1. Go to [Personal access tokens][gh-tokens] on GitHub.
2. Generate a new token:
  * give it `public_repo` permissions
  * setup expiration date
  * save the generated value locally
3. Go to your repository's settings on GitHub and then to the `Secrets` tab.
4. Create a token with the name `ACTIONS` (name should be the same as in the yaml file) and provide it 
the saved value from the generated token.
   
#### Add all generated files and commit them to GitHub

Check what is tracked by git

{% highlight sh %}
$ git status
{% endhighlight %}

If there's anything you don't want to add to GitHub, add those files to `.gitignore`.

Add, commit and push your files

{% highlight sh %}
$ git add --all
$ git commit -m "Initial commit"
$ git push -u origin master
{% endhighlight %}

#### Verify your changes

Go to the `Actions` tab of the repository and see that the workflow is completed successfully.

Once it is completed, navigate to [https://username.github.io][https://username.github.io], 
where `username` is your GitHub username. Success, the blog is ready and is published for everyone to see!

### (Optional) Setting up custom domain

If you want your blog to run at some address other than `username.github.io`, do the following:
1. Register a domain you like
2. Follow official [GitHub instructions][gh-custom-domain]
3. Add `CNAME` file to the root of your project and add there your domain (according to the documentation,
   it should be created automatically, but it did not happen ¯\\\_(ツ)_/¯)

### Example
If you need a specific example, you can visit [the source code of this blog][my-blog].

[gh-pages-versions]: https://pages.github.com/versions/
[gh-tokens]: https://github.com/settings/tokens
[gh-custom-domain]: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain
[jekyll-releases]: https://github.com/jekyll/jekyll/releases
[jekyll-gh-actions]: https://jekyllrb.com/docs/continuous-integration/github-actions/
[homebrew]: https://brew.sh
[localhost]: http://127.0.0.1:4000
[https://username.github.io]: https://username.github.io
[my-blog]: https://github.com/lina-is-here/lina-is-here.github.io

