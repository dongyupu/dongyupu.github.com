---
layout: post
title: "关于JIRA plugin的授权机制 "
subtitle:   "关于JIRA plugin的授权机制 "
date:       2014-10-27 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
## Jira Software plugins
最近在折腾jira agile和
Git Integration Plugin for JIRA
本来公司这边是有jira的授权的，但是jira agile和
Git Integration Plugin for JIRA
是需要单独购买授权许可的。没办法，先装个适用下。
搜索结果：
http://ju.outofmemory.cn/entry/32260

这篇文章很详细的分析了jira的授权许可机制。

for

### Git Integration Plugin for JIRA
~~~ python
PythonWin 2.7.3 (default, Apr 10 2012, 23:31:26) [MSC v.1500 32 bit (Intel)] on win32.
Portions Copyright 1994-2008 Mark Hammond - see 'Help/About PythonWin' for further copyright information.
>>> import base64
>>> license = 'AAABDw0ODAoPeNqNkE9rg0AQxe/7KRZ6VtSamAoegkqx+CekaU+FsF0mdlpdZXYNybevmtCW0kMPC2+ZefN7MzdFp3glDfcC7tyGzl24WPA42XHPcX2WgJaEvcFORfdoeKYM1CSmP980Q42KHzriD9l2/SVeQp4eRTPMXawc2leg6vCkgXRkuSwmmCuJMBBNEMt1LC9g357I0ABMdq19wr5B9WG/Iwm7RjOL/Sj2/Qy3hTR4hIuhQQlKw/PImYZ4rBA4xlVCSUhPPdL5B9K1vCWrqBYK9QUqWynZY1pG47NyP1g5K3/J8svQv/3X4u7cQylaiOKqKNJtnK3z/6WHMR71hPq6wWYg+SY0/D7NJ5cOjNUwLAIUTKASU75rhUFVNR4g7bLYhx4wfJ4CFHhMsR39fG+UlVU7iuahhTM1GPUjX02dp'
>>> len(license)
433
>>> s = base64.b64decode(license)
Traceback (most recent call last):
  File "", line 1, in
  File "C:\Python27\lib\base64.py", line 76, in b64decode
    raise TypeError(msg)
