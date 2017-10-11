---
layout: post
title: 2017-10-7-Linux服务器SSH登录失败
category: Linux
tag: [java,设计模式]
---

###ssh登录服务器时发生错误  

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:pGssPb/mG4ShC+TwMf+BzeTCs46T+XEfjz4j2Zaz7W8.
Please contact your system administrator.
Add correct host key in /Users/jiangwenbin/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/jiangwenbin/.ssh/known_hosts:3
ECDSA host key for 115.159.209.80 has changed and you have requested strict checking.
Host key verification failed.
```

解决办法：
cd ~/.ssh
vi konwn_hosts
删除服务器IP相关信息
