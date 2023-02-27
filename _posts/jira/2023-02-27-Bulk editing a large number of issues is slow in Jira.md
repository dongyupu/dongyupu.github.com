---
layout: post
title: "Bulk editing a large number of issues is slow in Jira"
subtitle:   "Bulk editing a large number of issues is slow in Jira"
date:       2023-02-27 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
## [Bulk editing a large number of issues is slow in Jira](https://confluence.atlassian.com/jirakb/bulk-editing-a-large-number-of-issues-is-slow-in-jira-968669347.html)

Platform notice: Server and Data Center only. This article only applies to Atlassian products on the server and data center platforms.

### Problem
Bulk editing issues in Jira is slow. It can take anywhere from 10 minutes to a few hours to bulk edit a large amount of issues. Here are some sample bulk edit times tested on a new/vanilla JIRA server instance:

- We've tested editing issues to add a value to 3 fields and change the priority of all issues, and this is how long this took depending on the number of issues:

  - 100 issues - 7 seconds
  - 500 issues - 32 seconds
  - 1000 issues - 67 seconds
Depending on the changes you are making to all of these tickets, the times can vary. It is acceptable for this to take up to a few minutes. However, in the cases where this is taking too long and the changes are prohibitive, you might be able to speed this up.

### Diagnosis
- Check if this is only occurring in a 3rd party or custom field - if so, get in touch with the plugin vendor for support

- This issue has been encountered when using specific custom fields from the Xray Test Management plugin for Jira.  
- Enable safe mode and see if the issue continues - if the issue goes away then this could be a plugin issue

- Run a background re-index and then test again
- Ensure you are not encountering any performance issues that could be causing this. 
### Solution
- If safe mode helps speed this up, disable each 3rd party individually and test to narrow it down to a specific plugin

- If this is caused by a custom field coming from a 3rd party plugin, contact the plugin vendor for support

- If this is caused by a custom field you have set up, or if you are unable to resolve this otherwise - contact Atlassian Support

  - Do include logs with the com.atlassian.jira.web.action.issue.bulkedit package set to DEBUG logging to add the Bulk Edit progress percentage added in the log, visualizing the slowness of the operation

