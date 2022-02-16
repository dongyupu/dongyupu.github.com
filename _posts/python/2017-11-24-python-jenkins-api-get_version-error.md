---
layout: post
title: "Jenkins: BadHTTPException: Error communicating with server"
subtitle:   "python-jenkins"
date:       2017-11-24 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

[There appears to be a minor bug in the get_version() function.](https://stackoverflow.com/questions/40253108/get-jenkins-badhttpexception-when-try-to-get-version-info-from-jenkins-run-on-do)



If the Jenkins server is setup with security enabled and IF the anonymous user doesn't have the 'Overall Read' rights then you need to pass the authorization key to urlopen()

i.e. in the file 'jenkins/__init__.py', the get_version() function needs to be changed from:

~~~python
def get_version(self):
    try:
        request = Request(self._build_url(''))
        request.add_header('X-Jenkins', '0.0')
        response = urlopen(request, timeout=self.timeout)
~~~

To:


~~~python
def get_version(self):
    try:
        request = Request(self._build_url(''))
        if self.auth:
            request.add_header('Authorization', self.auth)
        request.add_header('X-Jenkins', '0.0')
        response = urlopen(request, timeout=self.timeout)
~~~

To find where this module is on your system, run python and enter:

~~~python
import jenkins
jenkins.__file__
~~~

(I have reported the bug to the module authors)
