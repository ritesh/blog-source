+++
date = "2016-11-19T22:17:58Z"
description = "My experiences in moving from Jekyll to Hugo"
title = "Hello, Hugo!"
summary = "Finally got around to moving this blog from Jekyll to Hugo and setting up a Cloudflare account with SSL."
categories = ["yak-shaving"]
draft = false
url = "/2016/11/09/hello-hugo/"
+++

I had been running this blog using [Jekyll](https://jekyllrb.com/) for a while now, but didn't quite understand how it all worked. Jekyll is a great tool, and you don't need a whole lot of configuration to get started. If you're using github to host either personal, project or organisation pages, Jekyll is a no-brainer to get started. However, as I'm not much of a Ruby programmer, I didn't quite understand how Jekyll worked and having to `gem install ruby` felt wrong -- especially since it also installs a whole lot of other dependencies that I don't understand, either. Having to get the 'right' version of ruby for jekyll to run is also pretty annoying. After some digging around, I discovered [hugo](http://gethugo.io), which is a static site generator written in Go. I decided to finally take the plunge and move my Jekyll site to Hugo, in the hope that I could eke out at least one blog post about my experience.

### Why, Hugo?
Hugo's written in Go, which means that all you need to get started is a binary of Hugo somewhere in your `$PATH`. That's it. Everything it needs is baked into this statically compiled binary. Go code is easy to follow and the template code used by Hugo is Go's `html/template` which is available as part of the standard library. Besides, all the cool kids today use go for both personal and professional projects and I didn't want to miss out. 

Hugo shares much of the same common philosophy as Jekyll and possibly the hundreds of [static site generators](https://www.staticgen.com/) available today. You write your blog posts in markdown and they are rendered to HTML. The layout of your content and the look of your blog is decided by [community created themes](http://themes.gohugo.io/) that you can either customise or use out of the box.  If you're used to WordPress or similar content management platforms, creating a site like this may seem primitive. Hugo doesn't give you an editor or a point and click interface to add upload and add images, for example. But it's not just for people who are suckers for punishment, it's a great way to maintain your blog entirely from the command line (web based editing is on the roadmap - but I wouldn't hold my breath). If you're the type of person who avoids Finder and uses `mv` & `cp` instead, Hugo is probably for you.

This is not meant to be a Hugo tutorial; if you want to learn how to start out with a Hugo blog or website, try the [official documentation](https://gohugo.io/overview/introduction/). 

### Hugo and Jekyll Github pages
Jekyll works great with Github pages of all kinds as Github automatically recognises Jekyll based sites and automatically "builds" them into great looking static websites. Unfortunately, Github doesn't know what to do with Hugo based sites, so it attempts to build them automatically and fails. When moving from Jekyll to Hugo, this was the first hurdle that I ran into - which seems obvious in hindsight. There's a well written tutorial on using Github for [hosting with Hugo](https://gohugo.io/tutorials/github-pages-blog/) and I'll try and summarise my experience in getting it to work with personal sites i.e. served from the master branch of your `<YOUR_USERNAME>.github.io` repository. 

If you don't already have a `<YOUR_USERNAME>.github.io` repository, you should create one now, whether you plan to use Hugo or not. It's a great way to host content. The most important bit of information that I missed when reading the Hugo Github tutorial was that for personal sites, GitHub will only serve pages from the master branch of the `<YOUR_USERNAME>.github.io` repository. Not the `gh-pages` branch, which I thought was the case. The `gh-pages` branch is only relevant to organisation and project pages. This took me way too long to figure out and was frustrating, even though the tutorial clearly calls it out. RTFM is important, friends. 

To host and serve your user github page, you'll need to set up two repositories. One that stores the source for your blog and the other is the static site itself (e.g. ritesh.github.io). First, you want to create a git repository that is going to store all the contents of your blog. This includes the entire directory structure your site as created by Hugo on first run. Next, you want to add the `<YOUR_GITHUB_USERNAME>.github.io` repository as a git submodule as Hugo's `public` directory. I've added the theme as a submodule too, but this is not stricly required if you don't intend to modify the theme. After you're happy with the way your site looks, you need a way to push the generated site to your `<YOUR_GITHUB_USERNAME>.github.io`. To do this, the following script from the tutorial above works perfectly. The script looks like this, what it does should be self-explanatory. The `public` directory is a git submodule that contains your Hugo generated site:

```
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo # if using a theme, replace by `hugo -t <yourtheme>`

# Go To Public folder
cd public
# Add changes to git.
git add -A

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back
cd ..
```

Each time you run this deploy script a new commit is created in the submodule that contains your rendered site (i.e. public) and changes are pushed to master. This automatically updates your blog or website. 

### Adding SSL/TLS
Github offers free a free SSL certificate for you `<YOURNAME_USERNAME>.github.io` site, but you're in trouble if using a custom domain name as the name on the certificate will not match `yourvanitydomain.coolTLD`. To enable SSL/TLS (without origin SSL), I've used Cloudflare as my DNS servers for *.rsinha.org. Cloudflare can also be configured to provide additional features, such as caching and lots of other knobs and whistles, even on their free plan. When setting up Cloudflare, you can also configure it to rewrite all URLs to HTTPS rather than HTTP. This took me a while to figure out, since some of my assets were being served over HTTP even when I'd configured the assets on my site to point to https://. Chrome prevented the site from rendering correctly due to mixed-content warnings.

### Cost, Benefits
The total cost to host all of this is zero (thanks Github and the Cloudflare free plan!). That is, if you don't count time spent fiddling around to get everything right. To their credit, Hugo has great documentation and you want to RTFM and not skim. In return for time spent, you get a nice static website consisting of bits that you can customize to your heart's content. You also get a site with HTTPS enabled, caching, and free DDoS protection if the two readers of your blog decide to take you down.


### It works!

Everything's set up and works fine for now, but there are a few things that I still need to get right to make my life easier. For one, I can only edit my blog by checking it out somewhere and then writing content. There isn't a web editor that works directly with Hugo that I'm aware of. You can set it up so that a commit to your blog source automatically builds your blog, but I've not set this up. This should be possible with a CI service like Travis. 

[This theme](http://themes.gohugo.io/internet-weblog/) is great, but I still need to tweak some of the metadata generated to my liking. To do that, I need a better understanding of how metadata is generated for each post. For example, hugo can automatically generate metadata items such as title and date. I'm still not sure how I can get it to automatically generate custom slugs and URLs. I'm doing this manually for now and it feels wrong, surely the computer can do this for me!

Overall, I'm quite happy with this setup and I looking to tweak it a little more to my liking as and when I have free time. I'd highly recommend it for anyone looking to move over from Jekyll or for people who don't need self-hosted WordPress sites. It's a fun learning experience and it creates a site that is free from bloat and does not require security patches or other maintenance.

Also, if you're wondering why this is categorised as yak-shaving, read the [Wiktionary article](https://en.wiktionary.org/wiki/yak_shaving) to know more.   




