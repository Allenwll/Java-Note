# vim使用

## Vim

### 模式

- normal 普通模式 用于移动
- insert 插入模式 用于编辑 esc退出
- visual 可视模式 用于选择 esc退出


### 移动
	
	h 向左移动一个字符
	j 向下移动一行
	k 向上移动一行
	l 向右移动一个字符
	0 行首
	$ 行尾
	gg 第一行行首
	G 最后一行行首
	:n 到第n行

	w 移动到下一个单词的开始(由字母数字下划线组成)
	b 移动到上一个单词的开始(由字母数字下划线组成)
	W 移动到下一个字符串的开始(非空字符串)
	B 移动到上一个字符串的开始(非空字符串)
	e 移动到下一个单词的结尾
	ge 移动到上一个单词的结尾
	E 移动到下一个字符串的结尾
	gE 移动到下一个字符串的结尾
	
	fX 移动到下一个字符x
		; 下一个fx 用于重复
		, 上一个fx 用于重复
	Fx 移动到上一个字符x
		; 上一个fx 用于重复
		, 下一个fx 用于重复

	<C-f> 下一页
	<C-b> 上一页


### 编辑
	
	u 撤销
	<C-r> 重做
	i 当前位置插入
	I 行首插入
	a 后一个位置插入
	A 行尾插入
	cw 删除当前单词并插入
	cW 删除当前字符串并插入
	ce 删除直到字符串结束并插入
	dd 删除当前行
	dw 删除一个单词
	yy 复制当前行
	p 粘贴
	:wq! 保存退出
	:w! 保存
	:q! 不保存退出
	:saveas 另存为

    .			重复上次命令
    nX			重复X命令n次
    %:normal A;	在所有的行尾都加上；即{range}nornal 动作

    组合
		ye			从光标位置拷贝到当前单词结尾
		0y$			先移动到行头，在从行头拷贝到行尾
		gU			可视化选择后,变大写
		gu			可视化选择后,变小写
		d			可视化选择后,删除
		ddp			将当前行上移一行 即 先剪切一行，然后复制到下一行
		ddkp			将当前行下移一行
		xp			左右交换两个字符
		<start position>command<end position> 在开始于结束为止之间做command命令


	缩进 
		:n,m>     表示在n,到m之间进行缩进
		在选择模式中选择行, >向右缩进 < 向左缩进
		ggVG> 全部缩进
		gg=G 全部退缩


	 列编辑
		
		C-V 进入列选择模式
		rx	进入列选择模式，选择列后，替换当前位置为x
		I(Shift+i)xxx, Esc按两次， 进入列选择模式，选择列后，插入xxx





### 查找

	:/xx 在当前文件内查找xx并移动到第一个匹配位置
		n 下一个xx
		N 上一个xx

### 替换		

	:{range}s/pattern/string/[c,e,g,i] 在范围内把pattern替换成string
		g 	全局替换
		c	确认
		e	不显示error
		i	不区分大小写
		%表示所有行
		n,m	从第n行到第m行
		n,$ 从第n行到结尾
		可以使用#替换/ 则pattern与string如果含有/,/不被识别为分隔符

		n,ms/$/XXX/g 在行尾插入xxx
		n,ms/^/xxx/g 在行首插入xxx

### 搜索

	:find xxx.yy 递归查找xxx.yy文件,可使用tab补全(需要添加path路径:set path+=~/notes/**)

	:vim[grep] /pattern/[gj] 搜索的文件/范围
	g表示是否把每一行的多个匹配结果都加入
	j表示是否搜索完后跳到第一个匹配位置

	:cn[ext] <F5> 跳到下一个位置
	:cp[revious] <Shift-F5> 跳到前一个位置
	:cnf[ile] 跳到下一个文件
	:cpf[ile] 跳到前一个文件
	:cr[ewind] 跳到第一个quickfix
	:cla[st] 跳到最后一个quickfix

	vimgrep /mysql/ *.md **

	vimgrep /pattern/ %           在当前打开文件中查找
	vimgrep /pattern/ *             在当前目录下查找所有
	vimgrep /pattern/ **            在当前目录及子目录下查找所有
	vimgrep /pattern/ *.c          查找当前目录下所有.c文件
	vimgrep /pattern/ **/*         只查找子目录

	vimgrep /flash/gj  **/*.as 搜索当前目录以及所有子目录内as文件中的'flash'
	vimgrep /an error/gj   *.c   在所有.c文件中搜索an error
	vimgrep /an error/gj    *    在当前目录下的文件搜索an error，不包括子目录



### 文件

	:! mkdir -p xxx/yyy/zzz 递归创建xxx/yyy/zzz路径
	:! touch xxx/yyy/zzz/abc.md 在xxx/yyy/zzz下创建abc.md文件
	:! ls 列出文件与文件夹(系统命令)
	: ls 列出文件与文件夹


### 多窗口

    :vs xxx.yy 垂直多窗口打开xxx.yy
	:sv	xxx.yy 水平多窗口打开xxx.yy 使用tab补全文件名 <C-w> Ctrl与hjkl组合移动


### Ex 指令

#### 范围表示

- % 所有行
- n,m 从第n行到第m行
- . 当前行
- $ 结尾行

#### 指令

- :t :copy 复制行

	语法:

		{range}:t{target}
		
	示例:

		1:t$  将第一行复制到最后一行后
		1,5:t15 将第一行到第5行之间的内容复制到第15行之后

- :m :move 移动行

	语法:

		{range}:m{target}
		
	示例:

		1:m$  将第一行移动到最后一行后
		1,5:m15 将第一行到第5行之间的内容移动到第15行之后

- :normal 重复执行

		{range}:nomal {动作}



## 插件
### NERDTree


	1. 可以使用Ctrl+W 在NERDTree 与Vim编辑内容里面切换
	2. 使用! XXX 可以在NERDTree里面执行XXX系统命令，例如
		! mkdir XXX1/XXX2 #在NERDTree当前文件夹下递归创建目录XXX1/XXX2
		! touch XXX1/XXX2/XXX3.md 在XXX1/XXX2下创建XXX3.md文
	3. 执行完命令后在对应的目录下按r 就可以刷新目录


### ctrlp.vim:
      搜索文件名模糊搜索
      安装 
        Plugin 'ctrlpvim/ctrlp.vim'
      使用 
        在命令模式下ctrl+P

### ctrlsf.vim:
      安装
        brew install ack
        Plugin 'dyng/ctrlsf.vim'

### tagbar
      安装
        brew install ctags
        Plugin 'majutsushi/tagbar
        map <F3> :TagbarToggle<CR>
      使用 
        在命令模式下 :TagbarToggle
        (ssss(bbb))



参考链接：
https://www.oschina.net/translate/learn-vim-progressively
