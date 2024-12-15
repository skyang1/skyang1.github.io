---
wiki: diary
title: Hexo-stellar配置
---

今天主要问题在文档的配置，按照官方教程无法配置成功，更改项目结构后配置成功。

新的项目结构为：

1. wiki.yml ：文档目录，存放在source/_data目录下- diary

```yaml
- diary
```

2. diary.yml ：日记文档的配置文件，存放到source/_data/wiki目录下

```yaml
name: Diary
title: skyang-学习日记
subtitle: '每个人的独立博客 | Designed by xaoxuu'
tags: 日记
icon: /assets/wiki/stellar/icon.svg
cover: /assets/wiki/stellar/icon.svg
description: skyang学习日记-每天学习的一些零碎的知识，难以整理，写为日记
# repo: xaoxuu/hexo-theme-stellar
search:
  filter: /wiki/stellar/
  placeholder: 在 Stellar 中搜索...
leftbar: 
  - tree
  - timeline_stellar_releases
  - related
# comment_title: '评论区仅供交流，有问题请提 [issue](https://github.com/xaoxuu/hexo-theme-stellar/issues) 反馈。'
# comments:
#   service: giscus
#   giscus:
#     data-repo: xaoxuu/hexo-theme-stellar
#     data-mapping: number
#     data-term: 226
base_dir: /wiki/stellar/
tree:
  '快速开始':
    - index
	- day_20241210
```

3. day_20241210.md ：文档具体内容，存放在myblog/wiki/diary路径下