TypeError: Incorrect padding
>>> lens=len(license)
>>> lenx = lens - (lens % 4 if lens%4 else 4)
>>> print lenx
432
>>> print license[:lenx]
AAABDw0ODAoPeNqNkE9rg0AQxe/7KRZ6VtSamAoegkqx+CekaU+FsF0mdlpdZXYNybevmtCW0kMPC2+ZefN7MzdFp3glDfcC7tyGzl24WPA42XHPcX2WgJaEvcFORfdoeKYM1CSmP980Q42KHzriD9l2/SVeQp4eRTPMXawc2leg6vCkgXRkuSwmmCuJMBBNEMt1LC9g357I0ABMdq19wr5B9WG/Iwm7RjOL/Sj2/Qy3hTR4hIuhQQlKw/PImYZ4rBA4xlVCSUhPPdL5B9K1vCWrqBYK9QUqWynZY1pG47NyP1g5K3/J8svQv/3X4u7cQylaiOKqKNJtnK3z/6WHMR71hPq6wWYg+SY0/D7NJ5cOjNUwLAIUTKASU75rhUFVNR4g7bLYhx4wfJ4CFHhMsR39fG+UlVU7iuahhTM1GPUjX02d
>>> s = base64.b64decode(license[:lenx])
>>> s
"\x00\x00\x01\x0f\r\x0e\x0c\n\x0fx\xda\x8d\x90Ok\x83@\x10\xc5\xef\xfb)\x16zV\xd4\x9a\x98\n\x1e\x82J\xb1\xf8'\xa4iO\x85\xb0]&vZ]ev\r\xc9\xb7\xaf\x9a\xd0\x96\xd2C\x0f\x0bo\x99y\xf3{37E\xa7x%\r\xf7\x02\xee\xdc\x86\xce]\xb8X\xf08\xd9q\xcfq}\x96\x80\x96\x84\xbd\xc1NE\xf7hx\xa6\x0c\xd4$\xa6?\xdf4C\x8d\x8a\x1f:\xe2\x0f\xd9v\xfd%^B\x9e\x1eE3\xcc]\xac\x1c\xdaW\xa0\xea\xf0\xa4\x81td\xb9,&\x98+\x890\x10M\x10\xcbu,/`\xdf\x9e\xc8\xd0\x00Lv\xad}\xc2\xbeA\xf5a\xbf#\t\xbbF3\x8b\xfd(\xf6\xfd\x0c\xb7\x854x\x84\x8b\xa1A\tJ\xc3\xf3\xc8\x99\x86x\xac\x108\xc6UBIHO=\xd2\xf9\x07\xd2\xb5\xbc%\xab\xa8\x16\n\xf5\x05*[)\xd9cZF\xe3\xb3r?X9+\x7f\xc9\xf2\xcb\xd0\xbf\xfd\xd7\xe2\xee\xdcC)Z\x88\xe2\xaa(\xd2m\x9c\xad\xf3\xff\xa5\x871\x1e\xf5\x84\xfa\xba\xc1f \xf9&4\xfc>\xcd'\x97\x0e\x8c\xd50,\x02\x14L\xa0\x12S\xbek\x85AU5\x1e \xed\xb2\xd8\x87\x1e0|\x9e\x02\x14xL\xb1\x1d\xfd|o\x94\x95U;\x8a\xe6\xa1\x8535\x18\xf5#_M\x9d"
>>> ord(s[:4].decode('UTF-32BE'))
271
>>> licenseText, licenseSig = s[4:4 + 271], s[4 + 271:]
>>> licenseText
"\r\x0e\x0c\n\x0fx\xda\x8d\x90Ok\x83@\x10\xc5\xef\xfb)\x16zV\xd4\x9a\x98\n\x1e\x82J\xb1\xf8'\xa4iO\x85\xb0]&vZ]ev\r\xc9\xb7\xaf\x9a\xd0\x96\xd2C\x0f\x0bo\x99y\xf3{37E\xa7x%\r\xf7\x02\xee\xdc\x86\xce]\xb8X\xf08\xd9q\xcfq}\x96\x80\x96\x84\xbd\xc1NE\xf7hx\xa6\x0c\xd4$\xa6?\xdf4C\x8d\x8a\x1f:\xe2\x0f\xd9v\xfd%^B\x9e\x1eE3\xcc]\xac\x1c\xdaW\xa0\xea\xf0\xa4\x81td\xb9,&\x98+\x890\x10M\x10\xcbu,/`\xdf\x9e\xc8\xd0\x00Lv\xad}\xc2\xbeA\xf5a\xbf#\t\xbbF3\x8b\xfd(\xf6\xfd\x0c\xb7\x854x\x84\x8b\xa1A\tJ\xc3\xf3\xc8\x99\x86x\xac\x108\xc6UBIHO=\xd2\xf9\x07\xd2\xb5\xbc%\xab\xa8\x16\n\xf5\x05*[)\xd9cZF\xe3\xb3r?X9+\x7f\xc9\xf2\xcb\xd0\xbf\xfd\xd7\xe2\xee\xdcC)Z\x88\xe2\xaa(\xd2m\x9c\xad\xf3\xff\xa5\x871\x1e\xf5\x84\xfa\xba\xc1f \xf9&4\xfc>\xcd'\x97\x0e\x8c\xd5"
>>> licenseSig
'0,\x02\x14L\xa0\x12S\xbek\x85AU5\x1e \xed\xb2\xd8\x87\x1e0|\x9e\x02\x14xL\xb1\x1d\xfd|o\x94\x95U;\x8a\xe6\xa1\x8535\x18\xf5#_M\x9d'
>>> map(ord, '\r\x0e\x0c\n\x0f')
[13, 14, 12, 10, 15]
>>> import zlib
>>> licenseTextOriginal = zlib.decompress(licenseText[5:])
>>> print licenseTextOriginal
#Mon Oct 27 03:09:55 CDT 2014
Description=Git Integration Plugin for JIRA for JIRA\: Evaluation
NumberOfUsers=-1
CreationDate=2014-10-27
Evaluation=true
com.xiplink.jira.git.jira_git_plugin.active=true   #对于我来说主要是获取这个名字
licenseVersion=2
MaintenanceExpiryDate=2014-11-26
Organisation=cmcc
SEN=SEN-L4780846
LicenseExpiryDate=2014-11-26
LicenseTypeName=COMMERCIAL
com.xiplink.jira.git.jira_git_plugin.enterprise=true
PurchaseDate=2014-10-27

>>> 

然后参照网上生成的可以用的key：

Description=Git Integration Plugin for JIRA for JIRA\: Commercial,
NumberOfUsers=-1,
CreationDate=2014-08-01,
Evaluation=false,
com.xiplink.jira.git.jira_git_plugin.LicenseEdition=ENTERPRISE,
licenseVersion=2,
MaintenanceExpiryDate=2099-01-01,
Organisation=saper,
com.xiplink.jira.git.jira_git_plugin.active=true,
SEN=SEN-L4390388,
ServerID=BQJE-RNSR-9NMG-JDWN,
LicenseExpiryDate=2099-01-01,
LicenseTypeName=COMMERCIAL,
PurchaseDate=2014-08-01

~~~
更新license后就算破解成功了。



