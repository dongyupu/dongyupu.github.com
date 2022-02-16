---
layout: post
title: "git clone error:diffie-hellman-group1-sha1"
subtitle:   "no matching key exchange method found by OpenSSH 7.0!"
date:       2015-12-08 13:45:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

当git server上的ssh版本与客户端的版本不一致时可能导致“ no matching key exchange method found by OpenSSH 7.0!”

原因是 [密钥交换算法不匹配](https://wiki.archlinux.org/index.php/Secure_Shell#no_matching_key_exchange_method_found_by_OpenSSH_7.0)

OpenSSH 7.0 also deprecated the key algorithm diffie-hellman-group1-sha1, as we could see in <http://www.openssh.com/legacy.html>

If the client and server are unable to agree on a mutual set of parameters then the connection will fail. OpenSSH (7.0 and greater) will produce an error message like this:

~~~bash
  Unable to negotiate with 127.0.0.1: no matching key exchange method found.
  Their offer: diffie-hellman-group1-sha1
~~~

In this case, the client and server were unable to agree on the key exchange algorithm. The server offered only a single method diffie-hellman-group1-sha1. OpenSSH supports this method, but does not enable it by default because is weak and within theoretical range of the so-called Logjam attack.

The best resolution for these failures is to upgrade the software at the other end. OpenSSH only disables algorithms that we actively recommend against using because they are known to be weak. In some cases, this might not be immediately possible so you may need to temporarily re-enable the weak algorithms to retain access.

For the case of the above error message, OpenSSH can be configured to enable the diffie-hellman-group1-sha1 key exchange algorithm (or any other that is disabled by default) using the KexAlgorithm option - either on the command-line:

~~~bash
	ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 user@127.0.0.1
~~~

+  or in the ~/.ssh/config file:

~~~bash
	Host somehost.example.org
	KexAlgorithms +diffie-hellman-group1-sha1
~~~
