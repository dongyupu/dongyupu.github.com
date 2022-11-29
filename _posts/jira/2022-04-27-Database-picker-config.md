---
layout: post
title: "Jira Stop a hung Bulk Edit or Reindex task "
subtitle:   "JIRA private API /rest/projectconfig/1/migrationStatus/#id"
date:       2022-04-27 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

https://community.atlassian.com/t5/Jira-questions/Stop-a-hung-Bulk-Edit/qaq-p/902554


## Following suggestion needs to be run by the user who executed the task. In addition the user must be a JIRA Administrator in order to delete the task.

* If necessary grant temporary admin permission, delete task, then remove admin permission.
  * URL below applies to bulk edits as well as project changes:
  * There is a internal REST point to stop tasks:

  * DELETE /rest/projectconfig/1/migrationStatus/#id
    * "#id" can be taken from the progress page's URL.URL below applies to bulk edits as well as project changes



http://your_jira.com/plugins/servlet/restbrowser#/resource/projectconfig-1-migrationstatus-id/DELETE
