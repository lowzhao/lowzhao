---
title: Jekyll Quick Start Guide
layout: post
author: eugene
categories:
- Jekyll
- tutorial
image: assets/images/train.jpg
tags:
- featured
- sticky
---

I did the whole website in just under 3 hours! Thats crazy! That is from zero understanding of Jekyll to a full featured blog and able to serve and edit so smoothly. Jekyll by far have the smoothest learning curve I have experienced. Besides that I also learnt and setup <a href='https://disqus.com/' target='_blank'>Disqus</a>(for comments), <a href='https://disqus.com/' target='_blank'>Disqus</a>
## If you are like me don't have much time!
>  Leave a like if I can successfully help to set this up for you :D

\* Requirements before you continue:
1. Installed <a href='https://www.ruby-lang.org/en/documentation/installation/' target='_blank'>Ruby <i class="fab icon-ruby"></i></a>
2. Have a <a href='https://github.com/' target='_blank'>Github <i class="fab fa-github"></i></a> account
3. Installed <a href='https://git-scm.com/' target='_blank'>Git <i class="fab icon-git"></i></a>

### Instructions  🛠
1. Go to <a href='https://github.com/' target='_blank'>Github <i class="fab fa-github"></i></a> and start a new repository. (You can skips the options, "repository" is just a fancy name for "project" 🤪)
3. Copy the url shown to you.  (If you cannot see the url click the green `Clone or Download` button and copy)
5. Create a folder where you want to start your project.
6. Open Terminal any type `cd` + **space** then drag that folder into the terminal and hit `enter`.
7. Run the code below in the terminal:

```bash
gem install jekyll bundler;
curl -O https://codeload.github.com/wowthemesnet/mundana-theme-jekyll/zip/master;
unzip mundana-theme-jekyll-master.zip;
cd mundana-theme-jekyll-master;
bundle install;
jeykll build;
git init;
git remote add origin THE_URL_YOU_COPIED_FROM_GITHUB;
git add .;
git commit -m "init";
git push --set-upstream origin master;
```
\* Replace `THE_URL_YOU_COPIED_FROM_GITHUB` with the url you just copied.<br>
\* You might be prompt to enter your GitHub password.
6. Go back to github and your repository and go to settings. Then scroll down until you see `GitHub Pages` and click `None` and select `master branch`.
7. You will see a `url` at the same place (GitHub Pages section in your repository settings), after refresh click onto it and your website is ready!
8. At this time you will see that its very different from this one. You need to do some coding here to get it done. 
9. Open the `_config.yml` with any text editing program. (You might not see `_config.yml` if you didn't show extension. So you can look for `_config`)
10. Change `baseurl: '/mundana-theme-jekyll'` to `baseurl: 'GITHUB_PAGES_URL'` replace `GITHUB_PAGES_URL` with the `url` to your website GitHub save and try to push again! (`push` in git terms means putting/publish your changes onto a repository, in our example its on GitHub!)

Everytime you want to update your website you can 
```bash
jekyll build;
git add .;
git commit -m "ANY MESSAGE DESCRIBING YOUR WHAT IS YOUR UPDATE";
git push --set-upstream origin master;
```

Next we can set up <a href='https://github.com/jekyll/jekyll-admin' target='_blank'>Jekyll Admin</a>
1. Open Terminal any type `cd` + **space** then drag that folder into the terminal and hit `enter`.
2. Open up `Gemfile` and add  `gem 'jekyll-admin'` before end you should end up like this:

```
source "https://rubygems.org"
ruby RUBY_VERSION

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#

# If you have any plugins, put them here!
gem 'wdm', '>= 0.1.0' if Gem.win_platform?
group :jekyll_plugins do
    gem 'jekyll-feed'
    gem 'jekyll-sitemap'
    gem 'jekyll-paginate'
    gem 'jekyll-seo-tag'
    gem 'jekyll-admin'
end
```
4. Run the code below in the terminal:
```bash
bundle install;
bundle exec jekyll serve --watch;
```
1. Go to <a href='http://localhost:4000' target='_blank'>http://localhost:4000</a>. This is the home page.
3. Go to <a href='http://localhost:4000/admin' target='_blank'>http://localhost:4000/admin</a>. This is the admin panel.
4. You can create new post and refresh to see your post appear in the home page.
5. The post is written in the markdown format but you are free write HTML if you are familiar with it too!

I hope that complete newbie can also set up a full featured website. If you have any problems feel free to send me a message I will see if I can help you!

## If you want to `DIY` 

If you already have a full Ruby development environment with all headers and RubyGems installed (see Jekyll’s requirements), you can create a new Jekyll site by doing the following:

```bash
# Install Jekyll and Bundler gems through RubyGems
gem install jekyll bundler

# Create a new Jekyll site at ./myblog
jekyll new myblog

# Change into your new directory
cd myblog

# Build the site on the preview server
bundle exec jekyll serve

# Now browse to http://localhost:4000
```
