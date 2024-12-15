---
wiki: diary
title: npm安装软件包
---



# npm安装软件包

## 全局安装

```shell
nmp install hexo -g
```

安装的软件包在NodeJS安装目录下的global_modules文件夹下



## 本地安装

安装到当前路径的文件夹下

由于没有配置全局环境变量，可以使用 npx 指令使用安装的软件包

```
npx hexo g -d
```

