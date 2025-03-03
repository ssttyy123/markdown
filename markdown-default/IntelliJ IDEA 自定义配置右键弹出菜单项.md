---
title: IntelliJ IDEA 自定义配置右键弹出菜单项
date: 2025-03-03 16:12:11
tags: 
  - 问题解决
  - IntelliJ IDEA
---
## 问题描述
在安装了一些如 jclasslib 的文件查看的特殊插件，但是默认情况下只能通过菜单栏的特定 tab 打开查看，十分不便，需要将该文件打开放入右键文件之后的弹出菜单中。

## 解决方案
_预先安装如 jclasslib 的需要添加菜单的插件。_
1. 进入 **Settings -> Appearance & Behavior -> Menus and Toolbars** 。
2. 点开 **Project View Popup Menu** 菜单列表。
3. 在第三个菜单项区(放置 View 菜单项)中点击 **Add...** 。
   ![Snipaste_2025-03-03_15-57-47.jpg](https://s2.loli.net/2025/03/03/bkys89gRzP5SKt6.jpg)
4. 搜索 jclasslib ，选择 **Show Bytecode With Jclasslib** 并且点击 OK 确认，可以适当拖动该菜单项到合适位置，然后保存退出 Settings。
   ![Snipaste_2025-03-03_15-58-57.jpg](https://s2.loli.net/2025/03/03/kGvUKObd8iaAmhN.jpg)
5. 在相应的 class 文件上进行右键，出现 **Show Bytecode With Jclasslib** 菜单项。
   ![Snipaste_2025-03-03_15-59-34.jpg](https://s2.loli.net/2025/03/03/Gb8ZCuBhnMLrgHP.jpg)

## 扩展
1. **Editor Popup Menu** - 编辑器弹出菜单，提供与编辑器相关的功能，如代码编辑和格式化，在代码编辑器中右键弹出的菜单。
2. **Project View Popup Menu** - 项目视图弹出菜单，允许用户对项目文件和结构进行操作，在文件目录中右键文件弹出的菜单。
3. **Editor Tab Popup Menu** - 是编辑器选项卡的弹出菜单，通常提供与当前打开的文件或选项卡相关的操作，在文件打开后的 tab 部分右键弹出的菜单。
   
相关文档
[菜单和工具栏 |IntelliJ IDEA 文档](https://www.jetbrains.com/help/idea/customize-actions-menus-and-toolbars.html)