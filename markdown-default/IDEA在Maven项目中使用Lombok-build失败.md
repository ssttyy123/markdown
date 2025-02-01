---
title: IDEA在Maven项目中使用Lombok build失败
date: 2025-01-30 16:07:38
tags: 
  - 问题解决
  - Lombok
  - Maven
  - Java
---
# IDEA在Maven项目中使用Lombok build失败
## 问题描述
在 IDEA 中使用注解 `@Log4j2`中的 `log` 变量进行日志输出。
但是在进行 build 的时候出现如下问题：
```text
java：找不到符号 
    符号：变量：log
```
IDEA版本：2024.3
Lombok Maven版本：1.18.36

## 问题原因
在网络上查询该问题，大多解决方案为没有安装 Lombok 插件，但是根据检查，IDEA 已经安装了该插件。
并且在 Setting -> Build, Execution, Deployment -> Compiler -> Annotation Processors 中的选择项 Enable annotation processing 已经勾选。
在 [intellij-support](https://intellij-support.jetbrains.com/hc/en-us/community/posts/23064675521682-Lombok-not-workin-with-Intellij) 社区支持中查找到一个帖子，描述了一个解决方法，并且进行配置成功 build。

---

因为 build 报的是无法找到符号，并且 log 部分是自动生成的，可以猜测 jar 类加载错误，导致生成失败。<br>
根据配置中的 Processor path 进入该路径。

![lombok jar unknown.jpg](https://s2.loli.net/2025/01/30/oi3cvHAsLe4xlZq.jpg)

可以发现此文件夹下没有相应的 lombok 的 jar 包。至此可以认为 idea 此处自动生成的路径存在一些问题，只需要改为从 project classpath 中获取到 jar 包即可。<br>
问题原因大概率是新版本的 IDEA 在对 lombok 版本检查时没有检测到相关版本，使用了默认的 unknown 导致的没有相关 jar 包的原因。

![annotationProcessors.jpg](https://s2.loli.net/2025/01/30/3oSFTyMYOcEt15m.jpg)

## 解决方案
在 Annotation Processors 中将设置的 Processor Path 选项改为 Obtain processors from project classpath，并且应用该配置进行 build。
