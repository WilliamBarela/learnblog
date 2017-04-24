---
layout: post
title: "Marketplace: The Ruby CLI App"
date: 2017-04-23 09:44:00 -0400
---

Recently I had the chance to learn how to make a Ruby command line interface (CLI) gem. The choice to make one that would allow me to get my daily news feed from APM Marketplace was a no brainer. 

Ever since I was young I have loved listening to the radio. My local public broadcasting station would play NPR and I would enjoy listening to Morning Edition, Marketplace Morning Report, and FreshAir. Some years back I discovered Tunein of which I have been a fan evermore. While living in South Korea, I realized how much I missed listening to Marketplace to get my daily news fix on the latest events in the economy. Since the regularly scheduled airings don't work very well when you live in a different time zone, I started to regularly listen to the Marktplace podcasts on Tunein. However, there was only one problem: the podcast which my wife and I love listening to are always one day behind the most current. 

So, charged with the task of making a Ruby CLI Gem, my mission was clear. However, instead of just using Nokogiri to scrape the latest stories and `puts` them to the console, I chose to make a robust object oriented program which also parses [www.marketplace.org](www.marketplace.org) for the audio segments. In my CLI application, you can read, listen or even listen and read to the top articles of the day. When you select to listen to an article, Chrome in the app mode will open a fixed size window to ensure a quality user experience.

This is a screenshot of my working CLI. You can check out the repo [here](https://github.com/WilliamBarela/marketplace-cli-app). You can also listen to a walkthrough of the program on [YouTube](https://www.youtube.com/watch?v=Q3AnP8OslEc&t=262s)</br></br>
![](https://raw.githubusercontent.com/WilliamBarela/WilliamBarela.github.io/master/img/marketplace-ruby-cli.jpg)
