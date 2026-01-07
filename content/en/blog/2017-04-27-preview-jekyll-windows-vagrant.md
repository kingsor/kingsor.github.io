---
date: "2017-04-27"
summary: How to preview your GitHub Pages (Jekyll) on Windows 10 with Vagrant
categories:
- developer
tags:
- jekyll
- vagrant
- github pages
title: Preview GitHub Pages on Windows 10 with Vagrant
---

I was looking for a way to work with [Jekyll](http://jekyllrb.com/) in my Windows 10 box in order to preview my blog before committing to GitHub. I found that [setting up Jekyll on Windows](http://jekyll-windows.juthilo.com/) is a little tricky so I was looking for a way to run it using [Vagrant](https://www.vagrantup.com/).
Searching on Google I found the [jsturtevant/jekyll-vagrant](https://github.com/jsturtevant/jekyll-vagrant) project by [James Sturtevant](http://www.jamessturtevant.com/) that fits perfectly my need.
The project has a related post: [Running Jekyll in Windows](http://www.jamessturtevant.com/posts/running-jekyll-in-windows/) where James describes his solution.

Here are the steps you need to start working with Jekyll and Vagrant as depicted by James in his post.

## Setup
1. Install [Vagrant](https://www.vagrantup.com/) and [Virtual Box](https://www.virtualbox.org/). If you need help you can read this post: [Getting Started with Vagrant on Windows](https://www.sitepoint.com/getting-started-vagrant-windows/).
2. Clone the [jekyll-vagrant](https://github.com/jsturtevant/jekyll-vagrant) repository

    ```git clone https://github.com/jsturtevant/jekyll-vagrant.git```
3. Open command prompt to location of the ```vagrantfile``` in the new cloned repository and run ```vagrant up```
4. Jekyll and all it's dependencies are installed!

## Existing Jekyll Projects
1. Copy the projects folder to the folder that contains the ```vagrantfile```.  
2. Login to the VM using ```vagrant ssh```

## New Jekyll Projects
1.  Open a command prompt to location of the ```vagrantfile``` and run ```vagrant ssh```
2.  Once in the VM prompt ```cd /vagrant```
3.  Create a new site with ```jekyll new <sitename>```

## Start the Site
1. In the VM prompt ```cd /vagrant/<YourProjectFolder>```
2. Start the Jekyll server ```jekyll serve --force_polling --host 0.0.0.0```
([force polling is required](http://stackoverflow.com/a/23084706/697126) with vagrant because of share)
3. On your host machine you can open any browser and navigate to ```localhost:4000```
4. Work on you project on your host machine and see updates as they happen on ```localhost:4000```


James has also made experiments with [Docker](https://www.docker.com/) that he describes in this post: [Running Jekyll in Windows Using Docker](http://www.jamessturtevant.com/posts/Running-Jekyll-in-Windows-using-Docker/).