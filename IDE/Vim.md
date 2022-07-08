# VIM

## Plugins

### ctags

```bash
ctags -R *  // 生成ctags检索文件
```

### cscope

1. 安装：
   Plug 'vim-scripts/cscope.vim'

2. 配置快捷键

   ```bash
   ~/.vimrc 中添加如下配置：
   " scscope
   set cscopequickfix=s-,c-,d-,i-,t-,e-
   nnoremap <leader>sa :call cscope#findInteractive(expand('<cword>'))<CR>
   nnoremap <leader>sl :call ToggleLocationList()<CR>
   " s: Find this C symbol
   nnoremap  <leader>ss :call cscope#find('s', expand('<cword>'))<CR>
   " g: Find this definition
   nnoremap  <leader>sg :call cscope#find('g', expand('<cword>'))<CR>
   " d: Find functions called by this function
   nnoremap  <leader>sd :call cscope#find('d', expand('<cword>'))<CR>
   " c: Find functions calling this function
   nnoremap  <leader>sc :call cscope#find('c', expand('<cword>'))<CR>
   " t: Find this text string
   nnoremap  <leader>st :call cscope#find('t', expand('<cword>'))<CR>
   " e: Find this egrep pattern
   nnoremap  <leader>se :call cscope#find('e', expand('<cword>'))<CR>
   " f: Find this file
   nnoremap  <leader>sf :call cscope#find('f', expand('<cword>'))<CR>
   " i: Find files #including this file
   nnoremap  <leader>si :call cscope#find('i', expand('<cword>'))<CR>
   
   if has("cscope")
       set csprg=/usr/bin/cscope
       set csto=1
       set cst
       set nocsverb
       if filereadable("cscope.out")
           cs add cscope.out
       elseif $CSCOPE_DB != ""
           cs add $CSCOPE_DB
       endif
       set csverb
   endif
   ```

   

3. 生成cscope.files文件，使用绝对路径

    ```bash
    find 'absolute path' -name "*.h" -o -name "*.cpp" -o -name "*.hpp" -o -name "*.c" > cscope.files
    ```

   

4. 生成cscope.out

   ```bash
   cscope -Rbq -i cscope.files 
   or
   cscope -Rbq // 默认在当前路径查找cscope.files
   ```

5.  源码目录打开vim

   ```bash
   :cs add cscope.out
   ```

6. 常用指令

   ```bash
   :cs find s: 查找C语言符号，即查找函数名、宏、枚举值等出现的地方
   :cs find g: 查找函数、宏、枚举等定义的位置，类似ctags所提供的功能
   :cs find d: 查找本函数调用的函数
   :cs find c: 查找调用本函数的函数
   :cs find t: 查找指定的字符串
   :cs find e: 查找egrep模式，相当于egrep功能，但查找速度快多了
   :cs find f: 查找并打开文件，类似vim的find功能
   :cs find i: 查找包含本文件的文
   ```

   