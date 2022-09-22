---
title: Pull Request Bot Commands List
description: 
published: true
date: 2022-09-16T03:07:10.057Z
tags: 
editor: markdown
dateCreated: 2022-08-31T08:33:52.553Z
---

# Overview

Command              | Explain                                
---------------------|---------------------------------------------------
`/check`             | Retrigger all CI checks                
`/check jobname`     | Retrigger *jobname*                    
`/assign @username`  | Assign @username for the given PR      
`/review @username`  | Add @username to reviewer              
`/merge`             | Merge PR if meed all merge requirement 
`/+1` or `/approve`  | Add a approve label (maintainer-only)  

## `/check jobname` Usage 

Assume you want to retrigger a job says `build-distribution / check_job / archlinux-build`, then you need to use `/check archlinux-build` or `/check check_job/archlinux-build` to retrigger it.

# Tampermonkey Helper Script

To make life easier, you can also use the following Tampermonkey script by:

1. Install Tampermonkey extension for your web browser.
2. Add `https://raw.githubusercontent.com/deepin-community/deepin-chatopt-script/main/chatopt.js` as a user script 
3. After that, you'll be able to see related button under the comment input in the comment tab of a GitHub Pull Request page.

# Merge Requirement

1. At least one valid approval (from people who have [at leaset *write* permission](https://docs.github.com/en/organizations/managing-access-to-your-organizations-repositories/repository-roles-for-an-organization#permissions-for-each-role)).
2. All CI check passed.

Please note: `/approve` is only for marking purpose, for actual code review, you still need to go through the GitHub's code review process.
