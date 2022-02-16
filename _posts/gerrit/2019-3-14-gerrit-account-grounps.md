---
layout: post
title: "gerrit access control:Account groups"
subtitle:   "group access control"
date:       2019-03-14 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---

## Account Groups
* Account groups contain a list of zero or more user account members, added individually by a group owner. Any user account listed as a group member is given any access rights granted to the group.

* Every group has one other group designated as its owner. Users who are members of the owner group can:

  * Add users and other groups to this group

  * Remove users and other groups from this group

  * Change the name of this group

  * Change the description of this group

  * Change the owner of this group, to another group

* It is permissible for a group to own itself, allowing the group members to directly manage who their peers are.

* Newly created groups are automatically created as owning themselves, with the creating user as the only member. This permits the group creator to add additional members, and change the owner to another group if desired.

* It is somewhat common to create two groups at the same time, for example Foo and Foo-admin, where the latter group Foo-admin owns both itself and also group Foo. Users who are members of Foo-admin can thus control the membership of Foo, without actually having the access rights granted to Foo. This configuration can help prevent accidental submits when the members of Foo have submit rights on a project, and the members of Foo-admin typically do not need to have such rights.
