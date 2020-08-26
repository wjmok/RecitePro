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
**2\. 数据对照表**

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





