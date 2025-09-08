---
layout: post
title: On writing self-hostable software
description: ", well"
---

Last two and something years I'm orbiting the self-hosting sphere. I installed lots of different pieces of software on my home server, some are still there, working, some didn't really stick. A bit more than a year ago I even started my own free open source self-hostable project, and, surprisingly, it gained some traction, so I had a privilege of looking at self-hostable software not just as a user, but also as an author and maintainer of one. And at some point I started noticing patterns, which I wanted to share with you. Might be useful if you're considering launching your own FOSS self-hostable project.

## Keep it simple, silly

That should be probably the title of this article, but, well, it it isn't. Still applies to pretty much everything I'm about to say, though.

The installation simplicity is of the essence. I've seen some pretty interesting tools and libraries but if I'm asked to run `pip install` and then some other commands, you probably lost me. Docker is the universal way to distribute and run self-hosted software, so even if you never really touched this ground, please, find time and build the simplest Dockerfile possible, automate the docker image build with Github Actions and provide a docker-compose.yml file. Your running istructions should not be more complex than "Copy the file, run `docker compose up`". Otherwise all the people who don't feel comfortable running your code with python, ruby or php directly on their machine will be lost to you as users, probably forever.

## Defaults all the way down

Your compose file should have default values. If your software allows some kind of tweaking or configuration (timezone, distance unit, dark/light theme, db name and password, etc.), provide defaults that make sense. Move it in user settings within the application, if you can. Again, it will make it easier for a user to run your software. Defaults might not be safe (they are defaults) or consider every edge case. if someone liked your software, they will change them to their convenience later.

## Environment variables

Just put them all in your compose file. Yes, you may face users who will complain about having too big of a compose file, still, just put them there. Anyone proficient enough with docker who can ask about moving env variables to `.env` file is capable of doing it themself, and having it all in one place will be easier for you to maintain and for new users to kickstart your application.

## Docs & Readme

To me useful, your README.md should contain:
- Name and short description of your project
- 1-3 screenshots of the UI, if applicable
- Short instruction on how to get the app up and running

Everything else is an extra. This extra may or may not include examples of configuration for different use cases (e.g. different reverse proxies), guides and acknowledgements. I'd suggest keep readme minimal and put everything else in appropriate place at your documentation.

By the way, provide some kind of documentation. It may be a separate website or a directory in your repo with .md files, or a Github wiki, doesn't really matter. What matters is it should be easy to navigate, have clear page titles and cover most important aspects of your software.

## Ways to backup

If your software provides database of any kind, make sure in your docs you have an instruction on how to backup and restore the data. Don't overcomplicate it: a list of commands on how to create a database backup and restore it should be enough. Your application doesn't have to have a built-in backup mechanism, again: keep it simple. At least in the beginning.

## Choose a right license

...or don't. Choosing a proper license is hard. Changing a license down the stream seems nearly impossible due to the requirement to have an agreement to change the license from all the contributors. Licensing in general is a complex topic, but at least spend 15 minutes thinking what you want to allow to do with your software and what you don't want to allow. Then google for an appropriate license.

You can also just publish your software without one. It's also an option. This might be useful: [https://choosealicense.com/](https://choosealicense.com/)

## Share your work

If you're not building for yourself, you need feedback. Share your work in r/opensource and r/selfhosted, you'll immediately understand if what you're building is valuable or not, and if so, what issues people experience with it. Share as early as possible, and then make regular posts with updates. Be yourself and for the love of god, don't ask ChatGPT to write the post for you. Do it yourself.

## Have fun

You're doing it because you couldn't _not do it_, after all. Make sure to keep balance between tedious stuff and fun, or you risk facing a burnout and nobody really wants that. Also remember, nobody can demand anything from you unless they pay for it and you agreed to their term. You're not the support service for your project. You don't have to stress out because a stranger on the interned isn't satisfied with what you built for free in your spare time.


## There's more to that

Of course, those above are not all the stuff you can apply to development, publishing and maintaining free open source self-hosted software. There are best practices, security considerations, million and one different opinions on how to do stuff better (spoiler: somebody will come to your project issues on Github and will suggest rewriting everything in Rust). We can't cover them all in this tiny article. But they exist. But you don't have to worry about them all in the beginning. You can also actively choose not to worry about them all later on.
