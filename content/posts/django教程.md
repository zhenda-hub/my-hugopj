+++
title = 'Django教程'
subtitle = ""
date = 2024-04-30T14:07:14+08:00
draft = true
toc = true
tags = ["web"]
+++

# Django 教程

[toc]

## http 4 种请求和传参方式

## 网站开发逻辑梳理

### 初级阶段，使用 admin 后台快速建站

-   model
-   admin

### 直接编写后台逻辑和前台模板页面

-   url
    -   ‘’
    -   create
    -   \<int:pk\>
    -   \<int:pk\>/update
    -   \<int:pk\>/delete
-   view
    -   FBV
    -   CBV
-   template

    -   页面的主要类型
        -   list
        -   detail
        -   delete_confirm
    -   语法
        -   tags
            -   在 app 内创建 templatetags/demo.py
            -   {% include demo %}
            -   常用语法
                -   {% load xxx %} **在 html 的最开头**
                -   {% extends 'xxx.html' %}
                -   {% include xxx %}
                -   {% block xxx %} {% endblock xxx %}
                -   {% url 'namespace:name' %}
        -   variables
            -   {{ var1 }}
        -   filters
        -   comments

-   form
    -   Form: 灵活设置
    -   ModelForm：针对模型，添加了数据的验证和保存的功能

## view 之上的逻辑

中间件， 钩子

## 具体功能

### 用户和权限和邮箱

用户模块的重要意义： 支持多端用户，千人千面。

### celery 处理异步任务和定时任务

### 缓存

## 前后端分离

-   DRF
    -   FBV
    -   CBV
-   VUE

## 网站部署

配置 nginx 和 uwsgi

## django 源码风格

大量使用 Minin
