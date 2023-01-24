---
title: Getting Started with GitHub
navbar: Guides
layout: guides
key: 1.3
# bump: true

tags:
 - text: 'Required'
   type: 'is-danger'
---

We will use [GitHub](https://github.com/features) for all things related to code in class, including sharing lecture code examples, sharing starter homework and project code, submitting homeworks and projects, and collaborating on projects. It is a software development platform built on top of the `git` software, which is version control system for software.

Use this guide to get started with GitHub for the first time and learn some of the basics of the `git` software it is built upon.

## Setup GitHub Account
{: .page-header }

If you do not yet have a free GitHub account, you need to create one:

<https://github.com/join>

When creating an account for this class (including accounts already created), keep in mind:

  - Choose a professional username.

  - Add information to your profile, including a professional profile photo and brief bio (including your degree and expected graduation date).

  - Add your `@dons.usfca.edu` and `@usfca.edu` emails to your account. Go to ["Settings" » "Access" » "Emails"](https://github.com/settings/emails) to add an email.

      <i class="fas fa-info-circle"></i>&nbsp;The `@usfca.edu` version of your email is an *alias* that forwards to your `@dons.usfca.edu` account. Some university services use this alias, including Canvas and Zoom. That is why you need to add *both* forms of your email address.
      {: .notification }

  - Apply for the [**GitHub Student Developer Pack**](https://education.github.com/pack) after adding your university emails to your account.

You can see the [instructor's profile page](https://github.com/sjengle) for an example. When done with this class, you'll be able to show off your GitHub profile to potential employers.

<i class="fas fa-info-circle"></i>
Using your GitHub username and password authentication [no longer an option for authentication](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/) via the command-line.
{: .notification }

## Setup Repository Access
{: .page-header }

You will need to access your private GitHub repositories on your own local system to run and develop code. There are [two options](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github#authenticating-with-the-command-line) for accessing your private repositories:

  1. **Using SSH, which involves [generating SSH keys per system](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) you want to access GitHub from.** If you already use SSH (for example, to access `stargate.cs.usfca.edu`), then that approach may be more convenient since you likely already have SSH keys generated.

  1. **Using HTTPS, which involves [generating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) (instead of a password).** If you do not already use SSH, I recommend the HTTPS method. It allows more fine-grained control, including when it should expire. With this option, make sure to configure an expiration *after* the end of the semester, and add both "repo" and "workflow" access:

      ![Token Scope]({{ "/images/general/github-token-scope.png" | relative_url }}){: .is-400 }

Either way works; they each have different pros and cons. When you know which method you prefer, go to the [**Authenticating with the command line**](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github#authenticating-with-the-command-line) article on GitHub for details about get started.

You should only need to complete this step **once** for the entire semester!

## Learn Git and GitHub
{: .page-header }

If you are unfamiliar with `git` and GitHub, it is important to self-learn some basics. The operations we will use in class include: commit, push, pull, merge, and branch. The GitHub features we will use in class include releases, issues, and pull requests.

The CS Tutoring Center has recordings of past `git` workshops posted:

  - [**CS Tutoring Center**](https://tutoringcenter.cs.usfca.edu/)

Here are some other fantastic (and free) guides you can look at to learn more:

  - [**GitHub Guides**](https://guides.github.com/)  
    Guides for learning more about GitHub by GitHub.

  - [**GitHub Training and Guides (Videos)**](https://www.youtube.com/githubguides)  
    Videos produced by GitHub for learning Git and GitHub.

  - [**GitHub Learning Lab**](https://lab.github.com/)  
    Hands-on GitHub lessons taught by the friendly GitHub Learning Lab bot.

  - [**Git-SCM**](https://git-scm.com/)  
    Official Git project site (includes tutorials, a reference manual, and the free "Pro Git" ebook).

Here are a couple free resources that will help use `git` with Eclipse:

  - [**GitHub and Eclipse (Video)**](https://www.youtube.com/watch?v=XuuzSaelUzo)  
    Videos produced by GitHub for using GitHub with Eclipse.

  - [**Eclipse EGit Plugin User Guide**](http://wiki.eclipse.org/EGit/User_Guide)  
    User guide for the builtin `git` plugin in Eclipse. Works with GitHub repositories.

You also have free access to [**Linkedin Learning**](https://myusf.usfca.edu/ets/educational-technologies/linkedin) (formerly Lynda.com), which has many technical online tutorials and mini-courses. If you prefer that format, I strongly encourage you to take a look. There are *many* other resources you can find with a simple web search, like this [Really Friendly Git Intro](https://hellowebbooks.com/learn-git/) zine by Tracy Osborn or the [Oh Shit, Git!](https://wizardzines.com/zines/oh-shit-git/) zine by Julia Evans.

<i class="fas fa-code-branch"></i>
Don't just wing it! I recommend you spend some time learning the basics of `git` now, as it is a core skill for anyone in the tech and CS industries. It will also make this class and all CS classes that follow a lot less frustrating.
{: .notification }
