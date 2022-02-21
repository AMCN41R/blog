---
title: Why are you not using Azure Static Web Apps?!
tags:
  - azure
  - hosting
  - webapps
  - staticwebapps
photos: /images/azure-static-web-apps/header.jpg
date: 2022-02-21 19:44:59
---


So, I came across this blog the other day...

### [Host Static Web Sites for Free in Azure](https://techcommunity.microsoft.com/t5/healthcare-and-life-sciences/host-static-web-sites-for-free-in-azure/ba-p/3152200)
> _Have a simple site that’s just html, css, javascript and other static content?  You can host it for free in Azure. Not only is it free to host it is much easier to maintain than most off the self content management systems._

This sparked my interest, but then the kicker...

> _What about SSL? As soon as you add a custom domain SWA will automatically get an SSL cert that matches your domain. It will renew as needed- you need to do nothing else!_

#### Wait, what?

I have a [personal website](https://www.alexmcnair.net) that is a static site built with react (using [create-react-app](https://create-react-app.dev)). Its stored on [github](https://github.com/AMCN41R/about-me), no actions or any other CI/CD scripts. At the moment, I pay for hosting and have yet to sort an SSL certificate so I thought I'd check it out.

## Wow!
I was not prepared for how quick and easy it was! I mean, it took me literally 5 minutes.

1. Sign into the [Azure portal](https://portal.azure.com)
2. Go to _**Static Web Apps**_
3. Click _**Create**_

<img src="/blog/images/azure-static-web-apps/create.png" style="margin:0;">

4. Select a subscription, resource group and name for your static web app
5. Select the free tier and most appropriate region
6. Select Github and sign in
7. Select your repo and branch
8. Select your build details

<img src="/blog/images/azure-static-web-apps/github-config.png" style="margin:0;">

9. Create it.
10. And that's basically it!

As part of the creation process, it
- automatically created a github workflow that builds and deploys my app, including push and PR triggers
- committed this to the repo
- generated a custom hostname and SSL certificate
- deployed and hosted my app

### It's just so simple
I followed a few extra steps to [set up my custom domain](https://docs.microsoft.com/en-us/azure/static-web-apps/custom-domain), and now have my personal site hosted for free on azure!

### Further reading
There are other ways to do this (via VS Code for example), and other basic templates for different frameworks and static site generators. The docs are really helpful...

[Azure Static Web Apps](https://azure.microsoft.com/en-us/services/app-service/static/)
[Azure Static Web Apps Docs](https://docs.microsoft.com/en-us/azure/static-web-apps/)
[Create via the Portal](https://docs.microsoft.com/en-us/azure/static-web-apps/get-started-portal?tabs=vanilla-javascript)
[Configure Custom Domains](https://docs.microsoft.com/en-us/azure/static-web-apps/custom-domain)