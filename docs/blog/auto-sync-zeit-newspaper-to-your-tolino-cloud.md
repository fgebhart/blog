---
title: Auto-sync ZEIT Newspaper to your Tolino Cloud
description: Automatically Sync your ZEIT Newspaper to your Tolino Cloud using Selenium and GitHub Actions
comments: true
tags:
  - ebook
  - github actions
  - guide
  - newspaper
  - python
  - selenium
---

# How to automatically sync your ZEIT Newspaper to your Tolino Cloud?

This guide will outline the steps needed to set up a scheduled syncing mechanism that downloads the [ZEIT](https://premium.zeit.de/)
newspaper and uploads it to your [Tolino cloud](webreader.mytolino.com/). This mechanism will run once per week on GitHub
Actions, so you don't have to worry about execution or deployment. The automatism is based on [Selenium](https://selenium-python.readthedocs.io/),
which takes care of clicking the relevant browser buttons for you. 

<figure markdown>
  ![ZEIT Tolino Cover Picture](https://i.imgur.com/l5OJSUv.jpg){ width="500", loading=lazy }
  <figcaption>Let's make reading the newspaper on your Tolino even more fun!</figcaption>
</figure>


## Motivation

Reading the newspaper on an e-book reader device has many advantages. First, the device is small and can be used almost
everywhere, while the printed edition usually comes in a not-so-small format. Second, depending on the version of your
device, it might come with inbuilt background light, which allows for reading in gloomy areas. Third, in the case of
[ZEIT](https://premium.zeit.de/), the digital [`epub`](https://en.wikipedia.org/wiki/EPUB) edition does not even contain
any advertisement at all (by the time of writing - let's hope it stays like this). And fourth, having a newspaper in a
digital format enables searching through the document very easily.

While e-book readers became more convenient over the past few years, using them for reading the newspaper is still not
very common. One huge disadvantage is the missing integration of publishers with the ebook reader cloud providers. Taking
care of manually uploading the newspaper to the Tolino cloud is cumbersome and probably leads to the fact of reading
less. While this seems [automated for the Amazon Kindle](https://premium.zeit.de/faq/e-reader#Kindle-automatischer-Versand),
there is [no automatism for Tolino devices in place](https://premium.zeit.de/faq/e-reader#EPUB-Uebertragen). This guide
will bridge this gap.


## Guide

This guide will explain the usage of [zeit-on-tolino](https://github.com/fgebhart/zeit-on-tolino), a little application I
built recently. The app is based on [GitHub Actions](https://github.com/features/actions) and [Selenium](https://selenium-python.readthedocs.io/).
It requires a few manual steps to set up the syncing mechanism. Once configured, it will run in a [cron](https://en.wikipedia.org/wiki/Cron)-based
fashion to download your `epub` newspaper and subsequently upload it to your Tolino cloud every Wednesday evening (after
the release was published).


!!! Attention
    This guide assumes the following **prerequisites** to be fulfilled:

    1. You are able to log in at [premium.zeit.de](https://premium.zeit.de/) and download the `epub` edition of the ZEIT
       newspaper.
    2. You do own a [Tolino e-book reader device](https://mytolino.com/) and have access to the [Tolino cloud](https://webreader.mytolino.com/).


### Fork the `zeit-on-tolino` GitHub Repository

Go to the [zeit-on-tolino](https://github.com/fgebhart/zeit-on-tolino) repo and hit the fork button (or follow
[this link](https://github.com/fgebhart/zeit-on-tolino/fork)).

![Fork GitHub Repo](https://i.imgur.com/PkTTMbV.png){ loading=lazy }

On the upcoming `Create a new fork` page, fill the form with values of your choice and hit the `Create Fork` button.


### Allow GitHub Actions to Run

Once the forking was successful, navigate to the GitHub Actions settings page (Settings → Actions → General) of your repo
and allow all actions and reusable workflows. This step is required to allow the execution of GitHub Actions from the
upstream `zeit-on-tolino` repo in your forked repo.

![Allow GitHub Actions](https://i.imgur.com/llKDjso.png){ loading=lazy }


### Add Tolino and ZEIT Credentials to GitHub Repo Secrets

For the application to down- and upload the `epub` file, your login credentials for both Tolino cloud and ZEIT are
needed. This step shows how to add them to your repository secrets. Note, that [repository secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
are kept secret and can only be accessed by the repository owner and within the GitHub Actions workflows.

Navigate to the Actions Secrets page of your repo (Settings → Secrets → Actions) and add a new secret by clicking on the
`New repository secret` button:

![Add Repo Secret](https://i.imgur.com/ue6hM3r.png){ loading=lazy }

The following secrets are required:

| Secret Name             | Description  |
| ----------------------- | ------------ |
| `TOLINO_PARTNER_SHOP`   | The name of the Tolino partner shop, e.g. `thalia` (see [supported shops](https://github.com/fgebhart/zeit-on-tolino#which-tolino-partner-shops-are-supported)) |
| `TOLINO_USER`           | The username (usually an email address) you use for logging into the Tolino cloud |
| `TOLINO_PASSWORD`       | The associated password |
| `ZEIT_PREMIUM_USER`     | Your ZEIT premium username |
| `ZEIT_PREMIUM_PASSWORD` | The associated password |

Once you are done entering the five required secrets, they are ready to be used within the GitHub Action workflow. Your
list of repo secrets should now look like this:

![Repo Secrets List](https://i.imgur.com/K29SEkH.png){ loading=lazy }


### Execution of the Periodic GitHub Actions Sync Workflow

Everything should be in place by now and the sync mechanism should be triggered every Wednesday evening. To inspect the
exact time the workflow is scheduled to be executed, have a look at [the configuration file of the workflow](https://github.com/fgebhart/zeit-on-tolino/blob/main/.github/workflows/sync_to_tolino_cloud.yml#L5-L7).

To check whether the workflow is executed in your repo, navigate to the `Periodic Sync` workflow in the `Actions` tab
(Actions → Workflows → Periodic Sync). Note, that the Periodic Sync workflow offers to be triggered manually, by clicking
on the `Run Workflow` button. This is potentially useful, after initially setting up the project.

![Workflow Dispatch](https://i.imgur.com/1WJOV5a.png){ loading=lazy }

I hope you were able to successfully set up the auto-syncing – happy to receive comments and feedback. 🐙

-------------------------------------------------------------------------------------------------------------------------

## Troubleshooting

In case some of the above steps do not work as expected, I'd suggest using the comment section of this blog post for
discussions.

For frequently asked questions check out the [FAQ section of `zeit-on-tolino`](https://github.com/fgebhart/zeit-on-tolino#faq).

If you happen to have issues with your workflow run, feel free to [open an issue](https://github.com/fgebhart/zeit-on-tolino/issues/new)
for further investigation.
