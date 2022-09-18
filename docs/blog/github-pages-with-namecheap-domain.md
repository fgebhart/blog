---
tags:
  - guide
  - github pages
  - dns
comments: true
description: Configure a custom domain obtained from Namecheap for your static website hosted on GitHub Pages
title: GitHub Pages with Namecheap Domain
---

# How to Use a Custom Namecheap Domain with GitHub Pages?
This post will guide you through the different steps needed to configure a custom domain you obtained from
[Namecheap](https://www.namecheap.com/) with your static website hosted on GitHub Pages.

!!! Note
    This guide focuses on [Namecheap](https://www.namecheap.com/) as a domain name provider. The overall approach is
    likely similar to other domain name providers, however, the steps outlined below are specific to Namecheap and
    cannot be applied to other domain name providers directly.


## Motivation
Sometimes you want your GitHub Page to be reached via a custom domain to provide a catchy name or to put it next to your
existing website by using a subdomain. Note, that there are a [bunch of resources out there](#external-links) already,
but it seemed like none of them puts all the required pieces into a single place, thus I came up with this guide.

## Example
Consider the following example: The GitHub repo of this blog is located at `github.com/fgebhart/blog` and the default
domain for its corresponding GitHub Page would be `fgebhart.github.io/blog`. Now as you can see, I obtained the domain
`fgebhart.dev` from Namecheap and used the subdomain `blog.fgebhart.dev` as the custom domain for this blog.


## Guide
Let's configure a custom domain for your GitHub Page.

!!! attention
    This guide assumes the following **prerequisites** to be fulfilled:

    1. You have a GitHub repository set up and its corresponding static GitHub Page is published at the default domain 
      name (i.e. `<user>.github.io/<repo>`)
    3. You bought a domain on [Namecheap](https://www.namecheap.com/) and can configure it via the
      [Namecheap Dashboard](https://ap.www.namecheap.com/)


### Namecheap Configuration

Head over to your [Namecheap Dashboard](https://ap.www.namecheap.com/) 

### GitHub Pages Configuration


## External Links
* [GitHub docs on managing custom domains with GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site) (Visited 2022-09-18)
* [Namecheap Knowledgebase on linking your domain to GitHub Pages](https://www.namecheap.com/support/knowledgebase/article.aspx/9645/2208/how-do-i-link-my-domain-to-github-pages/) (Visited 2022-09-18)
