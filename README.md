#H3C S5130 SSH公匙登录配置
传统登陆交换机方式为SSH账号密码登陆,口令泄露或口令脚本引用会泄露账号密码从而威胁整个网络安全，故采用密匙对登陆的方式，降低口令泄露风险，从而减少口令泄露风险。
前言
传统登陆交换机方式为SSH账号密码登陆,口令泄露或口令脚本引用会泄露账号密码从而威胁整个网络安全，故采用密匙对登陆的方式，降低口令泄露风险，从而减少口令泄露风险。

一、H3C S5130 SSH公匙登录配置流程
1、客户端生成SSH密匙对
2、H3C S5130导入公匙
3、H3C S5130建立公匙登录用户
4、客户端SSH私匙登录测试

二、客户端生成SSH密匙对
1、输入ssh-keygen一路回车

root@~:~# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:qYfwyFJRPIptS5jt0T+ORz4TcGJSQUONWEa3/CommHs root@kean
The key's randomart image is:
+---[RSA 2048]----+
|     BX+.        |
|    .o=+..       |
|   *.+ .o        |
|  + O.= .o       |
|   +o= =S .      |
|   =o+ o=.       |
|  + + *=oo       |
|   oEo.o*        |
|  ..   . o       |
+----[SHA256]-----+


三、H3C S5130导入公匙
1、开启S5130 FTP远程登录

<H3C>system-view
2、开启FTP

[H3C]ftp server enable
3、建立FTP远程账户 xxx

[H3C]logging
[H3C]local-user xxx class manage

4、设置FTP密码 xxx

[H3C-luser-manage-kean]password simple xxx
5、设置该用户服务类型

[H3C-luser-manage-kean]service-type ftp
6、设置该用户访问路径

[H3C-luser-manage-kean]authorization-attribute work-directory flash:/
7、设置该用户权限

[H3C-luser-manage-kean]authorization-attribute user-role network-admin
8、退出

[H3C-luser-manage-kean]exit
9、FTP登录、上传公匙

10、导入生成公匙（xxx为公匙名 xxx.pub为上传公匙）

[H3C]public-key peer xxx import sshkey xxx.pub
四、创建H3C S5130公匙登录用户
1、创建SSH登录用户xxx

[H3C]local-user xxx
2、创建密码 xxx

[H3C-luser-manage-tjs240]password simple xxx
3、创建ssh类型

[H3C-luser-manage-tjs240]service-type ssh
4、创建用户等级

[H3C-luser-manage-tjs240]authorization-attribute user-role level-10
5、退出

[H3C-luser-manage-tjs240]exit
6、给创建xxx用户给定公匙xxx

[H3C]ssh user xxx service-type stelnet authentication-type publickey assign publickey xxx
7、关闭FTP服务

[H3C]undo ftp server enable
8、退出

[H3C]exit
五、客户端SSH私匙免密登录测试
1、登录 xxx为创建的SSH帐号

root@~:~# ssh tjs240@x.x.x.x


六、备选资料
1、删除公匙 xxx创建公匙名

[H3C]undo public-key peer xxx
2、删除SSH登录用户

[H3C]undo ssh user tjs240
3、查看SSH登录用户列表

[H3C]display ssh user-information
4、查看公匙列表

[H3C]display public-key peer brief
5、H3C官方操作文档

http://www.h3c.com/cn/d_201412/847585_30005_0.htm
6、有任何疑问可以加我QQ

839359374 或者 邮箱联系我	solitary_sen@163.com
--------------------- 
作者：solitary_sen 
来源：CSDN 
原文：https://blog.csdn.net/solitary_sen/article/details/94746544 
版权声明：本文为博主原创文章，转载请附上博文链接！
