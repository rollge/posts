---
title: "解决Docker Ctrl+p 不生效问题"
date: 2023-10-13T10:47:32+08:00
draft: false
tags: [docker, linux]
description: "使用docker过程中，如果进入docker终端后，ctrl+p无法生效，应该如何解决呢？"
---

使用docker过程中，如果进入docker终端（`docker exec`）后，ctrl+p无法生效，应该如何解决呢？

经过查阅资料，修复方法如下：

修改docker配置文件（`~/.docker/config.json`），如果没有此文件，则创建即可，添加如下内容：

```json
{
    "detachKeys": "ctrl-z,z"
}
```

如果 config.json 中还有其他条目，则将 detachKeys 条目添加到最后。
例如:

```json
{
    "HttpHeaders": {
        "User-Agent": "Docker-Client/19.03.11 (linux)"
    },
    "detachKeys": "ctrl-z,z"
}
```

**podman:**
```sh
# ~/.config/containers/containers.conf
[engine]
detach_keys="ctrl-z,z"
```


参考资料：

- [https://docs.docker.com/engine/reference/commandline/cli/](https://docs.docker.com/engine/reference/commandline/cli/)
- [https://stackoverflow.com/questions/20828657/docker-change-ctrlp-to-something-else](https://stackoverflow.com/questions/20828657/docker-change-ctrlp-to-something-else)


---
作者：卷哥Rollge

发表时间：2023年10月13日 10:47:32

勘误：[me@rollge.com](mailto:me@rollge.com)

**转载请注明来源，本文遵循 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)**