# Using Jekyll Themes

Recommend check out my other [guide](https://github.com/jmount1992/dummy-website-3) prior to doing this one.

## Kick Off

We will use the [Jekyll Quick Start](https://jekyllrb.com/docs/) to help us get a lot of the boilerplate out of the way.

1. Navigate to the directory where you want your root folder for your website to exist.
2. Use Jekyll to create a new website: `jekyll new <website-name>`
3. Navigate into the new website root folder: `cd <website-name>`
4. Locally serve the newly created website: `bundle exec jekyll serve`

Check out the new website. You will find it already looks much prettier than what we have previously created. This is because, when you run `jekyll new <website-name>`, it creates a lot of boilerplate including adding a pointer to the [minima](https://github.com/jekyll/minima) theme. We can also change the theme, and we will look at that shortly. However, first we shall look at some of the boilerplate, that hopefully we will make some sense to you if you did the previous guide, as well as remove some of the unrequired stuff.

### Gemfile Exploration
We will start by exploring the `Gemfile`. It all looks fairly standard. You will even see some comments and commented code pointing towards how you can use Jekyll with GitHub Pages. However, we will aim to use the latest and greatest Jekyll, as not to restrict ourselves to GitHub Pages dependencies. So we can go and get rid of that. There is also some gem files to for specific platforms, we can leave those be. Your Gemfile may now look something like this:

```ruby
# Gems source
source "https://rubygems.org"

# Jekyll, the theme, and jekyll plugins
gem "jekyll", "~> 4.3.1"
gem "minima", "~> 2.5"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
# do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
```

### Configuration File
Let us now look at the `_config.yml` file. Again, there are some comments, you should quickly skim these have they do have some insights. One thing to note is the exclusion list. [Jekyll configuration](https://jekyllrb.com/docs/configuration/options/#global-configuration) can provide some additional details on exclusion lists. Go ahead and update the configuration variables under the site settings comment. Feel free to remove some of the commentry as well. Your configuration file may look something like this now.

```yaml
# Site settings
title: James Mount
email: jmount1992@example.com
description: >- # this means to ignore newlines until "baseurl:"
  This is a dummy website that is part of James Mount's website building guide.
baseurl: "/dummy-website-4" # the subpath of your site, e.g. /blog
url: "https://jamesmount.tech" # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: jmount1992
github_username:  jmount1992

# Build settings
theme: minima
plugins:
  - jekyll-feed
```


### File Exploration

Everything else should be fairly straight forward. 


## Automatic Deployment

The first thing we will setup is the automatic deployment workflow. As we want to use the latest and greatest Jekyll version, and not be tied to what the github-pages Gem uses, we need to use a custom workflow. Here are the instructions to setup the automatic workflow. If you want a greater explanation, see the previous guide.

1. Copy the contents of the `jekyll.yml` [starter workflow](https://github.com/actions/starter-workflows/blob/main/pages) to a file called `jekyll.yml` located at `.github/workflows`. Hint: the filename you use doesn't have to match what GitHub uses.
2. In your copy of the workflow change `[$default-branch]` to `main`.
3. Push your changes to the remote repository and watch the action do its thing! You should now have a live website.