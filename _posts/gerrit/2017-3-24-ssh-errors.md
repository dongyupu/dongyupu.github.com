---
layout: post
title: "ssh: Could not create directory '/home/SYSTEM/.ssh' and cygheap base mismatch detected."
subtitle:   "on windows jenkins run as SYSTEM ,so cygwin sh will use SYSTEM as current user"
date:       2017-03-24 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-06.jpg"
---
# on windows jenkins run as SYSTEM ,so cygwin sh will use SYSTEM as current user

+ need a dir /home/SYSTEM/.ssh and chown -R SYSTEM:SYSTEM /home/SYSTEM

~~~bash
++ ssh -p 29418 Admin@10.2.48.75 gerrit ls-projects
Could not create directory '/home/SYSTEM/.ssh'.
Unable to negotiate with 10.2.48.75 port 29418: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1
~~~ 

+ cygheap base mismatch detected . 
 <br> install openssh in cygwin ,don't use the windows version .

~~~bash 
++ ssh -p 29418 Admin@10.2.48.75 gerrit ls-projects
      2 [main] ssh (2128) C:\Program Files\OpenSSH\bin\ssh.exe: *** fatal error - cygheap base mismatch detected - 0x180305408/0x180304408.
This problem is probably due to using incompatible versions of the cygwin DLL.
Search for cygwin1.dll using the Windows Start->Find/Search facility
and delete all but the most recent version.  The most recent version *should*
reside in x:\cygwin\bin, where 'x' is the drive on which you have
installed the cygwin distribution.  Rebooting is also suggested if you
are unable to find another cygwin DLL.
~~~

