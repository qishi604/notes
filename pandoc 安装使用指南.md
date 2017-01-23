# pandoc 安装使用指南

[github](https://github.com/jgm/pandoc)
[官方文档](https://pandoc.org/MANUAL.html)

## 安装：

`brew install pandoc`

### 安装pdf 转换器：

下载[mactex](http://www.tug.org/mactex/morepackages.html)

安装路径为

`/usr/local/livetex`

添加到环境变量

`sudo ln -s /usr/local/texlive/2016basic/bin/x86_64-darwin /usr/bin/latex`

## 使用

### 转pdf

注意：如果文档带有中文，需要指定中文字体`-V mainfont=SourceHanSansCN-Light`

`pandoc input.md -o output.pdf --latex-engine=xelatex -V mainfont=SourceHanSansCN-Light`

