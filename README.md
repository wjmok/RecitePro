# 背诵诗词小程序项目说明

### 目录

- 功能概述
- 系统架构说明
- 权限控制
- 关键功能实现说明
- 数据对照表


---

**1\. 功能概述**

&emsp;&emsp;
项目的用户类型分为：爱好者、学生、家长、老师、班主任，平台管理员
项目主要功能包括：
包含权限管理、用户管理、类别管理、文章管理、背诵报告管理等基本模块；
接入科大讯飞接口

---

**2\. 系统架构说明**


```flow
st=>start: Start:>https://www.zybuluo.com
io=>inputoutput: verification
op=>operation: Your Operation
cond=>condition: Yes or No?
sub=>subroutine: Your Subroutine
e=>end

st->io->op->cond
cond(yes)->e
cond(no)->sub->io
```

```seq
小程序/cms->api（后端服务器）: http预请求
Note left of 小程序/cms: 前后端分离api架构
Note left of 小程序/cms: 小程序参照微信小程序官方开发标准
Note left of 小程序/cms: cms采用vue2.5.2+webpack3.6.0

api（后端服务器）->>小程序/cms: http允许请求
Note right of api（后端服务器）: 后端代码架构thinkphp5.0
小程序/cms-->api（后端服务器）: http请求
api（后端服务器）-->小程序/cms: http请求返回json

api（后端服务器）->mysql: sql请求
mysql->api（后端服务器）: sql返回数据
Note right of mysql: mysql8.0，所有数据表没有任何表关联设置
```

---
**3\. 权限控制**

整个项目的权限控制的起点在于token的权限验证。
```flow
st=>start: 用户
op=>operation: 登陆验证
cond=>condition: Yes or No?
e=>end: 返回token

st->op->cond
cond(yes)->e
cond(no)->op
```

不同类型的用户采用不同的验证方式，验证成功后都会获得相同格式的token。
前端用户发起的重要请求均要携带token。后台会在处理请求起步阶段通过token获取用户信息，进而实现控制用户请求内容的效果。
不同类型的用户对相关的数据限制均会按照功能清单的要求，在业务代码中实现。

---
**4\. 关键功能实现逻辑**

> * 错误率的计算。通过正则算法，将编辑得文章正文拆解成有单句组成得数组，利用循环结合正则匹配在背诵转义内容中是否存在前后完全一致的单句内容。
> * 排名算法。因数据动态需要，决定采取每次在获取背诵报告列表时，通过sql的排序算法动态生成排名并插入返回数据中，故而在数据表中没有体现响应字段。
> * 文件命名规则。通常我们会首先按照年月生成不同的文件夹，最终文件的命名采取真实姓名+身份证后6位+时间戳



---
**5\. 数据对照表**

通用字段中大部分会在所有表中重复出现，部分数据表会出现passage类似字段，均为预留字段，
数据表中会有部分数据表通过字段区分不同类型数据。设计原因有两个。
其一，部分类型数据存在关键的相同信息且信息字段相当于主键，或者完全重复的接口调用。user表就是这种情况。如果分表会造成冗余和数据错乱，并且降低查询效率。
其二，部分类型数据太少，或者存在子父级等强关联。如school表。分表也会造成查询效率低下，多次查询，内存无意义消耗。

### 通用字段说明

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| id | int(11)| 主键：该数据ID |
| listorder | int(11) | 自定义排序 |
| img_array | varchar(100) | 图片组 |
| create_time | int(11) | 创建时间 |
| update_time | int(11) | 更新时间 |
| delete_time | int(11) | 删除时间 |
| mainImg | text | 主图 |
| thirdapp_id | int(11) | 关联thirdapp |
| user_no | varchar(255) | 关联user |
| user_type | tinyint(2) | 0,家长、学生、爱好者;1.班主任、任课老师;2,平台; |
| status | tinyint(2) | 状态:1正常；-1删除 |



### user表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| login_name | varchar(100) | 登录名 |
| password | varchar(255) | 密码 |
| nickname | varchar(255) | 微信昵称 |
| openid | varchar(50) | 微信openid |
| headImgUrl | varchar(9999) | 微信头像 |
| role | int(11) | 权限角色 |
| primary_scope | int(11) | 权限级别：60.平台;30.班主任;20.任课老师;10.家长、学生、爱好者 |
| user_type | tinyint(2) | 0,家长、学生、爱好者;1.班主任、任课老师;2,平台; |
| user_no | varchar(255) | 用户编号 |



### user_info表

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ |
| behavior | tinyint(2) | 身份:0.爱好者;1.学生;2.家长;3.任课老师;4.班主任； |
| name | varchar(255) | 姓名 |
| phone | varchar(255) | 手机号 |
| gender | varchar(255) | 性别0男1女 |
| idCard | varchar(255) | 身份证号码 |
| address | varchar(255) | 地址 |
| class | varchar(255) | 班级 |
| school | varchar(255) | 班级 |
| province | varchar(255) | 省份 |
| city | varchar(255) | 地级市 |
| country | varchar(255) | 县级市 |
| relation | varchar(255) | 亲属关系 |
| wechatNo | varchar(255) | 微信号码 |
| admissionTime | bigInt(13) | 入学时间 |
| teacher_no | bigInt(13) | 关联任课老师userNo |
| head_teacher_no | bigInt(13) | 关联班主任老师userNo |
| student_no | bigInt(13) | 关联学生userNo |


### article背诵文章表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| title | varchar(100) | 文章标题 |
| menu_id | int(11) | 文章分类 |
| description | varchar(255) | 描述 |
| author | varchar(255) | 作者 |
| dynasty | varchar(255) | 朝代 |
| content | text | 正文 |



### label表

| 字段 | 类型 | 说明 |
| ------ | ------ | ------ |
| title | varchar(40) | 菜单名称 |
| description | varchar(255) | 描述 |
| parentid | int(11) | 父级菜单ID |



### school表

| 字段 | 类型 | 说明 |
| ------ | ------ | ------ |
| title | varchar(40) | 名称 |
| description | varchar(255) | 描述 |
| address | varchar(255) | 地址 |
| parentid | int(11) | 父级菜单ID |
| type | tinyint(2) | 1,学校;2,班级; |
| province | varchar(255) | 省份 |
| city | varchar(255) | 地级市 |
| country | varchar(255) | 县级市 |



### report表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| errRate | decimal(2) | 错误率 |
| content | text | 背诵转义后内容 |
| start_time | bigInt(13) | 开始时间 |
| end_time | bigInt(13) | 结束时间 |
| relation_id | varchar(255) | 关联文章id |
| record_url | text | 背诵录音地址 |
| type | tinyInt | 0非班级作业1班级作业 |



### message表作业

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| description | varchar(255) | 作业描述 |
| deadline | bigint | 截止日期 |
| relation_id | varchar(255) | 文章id |
| relation_label | varchar(255) | 分类文章id |
| relation_class | varchar(255) | 关联班级 |
| relation_school | varchar(255) | 关联学校 |





