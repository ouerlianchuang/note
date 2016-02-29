# Testalent

标签： 设计文档

---

## 简介
> 我们把整个Testalent项目分为两个单页应用，userApp和answerApp。userApp包含所有用户行为，包含注册、登陆、资料修改、添加编辑考试、编辑考题、考生管理。answerApp是答题部分，用于考生答题。

## userApp

### 整体设计
我们基于vue实现单页应用，把整个userapp写作一个路由管理的vue实例，应用级状态包含已登陆，已激活，未登录。基于项目需求，划分如下路由映射组件

+ router
    +  /
        + /entry/:login
        + /entry/:signup
        + /manage
            + /manage/:testId/questions
            + /manage/:testId/students
        + /profile
+ components
    + entry.vue(切换login/sign up的本地状态)
    + manageList.vue(json:test/student/question,editBox.vue)
    + profile.vue

+ sessionStorageJson
    + profile
    + 考试信息
    + 考题信息
    + 考生信息

重定向
*  -> /entry/login -> /manage
router.beforeEach() -> login or manage

```
//伪代码
{
    data: ->
        return {
            state:login/sign up,
            post-login-url: ...,
            post-sign-up--url: ...,
            data: {},
            ...
        }
    ready: ->
        ...focus()
    methods: {
        submit: (state)->
    }
}
```


## answerApp

router.beforeEach() -> 检查时间
+ router
    + /pre
    + /examing
        + /examing/:questionId
    + /post
+ components
    + nav
    + questionlist
    + questioninfo(router-view)
        + 类型划分

+ sessionStorageJson
    + 已填写未保存内容（切换题目时保存
+ visibilitychange

## 构建系统及目录结构

> 基于lalaravel的生成目录，只写前端部分

线上依赖 jquery jsiaunui vue.js vue-router.js vuex.js
使用webpack、gulp构建及自动化项目

+ gulp
    + minifycss
    + uglify
    + webpack
    + rename
    + md5
    + del
+ webpack
    + vue
    + coffee
    + less
    + html-webpack-plugin
    + extract-text-webpack-plugin

配置好的g＋w构建如下目录

+ /app
    + /views
        + user.html
        + answer.html
+ /assets
    +  /less
    +  /components
        + /user
        + /answer
    +  /views
        + /user
        + /answer
    +  user-main.coffee
    +  user-routers.coffee
    +  user-vuex.coffee
+ /public
    + /template
    + /js
    + /css
    + /img
    + /front
    + vendors.js
    + user.js
    + answwr.js
+ /jisunui



## 接口需求
### entry

#### POST /login

data

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
name | string | 用户名 | 是
password | string | 用户密码 | 是

响应：

成功：200
响应格式：JSON

```
{
    "status": "success",
    "data": null
}
```

失败：200
响应格式：JSON

```
{
    "status": "fail",
    "data": "{Fail Message}"
}
```
错误：200
响应格式：JSON

```
{
    "status": "error",
    "message": "{Error Message}"
}
```

#### POST /register

data

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
name | string | 用户名 | 是
password | string | 用户密码 | 是
agency | string | 机构名 | 是
email | string | 用户邮箱 | 是

~~phone_number | string | 用户电话 | 否~~

响应：

成功：200
响应格式：JSON

```
{
    "status": "success",
    "data": null
}
```

失败：200
响应格式：JSON

```
{
    "status": "fail",
    "data": "{Fail Message}"
}
```
错误：200
响应格式：JSON

```
{
    "status": "error",
    "message": "{Error Message}"
}
```
### manage

#### GET /exam/list

data

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
user_id | string | 用户id | 是

响应：

成功：200
响应格式：JSON

```
{
    "status": "success",
    "data": {examlist}
}
```
#### POST /exam/add
data

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
user_id | string | 用户id | 是
title | string | 考试名 | 是
introdction | string | 考前信息 | 是
notification | string | 考中信息 | 是
start_time | string | 开始时间 | 是
end_time | string | 结束时间 | 是

#### POST /exam/update
data

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
exam_id | string | 考试id | 是
user_id | string | 用户id | 是
title | string | 考试名 | 是
introdction | string | 考前信息 | 是
notification | string | 考中信息 | 是
start_time | string | 开始时间 | 是
end_time | string | 结束时间 | 是


#### POST /exam/delete
data

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
exam_id | string | 考试id | 是
user_id | string | 用户id | 是

### profile

#### GET /profile
data

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
user_id | string | 用户id | 是

#### POST /update/profile

#### POST /update/email

#### POST /update/phone_number

### question

#### GET /exam/questions


#### POST /exam/question/add

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
exam_id | string | 考试id | 是
title | string | 题目标题 | 是
score | string | 分数 | 是
order | string | 排序 | 是
type | string | 开始时间 | 是
tried | string | 答题次数 | 是
passed | string | 通过人数 | 是
content | string | 题目信息 | 是

#### POST /exam/question/update

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
exam_id | string | 考试id | 是
question_id | string | 考试id | 是
title | string | 题目标题 | 是
score | string | 分数 | 是
order | string | 排序 | 是
type | string | 开始时间 | 是
tried | string | 答题次数 | 是
passed | string | 通过人数 | 是
content | string | 题目信息 | 是


#### POST /delete test question

名称 | 类型 | 定义 | 必需
------ | ------ | ------ | ------
exam_id | string | 考试id | 是


### candidates

#### GET /exam/candidates
#### POST /exam/candidates/add
#### POST /exam/candidates/delete



失败：200
响应格式：JSON

```
{
    "status": "fail",
    "data": "{Fail Message}"
}
```
错误：200
响应格式：JSON

```
{
    "status": "error",
    "message": "{Error Message}"
}
```

### answer

#### Get /exam-information
// 机构信息 考试名 考试时长 考试开始倒计时 考前说明
// 机构信息 考试名 考中信息 考试开始时间结束时间 题目列表 考生信息
// 机构信息 考试名 考生信息 成绩
~~#### Get /questions－list~~

#### post /visibilitychangenum/set （exam_id candidate_id）

#### Get /question

#### Post /question/save


## 补充

### 参数命名规范

管理员：user user_id
考生： candidates candidate_id
考试： exams  exam_id
题目： questions question_id
答题： answer
// postAnswer {exam_id,question_id,candidate_id,candidate_answer}
// getQuestion {exam_id,question_id}
//getQuestionStatic {exam_id,question_id}

### 会议补充

