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

The only other thing to note is the [jekyll-feed](https://github.com/jekyll/jekyll-feed) plugin. This is a plugin to generate an Atom feed of your Jekyll posts. A person can [subscribe](https://www.thesitewizard.com/faqs/howtoreadsitefeeds.shtml) to this feed and get updated when you have created a new post. More general information about Jekyll Plugins can be found [here](https://jekyllrb.com/docs/plugins/installation/). We also need to list the 

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

timezone: Australia/Brisbane

# Build settings
theme: minima
plugins:
  - jekyll-feed
```

**Important Notes**:
- We must also list the plugins in the configuration file, as shown in the above example.
- Remember to add in the timezone information.

### File Exploration

Everything else should be fairly straight forward. However, you may be wondering where are the directories `_layout`, `_includes`, `assets`, etc. As we are using a theme, these are bundled and stored with the minima Gem. You could overwrite the minima theme's default layout by creating, for example, `_layout/default.html` in your website root directory. If you wish to use minima theme's default layout as a starting point for your own default layout, you may wish to copy the theme's default layout first and then hack away. To find the theme's files on your computer you can run the command `bundle info --path <theme>`, in this case replace `<theme>` with `minima`. More information on Jekyll Themes can be found [here](https://jekyllrb.com/docs/themes/). We will look at changing the theme shortly. If you wish, you can change `index.markdown` and `about.markdown` to use the shorter file extension `.md`.


## Automatic Deployment

The first thing we will setup is the automatic deployment workflow. As we want to use the latest and greatest Jekyll version, and not be tied to what the github-pages Gem uses, we need to use a custom workflow. Here are the instructions to setup the automatic workflow. If you want a greater explanation, see the previous guide.

1. Copy the contents of the `jekyll.yml` [starter workflow](https://github.com/actions/starter-workflows/blob/main/pages) to a file called `jekyll.yml` located at `.github/workflows`. Hint: the filename you use doesn't have to match what GitHub uses.
2. In your copy of the workflow change `[$default-branch]` to `main`.
3. Prior to pushing your changes to your repository. Make sure you have gone to the repository settings online and set the Pages to source to use a github action. Settings > Pages > Under Build and Deployment > Set Source to `GitHub Actions`.
3. Push your changes to the remote repository and watch the action do its thing! You should now have a live website.

## Changing the Theme

There are many themes out there. The Jekyll documentation has a [list](https://jekyllrb.com/docs/themes/#pick-up-a-theme) of various places you could go to find themes. If you look at the [GitHub Pages documentation](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll), they state they have some default supported themes. However, the instructions aren't clear and can lead to some errors. Also, because we are using the latest and greatest Jekyll, we should be able to use any theme out there! The easiest way to use a theme, is to include the remote theme plugin and alter the configuration file. For example, to use the [Cayman](https://github.com/pages-themes/cayman) theme, we would do the following (feel free to try):

1. Change your `Gemfile` by removing the minima theme and adding the [jekyll-remote-theme](https://github.com/benbalter/jekyll-remote-theme) and [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag) plugins to the jekyll plugins group. We need the `jekyll-seo-tag` plugin as it is used by the Cayman theme. If you forget to add it, that is okay, when you go to serve your website locally you will get an error that will basically say you are missing the Gem. Simply add it and move on. You can also remove the `jekyll-feed` plugin if you wish, it isn't used by the Cayman theme. Your Gemfile should now look like this:
    ```ruby
    # Gems source
    source "https://rubygems.org"

    # Jekyll, the theme, and jekyll plugins
    gem "jekyll", "~> 4.3.1"

    group :jekyll_plugins do
        gem "jekyll-remote-theme"
        gem "jekyll-seo-tag"
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
2. Run `bundle update` to make the changes.

3. In `_config.yml` remove the `theme` tag and replace it with `remote_theme: cayman`, and add the plugins to the list. You can also remove the jekyll-feed plugin, if you removed it from the Gemfile. Your `_config.yml` will now look something like this:
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

    timezone: Australia/Brisbane

    # Build settings
    remote_theme: pages-themes/cayman@v0.2.0
    plugins:
    - jekyll-remote-theme
    - jekyll-seo-tag
    ```

4. Build and serve the website locally. You will most likely get a warning that says `Build Warning: Layout 'post' requested in ...`. This is because the Cayman theme only has one layout, the `default` layout. Typically the easiest way to see what layouts are available with a remote theme, is to look at its github repository. Go change the layout in `index.md`, `about.md`, and in the post located in the `_posts` folder to `default`.

5. Rebuild and serve the website locally.

If you followed along, you will see our home page now follows the Cayman theme. Yay! However, unfortunately the Cayman theme is designed as a single page. We could, using the knowledge we gained in the previous guide, create a navigation menu by creating `_includes/navigation.html` and `_data/navigation.yml` files, and hacking the `default.html` template. And we will look at that shortly. However, firstly, we will go explore another theme.

The Cayman theme is one of several themes that GitHub Pages supports. However, we aren't limited to the themes supported by GitHub pages. As we are using the latest Jekyll and the remote-theme plugin, we can use any theme. A very popular theme is the [minimal mistakes](https://github.com/mmistakes/minimal-mistakes) theme. A preview of the theme can be found [here](https://mmistakes.github.io/minimal-mistakes/). Most themes have an online preview. Let's try changing to that theme.

1. In the `Gemfile` add `gem "jekyll-include-cache"` to the list of jekyll plugins.
2. In the `_config.yml` file change the remote theme to `mmistakes/minimal-mistakes@4.24.0` and add `jekyll-include-cache` to the list of plugins. Also, while we are at it, add set some of the configuration variables associated with the minimal mistakes theme. In this example we set some of the site author configuration. Some of the original configuration variables we had might not actually be used by the minimal mistakes theme but, we can leave them. Your `_config.yml` may now look something like this:
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

    timezone: Australia/Brisbane

    # Build settings
    remote_theme: mmistakes/minimal-mistakes@4.24.0
    plugins:
    - jekyll-remote-theme
    - jekyll-seo-tag
    - jekyll-include-cache

    # Minimal Mistakes - Site Author Configuration
    author:
    name: "James Mount"
    bio: "I am an **amazing** person."
    location: "Somewhere"
    links:
        - label: "Email"
        icon: "fas fa-fw fa-envelope-square"
        url: "mailto:jmount1992@example.com"
        - label: "Twitter"
        icon: "fab fa-fw fa-twitter-square"
        url: "https://twitter.com/jmount1992"
    ```
3. Copy the following into your `index.md`:
    ```markdown
    ---
    layout: single
    author_profile: true
    excerpt: "This post should display a **header with an overlay image**, if the theme supports it."
    header:
    overlay_image: /assets/images/unsplash-image-1.jpg
    caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
    actions:
        - label: "More Info"
        url: "https://unsplash.com"
    ---

    # Lorem Ipsum

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi in ipsum sit amet leo tristique venenatis. Nunc porttitor feugiat gravida. Suspendisse pharetra ac risus id venenatis. Proin tempus eget arcu et euismod. Sed nec eros fermentum, consequat nisi non, dictum velit. Mauris diam ante, consequat vel hendrerit id, ultrices et nibh. Cras a dolor suscipit, aliquam nulla et, lacinia libero.
    ```

4. Add a `_data/navigation.yml` file with the following contents:
    ```yaml
    main:
    - title: About
      url: /about
    ```

5. As we touched the configuration and gemfile. Rebuild and serve the website locally. Check out the results.

The information for what add into `_config.yml`, `index.md`, and `_data/navigation.yml`, came from reading the minimal mistakes documentation. You should read the documentation associated with the theme. It will help you understand what it can be used for and, how much hacking you would need to do for it to fit your purpose. Speaking of hacking, let's revert the previous steps to go back to the Cayman theme and hack that theme a little bit to better suit our purposes. To revert, 

1. Change the remote theme in tag in `_config.yml` back to cayman: `remote_theme: pages-themes/cayman@v0.2.0`
2. In `index.md` comment out the front matter and set the layout to default. It should look something like this:
    ```markdown
    ---
    layout: default
    # author_profile: true
    # excerpt: "This post should display a **header with an overlay image**, if the theme supports it."
    # header:
    #   overlay_image: /assets/images/unsplash-image-1.jpg
    #   caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
    #   actions:
    #     - label: "More Info"
    #       url: "https://unsplash.com"
    ---

    # Lorem Ipsum
    ```
3. You will need to rebuild and serve the website to see the changes locally: `bundle exec jekyll serve`

## Hacking Themes


