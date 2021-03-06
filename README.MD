[TOC]

# 介绍

*Typora QCloud Cos* 是一个基于 Python 开发的 Typora 图片上传插件。可以自动地将 Typora 中的图片上传到腾讯云对象存储。

---

特性：
* 支持本地图片和网络图片上传（将图片复制并粘贴到 typora）
* 通过图片字节流识别图片格式
* 通过 md5 避免重复上传图片
* 简单，只需3步配置
* 小巧，只有 21k
* 开源，提供了UML类图，方便熟悉Python的小伙伴了解源码

# 为什么

通过 *Typora QCloud Cos* 将图片上传到腾讯云对象存储并得到图片外链，可以将 Markdown 文本和图片存储解耦，使得可以在任何地方浏览 Markdown 并且正常显示图片。



> [Typora](https://www.typora.io/)
>
> [腾讯云对象存储定价](https://cloud.tencent.com/product/cos/pricing)

# 如何使用

视频教程：
* [bilibili](https://www.bilibili.com/video/BV14k4y117Wd/)
* [youtube](https://www.youtube.com/watch?v=iODN7TYASiY&ab_channel=ziboliao)

---


第一步，安装 Python 和 插件：

[下载和安装 Python](https://www.python.org/downloads/)，安装时记得勾选 `Add Python to PATH` 选项

```bash
# 安装 typora_cos
pip install -U typora_cos
```
---
第二步，执行以下命令生成配置文件，并用记事本打开：

```bash
python -m typora_cos.qcloud --init

# 已生成配置文件，请继续完成配置： C:\Users\liaoz\.typora_cos\config.xml
```

> 从 https://console.cloud.tencent.com/cam/capi 获取 `SecretId` 和 `SecretKey`
>
> 从 https://console.cloud.tencent.com/cos5/bucket 创建存储桶并得到存储桶名称 `bucket` 和 所属区域 `region`

创建存储桶时需要勾选 `公有读私有写` 选项


<img src="https://test-1253318070.cos.ap-nanjing.myqcloud.com/303049df7690b9b7a1dc19721265be6e.png" alt="image-20200922200543157" style="zoom: 45%;" />


<img src="https://test-1253318070.cos.ap-nanjing.myqcloud.com/1b4d0796f82976f6d3983f71b320a95d.png" alt="image-20200922200904022" style="zoom: 45%;" />

---


第三步 （可选），在命令行进行测试


```bash
python -m typora_cos.qcloud https://www.python.org/static/img/python-logo.png

# 已成功上传：
# https://test-1253318070.cos.ap-nanjing.myqcloud.com/7cbb8b7f3ec73ce6716fedaa4d63f6ce.png
```

```bash
python -m typora_cos.qcloud C:\Users\liaoz\Pictures\mc.jpg           

# 已成功上传：
# https://test-1253318070.cos.ap-nanjing.myqcloud.com/739d7e571980d16ad192ff72e4f2d5d9.jpeg
```

---

第四步（最后一步），typora 设置：

```
文件 -> 偏好设置 -> 自定义命令 -> python -m typora_cos.qcloud
```

<img src="https://markdown-images-1253318070.cos.ap-nanjing.myqcloud.com/39b3d6a83962c0cbf51418af08ad623a.png" alt="image-20200922185842737" style="zoom:50%;" />

---


已知问题：
* 网络图片的URL不能包含请求参数（不能包含 `&` 字符）

# 设计

如果大家对源码感兴趣，可以访问项目地址：

* https://gitee.com/liaozb1996/typroa_qcloud_cos
* https://github.com/liaozb1996/typora-qcloud-cos



流程：

1. 输入一个或多个路径
2. 将路径转换成字节流
3. 将字节流抽象成对象存储文件类
4. 执行上传操作，返回 URL

设计模式：策略模式+简单工厂方法、外观模式、简单工厂方法

其他：配置和代码解耦

![image-20200923154820861](https://markdown-images-1253318070.cos.ap-nanjing.myqcloud.com/45f7958e4b28fc9445d0c712a6a35df4.png)