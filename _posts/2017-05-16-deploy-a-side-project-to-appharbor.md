---
layout: post
title: "Deploy a side project on AppHarbor"
description: "Deploy a simple side project on AppHarbor with some troubles."
category: Developer
draft: false
tags:
- development
- appharbor
- side-project
- asp-net
- nancyfx
- github
---

I deployed [Shortr](http://shortr.apphb.com/) to [AppHarbor](https://appharbor.com/).

[ShortUrl](https://github.com/kingsor/ShortURL) is a simple side project I play with in order to try some stuff for future use in another side project.

It's a simple url shortener project created with [NancyFx](http://nancyfx.org/) and [MongoDB](https://www.mongodb.com/).

It's based on [ShortURL](https://github.com/horsdal/ShortURL) project by [Christian Horsdal](https://github.com/horsdal) following his tutorial [Frictionless .NET Web App Development with Nancy](http://www.horsdal-consult.dk/2011/11/frictionless-net-web-app-development.html) with a front-end based on [Shortio](https://github.com/luishendrix92/shortio) project by [Luis Lopez](https://github.com/luishendrix92) following his tutorial [Let's build a URL Shortener with Node, MongoDB and Hapi.js](https://www.codetuts.tech/build-a-url-shortener-node-hapi-js/).

I had some troubles when I moved from testing the app with [ShortUrlDesktopApp](https://github.com/kingsor/ShortURL/tree/master/ShortUrlDesktopApp) project (Nancy.Hosting.Self) to testing the app with [ShortUrlWebApp](https://github.com/kingsor/ShortURL/tree/master/ShortUrlWebApp) project (Nancy.Hosting.Aspnet). 

Trouble with embedded resources from "Content/public" folder in [ShortUrl](https://github.com/kingsor/ShortURL/tree/master/ShortUrl) library.

I found the solution [here](https://groups.google.com/forum/#!msg/nancy-web-framework/N3neO1FJ3Qc/NzooDTVSUFIJ) and I solved on local machine testing with Visual Studio.

After committing everything, I discovered that GitHub doesn't show commits in the contributions graphic grid when made on forked projects.  
Because I use this project as a playground for Webmarks project, I'm not interested to create pull requests to original project. So I wanted to detach my project from the original one.  
On GitHub site [I found that](https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile/#commit-was-made-in-a-fork) you have to contact [GitHub Support](https://github.com/contact).
> To detach the fork and turn it into a standalone repository on GitHub, contact [GitHub Support](https://github.com/contact).

But I have not pull requests on that repo and only the branch master, so I followed [this solution found on StackOverflow](http://stackoverflow.com/a/18390313/2768802):

* verify that my local repo is in sync with the remote one
* delete the repo on GitHub
* create a new repo on GitHub with the same name, this way the origin url is the same
* execute a [force push](https://twitter.com/dbelcham/status/392364417242775552) to the remote repo

Everything was fine.

Then I had to push to the [AppHarbor](https://appharbor.com/) repo from the local repo. But the original project uploaded on AppHarbor was different from the one I'm working on now. So I have to [force push](https://twitter.com/dbelcham/status/392364417242775552) to AppHarbor repo, too. This way also the AppHarbor repo is in sync with my local repo.

At this point AppHarbor build my project and deploy it if there are no errors. The build was fine, but the site didn't work. There was a problem with Nuget packages references to different versions of MongoDB Driver and a missing reference to [System.Runtime.InteropServices.RuntimeInformation](https://www.nuget.org/packages/System.Runtime.InteropServices.RuntimeInformation/) package in the [ShortUrlWebApp project](https://github.com/kingsor/ShortURL/tree/master/ShortUrlWebApp) that was not required running it on my local machine ("It works on my machine"). After some "try and error" steps, finally I was able to see my site working.