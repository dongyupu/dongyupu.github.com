---
layout: post
title: "atlssian git tutorials"
subtitle:   "atlssian git tutorials"
date:       2017-11-07 020:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-06.jpg"
---
[atlssian git tutorials](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud)

# Git basics

* Git is a free and open source version control system, originally created by Linus Torvalds in 2005. Unlike older centralized version control systems such as SVN and CVS, Git is distributed: every developer has the full history of their code repository locally. This makes the initial clone of the repository slower, but subsequent operations such as commit, blame, diff, merge, and log dramatically faster.

* Git also has excellent support for branching, merging, and rewriting repository history, which has lead to many innovative and powerful workflows and tools. Pull requests are one such popular tool that allow teams to collaborate on Git branches and efficiently review each others code. Git is the most widely used version control system in the world today and is considered the modern standard for software development.

# How Git works
* Here is a basic overview of how Git works:

  * 1.Create a "repository" (project) with a git hosting tool (like Bitbucket)
  * 2.Copy (or clone) the repository to your local machine
  * 3.Add a file to your local repo and "commit" (save) the changes
  * 4."Push" your changes to your master branch
  * 5.Make a change to your file with a git hosting tool and commit
  * 6."Pull" the changes to your local machine
  * 7.Create a "branch" (version), make a change, commit the change
  * 8.Open a "pull request" (propose changes to the master branch)
  * 9."Merge" your branch to the master branch

# [Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)

### What is a successful Git workflow?

* When evaluating a workflow for your team, it's most important that you consider your team’s culture. You want the workflow to enhance the effectiveness of your team and not be a burden that limits productivity. Some things to consider when evaluating a Git workflow are:

  * Does this workflow scale with team size?
  * Is it easy to undo mistakes and errors with this workflow?
  * Does this workflow impose any new unnecessary cognitive overhead to the team?


# [Git LFS](https://www.atlassian.com/git/tutorials/git-lfs)
* What is Git LFS?
* Git is a distributed version control system, meaning the entire history of the repository is transferred to the client during the cloning process. For projects containing large files, particularly large files that are modified regularly, this initial clone can take a huge amount of time, as every version of every file has to be downloaded by the client. Git LFS (Large File Storage) is a Git extension developed by Atlassian, GitHub, and a few other open source contributors, that reduces the impact of large files in your repository by downloading the relevant versions of them lazily. Specifically, large files are downloaded during the checkout process rather than during cloning or fetching.







