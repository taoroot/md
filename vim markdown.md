--- 
title: MarkDown在vim下编辑预览
tags:  ["vim","markdown"]
notebook: Knowledge 
---

## MarkDown 如何在vim下编辑预览
### markdown-preview.vim
    https://github.com/iamcco/markdown-preview.vim.git

这个插件具有实时预览功能
当打开了一个md文件时,输入命令

打开预览

    :MarkDownPreview

停止预览

    :MarkDownPreviewStop

vim安装配置 (vundle安装)

    Plugin 'suan/vim-instant-markdown'

默认 vimrc设置

    let g:mkdp_path_to_chrome = "google-chrome"
    " path to the chrome or the command to open chrome(or other modern browsers)

    let g:mkdp_auto_start = 0
    " set to 1, the vim will open the preview window once enter the markdown
    " buffer

    let g:mkdp_auto_open = 0
    " set to 1, the vim will auto open preview window when you edit the
    " markdown file

    let g:mkdp_auto_close = 1
    " set to 1, the vim will auto close current preview window when change
    " from markdown buffer to another buffer

    let g:mkdp_refresh_slow = 0
    " set to 1, the vim will just refresh markdown when save the buffer or
    " leave from insert mode, default 0 is auto refresh markdown as you edit or
    " move the cursor

    let g:mkdp_command_for_global = 0
    " set to 1, the MarkdownPreview command can be use for all files,
    " by default it just can be use in markdown file

MacOS vimrc设置    
    "根据自己选择览器路径
    " let g:mkdp_path_to_chrome = "open -a Google\\ Chrome"
    let g:mkdp_path_to_chrome = "open -a safari"

    " path to the chrome or the command to open chrome(or other modern browsers)

    let g:mkdp_auto_start = 0
    " set to 1, the vim will open the preview window once enter the markdown
    " buffer

    let g:mkdp_auto_open = 0
    " set to 1, the vim will auto open preview window when you edit the
    " markdown file

    let g:mkdp_auto_close = 1
    " set to 1, the vim will auto close current preview window when change
    " from markdown buffer to another buffer

    let g:mkdp_refresh_slow = 0
    " set to 1, the vim will just refresh markdown when save the buffer or
    " leave from insert mode, default 0 is auto refresh markdown as you edit or
    " move the cursor

    let g:mkdp_command_for_global = 0
    " set to 1, the MarkdownPreview command can be use for all files,
    " by default it just can be use in markdown file
    " macvim 配置 "