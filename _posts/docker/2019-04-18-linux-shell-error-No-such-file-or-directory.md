---
layout: post
title: "[FATAL tini (6)] exec /entrypoint.sh failed: No such file or directory"
subtitle:   ""
date:       2019-04-18 16:45:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

## [FATAL tini (6)] exec /entrypoint.sh failed: No such file or directory

- 在Windows上编辑shell脚本迁移到linux平台是需要注意文件的格式，否则会出现命名文件在存在，权限也对，但是执行时却提示了上面的错误

- 使用 vi 打开文件，输入：`set ff` 显示 `fileformat=dos`
- 需要重新设置一下格式，输入：`set ff=unix` 或者安装 `dos2unix`把脚本格式化为 `UNIX` 格式即可
