--- 
title: 在Sublime中配置Evernote
tags:  ["Sublime","Evernote"]
notebook: Knowledge
---

##  在Sublime中配置Evernote

将markdown文件写好以后导入到印象笔记中.

步骤如下:

1. 获取Evernote的API
2. 在Sublime text中安装插件

### 需要的插件

1. Package control (安装包使用)
2. Sublime Tmpl (md模板)
3. Sublime-evernote (Evernote 上传插件)
4. Markdown Preview (在浏览器中预览)
5. MarkdownLivePreview (在Sublime中预览)
6. Markdown Extended (语法高亮)

###插件设置

#### 1. Package control 
        Sublime text 3 官网查看
#### 2. Sublime Tmpl
    1. 根据Evernote的API,需要在上传的文本的顶部写明title,tags,notebook.
       为了方便,我们使用这个插件作为新建md的模板.
	2. 通过 Preferences -- Bowse Packages... 找到SublimeTmpl\tmplates文件夹,并新建一个md.tmpl的文档
    3. Sublime 打开 md.tmpl 文档 写入以下格代码(共5行)
        --- 
        title: 在Sublime中配置Evernote
        tags:  ["Sublime","Evernote"]
        notebook: First Notebook
        ---
    4. 配置顶部菜单menu中的MD选项
       安装了插件后,菜单栏File中出现了一个New file(SublimeTmpl).
       在New file(SublimeTmpl) -- menu 添加如下代码：
        {
            "caption": "md",
            "command": "sublime_tmpl",
            "args": {
            "type": "md"
            }
        },
    5. 添加了以后New file(SublimeTmpl) > menu 中就会出现对应的模板选项了.
    6. 给这个添加快捷键(control + option + M)
       在control + Command + p 调出Command palatte 输入 Key Bindings——User 中代码如下(写在一行):
       { "keys": ["ctrl+alt+m"], "command": "sublime_tmpl", "args": {"type":"md"},"context":[{"key":"sublime_tmpl.md"}] },

#### 3. Sublime-evernote 

    1. 登入evernote官网获取到授权码
      国际版https://www.evernote.com/api/DeveloperToken.action
      国内的https://www.yingxiang.com/api/DeveloperToken.action
    2. Preferences -- Packages Setting -- Evernote -- Setting-User添加如下代码:
      {
        "noteStoreUrl": "",
        "token": ""
      }
      在冒号内填入授权码(如何出现错误,将其中的授权码中的https改成http)

#### 4. Markdown Preview 

#### 5. MarkdownLivePreview 

#### 6. Markdown Extended 
      1. 先打开一个md文件,配置顶部菜单View -- Syntax -- Open All with extent as... -- markdown Extended 
         将当前的md默认高亮语法固定下来
      2. 使用格式
      ```cpp 
      代码
      ```
      '`'是esc下面那个键,后面跟语言('`'前面不能有空格否则失效)
