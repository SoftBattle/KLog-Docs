## 1. User

用户相关的键：

```
	uid: '用户id',
	nickname: '',
	avatar: '',
    followers: '关注我的人',
    follows: '我关注的',
    stars: '文章收藏',
    articles: '我的文章',
```

文章相关的键：

```
aid: 文章id
author: 作者
title: 文章标题
subTitle: 副标题
tags: 标签，数组
banners: banner图，数组
view: 浏览数，数字
ctime: 创建时间，date
mtime: 最近编辑时间，date
```



1. 用户主页左边tab

   ```ts
   interface UserBasicInfo {
       uid: string,
       nickname: string,
       avatar: string
   }
   ```

2. 文章基础信息，不包含content

   ```ts
   interface ArticleBasicInfo {
       aid: string
       title: string
       subTitle: string
       banners: string[]
       view: number
       ctime: Date
       mtime: Date
       tags: string[]
   }
   ```

3. 文章详细信息，包含content与收藏信息

   ```ts
   interface Article {
       aid: string
       authour: string
       title: string
       subTitle: string
       content: string
       banners: string[]
       tags: string[]
       view: number
       ctime: Date
       mtime: Date
    star: boolean // 收藏与否
   }
   ```
   
4. 新建文章

   ```ts
   interface NewArticle {
   	title: string
       subTitle: string
       content: string
       banners: string[]
       tags: string[]
   }
   ```

   