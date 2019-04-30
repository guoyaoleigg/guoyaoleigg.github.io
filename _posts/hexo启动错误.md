---

title: hexo启动错误
date: 2019-04-28 12:44:54
tags: 问题处理

---
在hexo博客部署和启动的时候，遇到提示YAML的错误，仔细检查 **_config.yml** 文件，并没有错误。仔细检查是 博客.md 的标签头少了一个空格。
```shell
ERROR Process failed: _posts/安装hexo.md
YAMLException: can not read a block mapping entry; a multiline key may not be an implicit key at line 4, column 1:
```
WindosPowerShell 并没有打印出来 _posts/安装hexo.md 以后可以把错误日志拷贝出来。