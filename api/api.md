# api文档

```
Content-Type: application/json;charset=UTF-8
```

## 1 用户验证

### 1.1 用户注册

```
post /api/auth/regist
```

#### 请求参数：

| 参数名 | 类型   | 说明          |      |
| ------ | ------ | ------------- | ---- |
| uid    | string | 用户id/用户名 |      |
| passwd | string | 用户密码      |      |

示例：

```
{
	'uid': 'cmkk',
	'passwd': 'r12345'
}
```

#### 返回数据

示例1：

```
{
	stat: 'ok',
	msg: '注册成功！'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```



### 1.2 用户登录

```
post /api/auth/login
```

#### 请求参数：

| 参数名 | 类型   | 说明          |      |
| ------ | ------ | ------------- | ---- |
| uid    | string | 用户id/用户名 |      |
| passwd | string | 用户密码      |      |

示例：

```
{
	'uid': 'cmkk',
	'passwd': 'r12345'
}
```

#### 返回数据

示例1：

```
{
	stat: 'ok',
	msg: '登陆成功！'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

示例3：

```
{
	stat: 'User_Not_Exist',
	msg: '用户户不存在'
}
```

示例4：

```
{
	stat: 'User_Passwd_Incorrect',
	msg: '账户或密码错误'
}
```

说明：用户登录或注册成功时，后台需要生成一个token并将其放入请求头的`Cookies`中，也即`Cookies: token=生成的token`，在之后的每次请求中都会携带这个token，后台只需从请求头中取得该token并解析，如此可实现用户验证功能。



### 1.3 退出登录

```
post /api/auth/logout
```

#### 请求参数：

```
无
```

说明：退出登录操作无需参数，后台根据请求头中的token解析出用户后对其进行token进行过期处理

#### 返回数据

示例1：

```
{
	stat: 'ok',
	msg: '登出成功'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

---



## 2 用户

### 2.2 获取用户信息

```
post /api/user/info
```

#### 请求参数：

| 参数名 | 类型   | 说明          |
| ------ | ------ | ------------- |
| uid    | string | 用户id/用户名 |

示例：

```
{
	'uid': 'cmll',
}
```

说明：后台通过token验证身份，验证通过则返回uid的用户信息，全用户通用（即获取自己的信息时也需要带上uid）；当uid为空时，返回当前用户（token所指用户）信息。

#### 返回数据

示例1：

```
{
	stat: 'ok',
	data: {
		// 用户信息
		uid: 'cmll',
		nickname: 'cmLily',
		avatar: 'lili.png',
	}
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

### 2.3 修改密码

```
put /api/user/passwd
```

#### 请求参数：

| 参数名    | 类型   | 说明   |
| --------- | ------ | ------ |
| newPasswd | string | 新密码 |
| oldPasswd | string | 旧密码 |

示例：

```
{
	newPasswd: 'r12345',
	oldPasswd: '123456'
}
```

#### 返回数据

示例1：

```
{
	stat: 'ok',
	msg: '密码修改成功'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

### 2.4 修改昵称

```
put /api/user/nickname
```

#### 请求参数：

| 参数名   | 类型   | 说明 |
| -------- | ------ | ---- |
| nickname | string | 昵称 |

示例：

```
{
	'nickname': 'cmKKK',
}
```

#### 返回数据

示例1：

```
{
	stat: 'ok',
	msg: '成功'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

### 2.5 修改头像

```
put /api/user/avatar
```

#### 请求参数：

| 参数名 | 类型   | 说明         |
| ------ | ------ | ------------ |
| avatar | string | 图片所在路径 |

示例：

```
{
	avatar: 'lily.png',
}
```

#### 返回数据

示例1：

```
{
	stat: 'ok',
	msg: '成功'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

### 2.6 获取用户关注

```
post /api/user/follows
```

#### 请求参数：

| 参数名    | 类型   | 说明          |
| --------- | ------ | ------------- |
| uid       | string | 用户id/用户名 |
| pageSize  | number | 分页大小      |
| pageIndex | number | 当前页        |
|           |        |               |

示例：

```
{
	uid: 'cmkk',
	pageSize: 20,
	pageIndex: 1
}
```

说明：依据uid搜索，对所有用户通用（搜索自己时，也需要带上自己的uid）

#### 返回数据

示例1：

```json
{
	stat: 'ok',
	data: {
		items: [
			{
				uid: 'cmkk',
                nickname: 'cmKangkang',
                avatar: 'kk.png'
			},
            {
				uid: 'cmll',
                nickname: 'cmlily',
                avatar: 'll.png'
			},
		],
		total: 2 // 搜索结果总数
	}
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

### 2.7 获取用户粉丝

```
post /api/user/followers
```

#### 请求参数：

| 参数名    | 类型   | 说明          |
| --------- | ------ | ------------- |
| uid       | string | 用户id/用户名 |
| pageSize  | number | 分页大小      |
| pageIndex | number | 当前页        |
|           |        |               |

示例：

```
{
	uid: 'cmkk',
	pageSize: 20,
	pageIndex: 1
}
```

说明：依据uid搜索，对所有用户通用（搜索自己时，也需要带上自己的uid）

#### 返回数据

示例1：

```json
{
	stat: 'ok',
	data: {
		items: [
			{
				uid: 'cmkk',
                nickname: 'cmKangkang',
                avatar: 'kk.png'
			},
            {
				uid: 'cmll',
                nickname: 'cmlily',
                avatar: 'll.png'
			},
		],
		total: 2 // 搜索结果总数
	}
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```



### 2.8 获取用户收藏文章

```
post /api/user/stars
```

#### 请求参数：

| 参数名    | 类型   | 说明          |
| --------- | ------ | ------------- |
| uid       | string | 用户id/用户名 |
| pageSize  | number | 分页大小      |
| pageIndex | number | 当前页        |
|           |        |               |

示例：

```
{
	uid: 'cmkk',
	pageSize: 20,
	pageIndex: 1
}
```

说明：依据uid搜索，对所有用户通用（搜索自己时，也需要带上自己的uid）

#### 返回数据

示例1：

```json
{
	stat: 'ok',
	data: {
		items: [
			{
                aid: 'jiwjisw',
                title: 'readme',
                authour: 'cmkk',
                subTitle: '说明',
                banners: ['msms.png', 'dhwyudhw.png'],
                tags: ['js'],
                view: 100,
                ctime: 114218327832,
                mtime: 114356676767,
                
            }
		],
		total: 1 // 搜索结果总数
	}
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```



### 2.8 获取用户文章

```
post /api/user/articles
```

#### 请求参数：

| 参数名    | 类型   | 说明          |
| --------- | ------ | ------------- |
| uid       | string | 用户id/用户名 |
| pageSize  | number | 分页大小      |
| pageIndex | number | 当前页        |
|           |        |               |

示例：

```
{
	uid: 'cmkk',
	pageSize: 20,
	pageIndex: 1
}
```

说明：依据uid搜索，对所有用户通用（搜索自己时，也需要带上自己的uid）

#### 返回数据

示例1：

```json
{
	stat: 'ok',
	data: {
		items: [
			{
                aid: 'jiwjisw',
                authour: 'cmkk',
                title: 'readme',
                subTitle: '说明',
                banners: ['msms.png', 'dhwyudhw.png'],
                tags: ['js'],
                view: 100,
                ctime: 114218327832
                mtime: 114356676767
            }
		],
		total: 1 // 搜索结果总数
	}
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```



### 2.9 关注用户

```
post /api/user/follow
```

#### 请求参数：

| 参数名 | 类型   | 说明          |
| ------ | ------ | ------------- |
| uid    | string | 用户id/用户名 |

示例：

```
{
	uid: 'cmll',
}
```

说明：解析token，将uid代表的用户添加至token代表的用户的follows中

#### 返回数据

示例1：

```json
{
	stat: 'ok',
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```



### 2.9 取消关注

```
post /api/user/unfollow
```

#### 请求参数：

| 参数名 | 类型   | 说明          |
| ------ | ------ | ------------- |
| uid    | string | 用户id/用户名 |

示例：

```
{
	uid: 'cmll',
}
```

说明：解析token，将uid代表的用户从token代表的用户的follows中移除

#### 返回数据

示例1：

```json
{
	stat: 'ok',
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```



---



## 3 文章

### 3.1 预览/浏览文章

```
get /api/article/:aid
```

#### 请求参数：

无

示例：

```
get /api/article/hsdwuhdwu
```

说明：后台需要在在url中取得参数aid，以此返回对应的文章

#### 返回数据

示例1：

```json
{
	stat: 'ok',
	data: {
		aid: 'jiwjisw',
        authour: 'cmkk',
        title: 'readme',
        subTitle: '说明',
        content: '# readme dede'
        banners: ['msms.png', 'dhwyudhw.png'],
        tags: ['js'],
        view: 100,
        ctime: 114218327832,
        mtime: 114356676767,
		star: true
	}
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

### 3.2  新建文章

```
post /api/article/new
```

#### 请求参数：

| 参数名   | 类型     | 说明     |
| -------- | -------- | -------- |
| title    | string   | 文章标题 |
| subTitle | string   | 副标题   |
| banners  | string[] | banner图 |
| tags     | string[] | 标签     |
| content  | string   | 文章正文 |

示例：

```
{
	title: 'readme',
    subTitle: 'readme dedefe',
    content: '# readme',
    banners: ['123.png'],
    tags: ['js', 'react']
}
```

说明：解析token得到作者。

#### 返回数据

示例1：

```json
{
	stat: 'ok'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

### 3.3  编辑文章

```
put /api/article/:aid
```

#### 请求参数：

| 参数名   | 类型     | 说明     |
| -------- | -------- | -------- |
| title    | string   | 文章标题 |
| subTitle | string   | 副标题   |
| banners  | string[] | banner图 |
| tags     | string[] | 标签     |
| content  | string   | 文章正文 |

示例：

```
{
	title: 'readme',
    subTitle: 'readme dedefe',
    content: '# readme',
    banners: ['123.png'],
    tags: ['js', 'react']
}
```

说明：解析token得到作者，判断是否有权限；在url中获取aid，找到需要更新的的文章。

#### 返回数据

示例1：

```json
{
	stat: 'ok'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

### 3.4  删除文章

```
delete /api/article/:aid
```

#### 请求参数：

无

示例：

```
delete /api/article/djuedeuef
```

说明：解析token得到作者，在url中获取aid，判断是否具有权限删除，能则删除。

#### 返回数据

示例1：

```json
{
	stat: 'ok'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

示例3：

```
{
	stat: '',
	msg: '权限不足'
}
```

### 3.5  收藏文章

```
post /api/article/unstar
```

#### 请求参数：

| 参数名 | 类型   | 说明         |
| ------ | ------ | ------------ |
| aid    | string | 收藏的文章id |
|        |        |              |

示例：

```
{
	aid: 'wdwdwswsdw'
}
```

说明：解析token得到作者，将。

#### 返回数据

示例1：

```json
{
	stat: 'ok'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

### 3.6  取消收藏文章

```
post /api/article/star
```

#### 请求参数：

| 参数名 | 类型   | 说明         |
| ------ | ------ | ------------ |
| aid    | string | 收藏的文章id |
|        |        |              |

示例：

```
{
	aid: 'wdwdwswsdw'
}
```

说明：解析token得到作者，若该文章已经收藏，则取消收藏，否则收藏。

#### 返回数据

示例1：

```json
{
	stat: 'ok'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```

## 4 文件

### 4.1 图片上传
```
post /api/file/upload
```

#### 请求参数：

| 参数名 | 类型 | 说明           |
| ------ | ---- | -------------- |
| file   | File | 图像二进制文件 |
|        |      |                |

示例：

```json
// 请求体data的值为FormData
file: file文件
```

说明：后台获取上传文件并存储后，返回静态文件地址，在图片展示时需要根据该路径请求文件。

#### 返回数据

示例1：

```json
{
	stat: 'ok',
    data: 'hduwed.png'
}
```

示例2：

```
{
	stat: 'Internal_Server_Error',
	msg: '内部错误'
}
```




