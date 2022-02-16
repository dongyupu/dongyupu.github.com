---
layout: post
title: "upgrade library org.apache.commons.discovery to version 0.5 "
subtitle:   "jira/confluence Reinstall pugin error exception java.lang.IllegalStateException: The bundle is uninstalled. "
date:       2020-04-7 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
### upgrade library org.apache.commons.discovery to version 0.5
### NOTE: This suggestion is for JIRA Server.
[Link](https://jira.atlassian.com/browse/JRASERVER-59570)
### Summary
* JIRA (and Confluence) is using obsolete version 0.2 dated to 2003.
* When using AXIS library bundled in JIRA within add-on calls to AXISClient are not usable after deployment.

### Steps to reproduce
  * Install add-on with AXISClient
  * Invoke some WS calls
  * Reinstall add-on
  * Invoke same WS calls
### Expected behaviour
  * WS calls returns same results

### Actual behaviour
  * WS calls after reinstall fail with exception java.lang.IllegalStateException: The bundle is uninstalled.

## Workaround
  * Restart JIRA

## Proposed solution
Upgrade library org.apache.commons.discovery to version 0.5
