---
layout: post
title: Convoluted Blogging - Part One
tags:
  - tech
  - windows server
  - nano server
  - docker
  - containers
---

So as mentioned in a previous post, I've been spending a lot of time lately trying to vet different cloud technologies for hosting a simple web app, and I've opted to use this blog as the guinea pig. It's been quite the journey thus far as I've intentionally made this so much more difficult than just paying for a simple Wordpress site and am beginning to question my motives. So, a few notes on the journey thus far.

## Converting the Blog from WordPress to Jekyll

For starters, I exported my previous posts out of Wordpress.com and opted to move to Jekyll. If you aren't familiar with Jekyll, it is a ruby-based app that will generate a static HTML website. I had spent a little time with it before I opted to host on Wordpress.com a few years ago, so decided now was as good a time as any to get my hands dirty with it.

Oh and dirty they got. Being ruby based, I had to get my Windows laptop configured to actually work with all the various gems required for Jeykll and the import tool and that took longer than I'd like to admit. I ultimately lost my mind and opted to build the thing in a Docker container running Windows Server Core with all the ruby dependencies. That worked, but then I started thinking about the vast size of a server core install for all of a few HTML pages, so I started looking at [Nano Server](https://docs.microsoft.com/en-us/windows-server/get-started/getting-started-with-nano-server). If you are unfamiliar with Nano Server and have a desire to do any Windows container work, I highly suggest it. It's much smaller and faster than the other Windows offerings yet comes with a ton of challenges that I'll have to some day write about. The biggest challenge was how to get Ruby installed and running, as I wanted to run the Jekyll dev server for a bit while I tweaked settings, with the idea that I'd ultimately move it over to IIS (static hosting has it's advantages). To solve that problem, I found a great link to a Docker multi-stage build on [GitHub](https://github.com/woodie/nanoserver-ruby) that led me to success with the [Nano container for Jekyll development](https://github.com/matthewrstone/random-dockerfiles/tree/master/nano-jekyll-dev).

## Re-launching the Site

Once I had everything where I wanted, I spun up the most recent Windows Server Core Containers AMI on Amazon Web Services and was able to run the dev site *in production*. You should always run development instances in the production. Ah well, risking issues with a blog that hasn't been updated in a year and change isn't really a big issue for me, so let's live large.

*Side note*: if you just want to run Jekyll and not worry about it, you can create a [GitHub pages](https://pages.github.com/) site and it will host everything beautifully, but since I was trying to work on workflows in the cloud, I took that option off the table. Besides, given the above tomfoolery, that seemed like giving up.

Now that I actually had a...thing that served pages that resembled a blog, I took a bit of time to find another theme and stuck with [Beautiful Jekyll](http://deanattali.com/beautiful-jekyll/). A few more gratuitous edits and a switch to my [IIS container](https://github.com/matthewrstone/random-dockerfiles/tree/master/nano-iis-blog) and we were off and running.

The blog was back. Now I just needed to actually add content...and a deployment strategy. Thus began the process of trying to find the best way I could go from a git push to an updated website.