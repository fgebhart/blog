---
slug: gitHub-pages-with-namecheap-domain
description: Configure a custom domain obtained from Namecheap for your static website hosted on GitHub Pages
comments: true
authors:
  - fgebhart
tags:
  - dns
  - github pages
  - guide
  - namecheap
date: 2022-09-19
categories:
  - Blogging
glightbox: false
---

# How to Use a Custom Namecheap Domain with GitHub Pages?
This post will guide you through the different steps needed to configure a custom [Namecheap](https://www.namecheap.com/)
domain with your static website hosted on GitHub Pages.

<figure markdown>
  ![ZEIT Tolino Cover Picture](https://i.imgur.com/vecomx7.png){ width="400", loading=lazy }
</figure>

<!-- more -->

!!! Note
    This guide focuses on [Namecheap](https://www.namecheap.com/) as a domain name provider. The overall approach is
    likely similar to other domain name providers, however, the steps outlined below are specific to Namecheap and
    cannot be applied to other domain name providers directly.


## Motivation
Sometimes you want your GitHub Page to be reachable via a custom domain to provide a catchy name or to put it next to
your existing website by using a subdomain. Note, that there are a [bunch of resources on this topic out there](#external-links)
already, but it seemed like none of them puts all the required pieces into a single place, thus I came up with this
guide.

## Example
Consider the following example: The GitHub repo of this blog is located at `github.com/fgebhart/blog` and the default
domain for its corresponding GitHub Page would be `fgebhart.github.io/blog`. Now as you can see, I obtained the domain
`fgebhart.dev` from Namecheap and used the subdomain `blog.fgebhart.dev` as the custom domain for this blog.


## Guide
Let's configure a custom domain for your GitHub Page.

!!! Attention
    This guide assumes the following **prerequisites** to be fulfilled:

    1. You have a GitHub repository set up and its corresponding static GitHub Page is published at the default domain 
      name (i.e. `<user>.github.io/<repo>`)
    3. You bought a domain on [Namecheap](https://www.namecheap.com/) and can configure it via the
      [Namecheap Dashboard](https://ap.www.namecheap.com/)


### Namecheap Configuration

Head over to your [Namecheap Dashboard](https://ap.www.namecheap.com/) and hit the `MANAGE` button:

![Namecheap Dashboard](https://i.imgur.com/zCE3uJD.jpg){ loading=lazy }

On the `Domains → Details` page click the `Advanced DNS` tab:

![Namecheap Dashboard](https://i.imgur.com/75pYTsC.png){ loading=lazy }

In the `Host Records` section, you need to add five entries. Four of them are of type `A Record` with Host being `@`
pointing to the GitHub Pages IP addresses. The fifth record will be the actual `CNAME Record` pointing to your default
GitHub Page domain. The below screenshot shows the values needed to map your new custom domain (`blog.fgebhart.dev`
in this case) to the default GitHub Pages domain (`fgebhart.github.io/blog`) as given in the above [example](#example).

![Namecheap Dashboard](https://i.imgur.com/xr4uE3i.png){ loading=lazy }

Copy the values from this table and configure your host records accordingly.

| Type         | Host  | Value              | TTL       |
| ------------ | ----- | ------------------ | --------- |
| A Record     | @     | 185.199.108.153    | Automatic | 
| A Record     | @     | 185.199.109.153    | Automatic | 
| A Record     | @     | 185.199.110.153    | Automatic | 
| A Record     | @     | 185.199.111.153    | Automatic | 
| CNAME Record | blog  | fgebhart.github.io | Automatic | 


!!! Hint
    Both [subdomains and apex domains are supported](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages#supported-custom-domains)
    with GitHub Pages. If you prefer to use the apex domain instead of the subdomain you have to enter `www` instead of
    `blog` in the `Host` column since [`www` is just a usual subdomain](https://stackoverflow.com/questions/20680521/is-www-a-subdomain)
    from DNS perspective.

It usually takes a while until this configuration becomes active. After waiting for one hour verify your DNS settings to
be active by running the following command (replace `blog.fgebhart.dev` with your domain of course):

```bash
dig +noall +answer +nocmd blog.fgebhart.dev
```
Your output should look similar to this one:

```
blog.fgebhart.dev.	1799	IN	CNAME	fgebhart.github.io.
fgebhart.github.io.	3600	IN	A	185.199.110.153
fgebhart.github.io.	3600	IN	A	185.199.111.153
fgebhart.github.io.	3600	IN	A	185.199.108.153
fgebhart.github.io.	3600	IN	A	185.199.109.153
```

If you get the expected output, your Namecheap configuration is in place and we can proceed with the configuration of
GitHub.


### GitHub Pages Configuration

Go to your GitHub repository and navigate to the GitHub Pages settings. For this blog the URL is [https://github.com/fgebhart/blog/settings/pages](https://github.com/fgebhart/blog/settings/pages).
Given the above-mentioned prerequisites, your static website should already be deployed to GitHub Pages and accessible
via the default `github.io` URL (if you still need to set up the deployment, I'd suggest having a look at the [GitHub Actions Workflow of this blog](https://github.com/fgebhart/blog/blob/main/.github/workflows/publish.yml),
check out the [GitHub docs on publishing your website](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
or reading the [docs of mkdocs-material regarding publishing](https://squidfunk.github.io/mkdocs-material/publishing-your-site/)).

Scroll down to the `Custom Domain` section and enter your custom domain into the input field. In the case of this blog
`blog.fgebhart.dev` was entered:

![GitHub Pages Custom Domain Settings](https://i.imgur.com/MGhhZCI.png){ loading=lazy }

!!! Hint
    Again, if you prefer to use your apex domain, simply omit the subdomain and enter e.g. `fgebhart.dev` here.

In case your Namecheap configuration was successful as described above, the GitHub DNS check should pass and you should
be able to hit the `Save` button. It is also strongly recommended to enable HTTPS for your custom domain by checking the
`Enforce HTTPS` box.

After a moment your static website should be accessible via your new domain. 🐙

-------------------------------------------------------------------------------------------------------------------------

## Troubleshooting

### My Website is not Accessible via the Custom Domain

Patience. Updating and propagating new DNS records can take quite some time. This makes iterating a tedious task, but
using the above-mentioned `dig` command helps verify that things are set up as intended.


### Each Commit to my Repo seems to break the configured Domain Routing

In case a push/commit to your repo containing the source code of your static website seems to break the DNS
configuration, you might need to add a CNAME file to your repo. Usually, configuring the custom domain on the GitHub
Pages settings tab will add a CNAME file to the branch from which your static page will be deployed. However, a GitHub
Action could be implemented in such a way, that it would overwrite the CNAME file with every commit. If this is the
case, you would need to add the file manually to your repo. To do so, have a look at the deploy branch of your repo
(usually `gh-pages`) to conclude which folder of your source code branch will be deployed from the deploy branch. In the
root of this folder, you have to add a file called `CNAME` (all uppercase). The content of the file must contain a single
line only, which is the custom URL to point to your GitHub Page. In the case of this blog, [here is the CNAME file](https://github.com/fgebhart/blog/blob/main/docs/CNAME)
which contains `blog.fgebhart.dev`. For more details have a look at the [GitHub Troubleshooting docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages#cname-errors).


## External Links
* [GitHub docs on managing custom domains with GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site) (Visited 2022-09-18)
* [Namecheap Knowledgebase on linking your domain to GitHub Pages](https://www.namecheap.com/support/knowledgebase/article.aspx/9645/2208/how-do-i-link-my-domain-to-github-pages/) (Visited 2022-09-18)
* [Namecheap guide on the different DNS types](https://www.namecheap.com/guru-guides/dns-records/) (Visited 2022-09-19)


*[apex domain]: Apex domains are known as base, root or naked domains. It does not contain any subdomain, e.g. fgebhart.dev
*[DNS]: Domain Name System
*[CNAME]: "Canonical Name" - DNS record type to point the name of your website to your A record
