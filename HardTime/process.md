记录这段时间遇到的问题

第一阶段：成功开启Apache服务，并能在页面正常访问Apache服务

问题：

1.启动命令输入后需要输入密码，尝试几次后并不成功

​	解决：查看当前用户及所属用户组(分别是zhuyanfu/staff),修改密码时也需要输入原密码，此处应该和开机密码一致(可做实验—实验已做，确认是，记录一下这个命令：dscl . -passwd /Users/zhuyanfu XXX)，结果成功修改密码(其实前后两次密码相同，都是leizi172677,所以实际上并没有修改，怀疑是成功运行完修改密码的命令后，缓存消失)，修改密码后，再次运行启动Apache服务的命令输入修改后的密码显示成功启动服务！这里附上Mac管理用户及用户组的一个链接:

<https://jingyan.baidu.com/article/14bd256e943fa6bb6d261203.html>

2.在页面上输入localhost，没能成功，显示无法访问此网站

​	解决：

​		1).经过查询日志(/private/var/log/apache2)发现今天所有的访问根本没有走日志

​		2).sudo apachectl 这个命令有一个选项-k(具体作用待查)，加上此选项启动Apache直接报错：AddType requires at least two arguments, a mime type followed by one or more file extensions，需修改httpd.conf文件，在Addtype application/x-httpd-php .php这一段.php之前加一个空格，重启后发现页面报错改为forbidden

​		3).403实际上就是 **拒绝访问** 的意思，怀疑是虚拟主机配置有问题，目前暂时用不到虚拟主机，去到httpd.conf中将Include /private/etc/apache2/extra/httpd-vhosts.conf  这段注释一下(httpd.conf文件的第520行)，重启后发现问题已经解决了！网页中已经能够访问到默认的内容了，现在已经是凌晨两点多了，今天先到这儿，最后附上403问题得以解决的灵感来源：<https://www.cnblogs.com/wajika/p/6481167.html>

第二阶段：重新从GitHub上拉取项目，将笔记移植到GitHub上管理

​	Git在之前已经安装完成了，所以基本上没有碰到什么有硬度的问题

​	这里注意一下，只要切换到GitHub当初执行git init 的目录下就可以正常操作git指令了，不像Windows那样需要右键->git bash here 来打开一个操作窗口，也就是说Mac安装完成Git之后就可以在终端直接操作git了，对于用习惯了Windows的老狗们来说思想需要转换一下

第三阶段：配置虚拟主机，目标是本地通过www.bo.com就能访问项目

​	①.待办：sudo vim XXX 总是询问输入密码，解决之

​	—已经解决，放一个链接->https://blog.csdn.net/zlfing/article/details/90448667，仅仅用第一种方案就解决了

​	②.有问题但是没有报错，而是固执的读取了默认路径下test.php文件里的内容，打算做一个测试目录来验证我的猜想，今天太晚了，明天再做。。。

​	OK,现在直接检测不到请求了，那就可能是步子迈的太大了，往回收一收。。。先搞一个测试目录用来测试虚拟主机是否搭建成功

​	Mac 下复制文件的全路径：command + option + C ; 粘贴就是command + V

​	总结一下截止到目前的进度和问题：在Apache的主配置文件中设置的端口号为:8080，打开vhost相关模块的加载后，网页第一次可以正常访问localhost:8080，刷新就访问不到了(WTF!?)；做了一个测试目录位置在 /Users/zhuyanfu/Applications/MyFiles/index.php，在httpd-vhosts.conf中配置好了，本地解析也做好了，域名是www.test.com，但是不能正常访问，页面没有有价值的报错信息，显示无法访问此网站



​	③.现在就已经感觉切换目录很麻烦了，穷则思变，Mac自带了一个叫自动操作的程序，抽空学习一下

​	测试完了问题1中的开机密码修改之后发现Apache启动后有问题，具体报错如下：

![image-20190708171945878](/Users/zhuyanfu/Library/Application Support/typora-user-images/image-20190708171945878.png)

​	我怀疑是因为中间更改过密码，所以Apache有这么一套保护机制，那就按照他说的做，到httpd.conf中查找ServerName关键字，添加一下ServerName localhost:80这段

第四阶段：成功安装数据库，跑一个小demo来测试，也可以用GitHub上的相关项目(那个laravel5.5)

第五阶段：做草图，搞一个系统而小的项目需求

第六阶段：着手将BO项目作出一个1.0出来，这期间开始找工作