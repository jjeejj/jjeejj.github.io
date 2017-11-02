---
title: linux命令之wget
tags: linux
date: 2017-11-02 22:37:04
---


---------------------------------------------------
`wget`:命令用来从指定的URL下载文件。wget非常稳定，它在带宽很窄的情况下和不稳定网络中有很强的适应性

* 如果是由于网络的原因下载失败，wget会不断的尝试，直到整个文件下载完毕
* 如果是服务器中断了下载过程，它会再次连到服务器上从停止的地方继续下载，这对从那些限定了链接时间的服务器上下载大文件非常有用的
--------------------------------------------------
<!--more-->

#### 语法

`wget [选项] 参数`

#### 选项

* -a,--appent-file Flie 将下载资源的输出信息，追加到到指定的文件中
* -o,--output-file File 将下载资源的输出信息，输出到指定的文件中
* -b,--background 以后台的方式，运行wget
* -q,--quiet 安静的模式，不显示指令执行的过程
* -v,--verbose 显示详细的输出信息（这是缺省设置）
*  -c,--continue 继续执行上次终端没有完成的任务。断点下载
* -L,--releative 下载关联的连接
* -r,--recursive 递归下载
* -p,--page-requisites  下载显示html文件所需的全部文件
* -O,--output-document file 指定下载的文件名
* -P,--directory-prefix PREFIX 将文件保存到目录PREFIX/下
* --limit-rate=300k 限定下载速率
* -U,--user-agent=AGENT 设定代理的名称
* --spider 不下载任务东西，可以进行链接有效性测试
* -t,--tries=numbers 下载重试次数，默认是20,0代表无穷次
* -T,timeout=SECONDS 设置超时时间
* -np,--no-parent 不爬上层目录，不追溯父级链接
* -l,--level=number 下载的层级数
* -i,--input-file file 从文件提取多个url并下载
* -R,--reject=list 过滤不需要下载的文件类型,以英文的逗号分开
* -A,--accept=list 需要下载的文件类型
* -Q,--quota=5m 现在下载总文件大小，对单个文件下载不起作用
* -k,--conver-links 将页面中的链接转换为指向本地的链接
* -m,--mirror 开启适用于镜像的选项
* --follow-ftp 只下载ftp连接
* --ftp-user=USERNAME ftp用户名
* --ftp-password=PASSWORD ftp密码
* --no-check-certificate 不验证服务的ssl
* -nH, --no-host-directories 不创建主机目录,默认时创建的

>wget默认会根据网站的`robots.txt`进行操作使用`-e robots=off`参数即可绕过该限制

#### 参数

 `URL`: 下载指定的url地址


#### 实例

1. 下载单个文件

    `wget http://www.test.com/testfile.zip`

2. 下载并以不同的文件名来保存

    `wget -O testfile.zip http://www.test.com/testfile.zip?sss=a`
>wget 默认会以最后一个符号 / 后面的字符来命名，上面这个链接默认文件名为testfile.zip?sss=a

3. 限速下载

    `wget --limit-rate=300k http://www.test.com/testfile.zip`

4. 伪装代理名称下载

    `wget --user-agent="Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.204 Safari/534.16" http://www.test.com/testfile.zip`

5. 过滤不需要下载的文件类型

    `wget --reject=gif,png http://www.test.com/testfile.zip`

6. 下载指定格式的文件

    `wget -r -A=jpg http://www.test.com/testfile.zip`
>该功能可以下载一个网站的所有视频，或者文件

7. 进行ftp下载

    `wget --ftp-username=USERNAME --ftp-password=PASSWORD ftp-url`

8. 下载整个网站本地查看

    `wget -m -k -p -P /<LOCAL_DIR> http://www.test.com/`

9. 获取一个网站的所有图片---**该命令目前有问题，待查资料**

    `wget -r -p `




>**待补充**