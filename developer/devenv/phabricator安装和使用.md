= 安装 =
==== 安装准备 ====
根据[整体开发环境搭建](http://121.40.214.114/w/devenv/globaldev)搭建好基础开发环境，确保mysql的root用户密码为空
==== 下载，更新 ====
```
nginx -s stop
cd /usr/www
git clone git://github.com/facebook/libphutil.git
git clone git://github.com/facebook/arcanist.git
git clone git://github.com/facebook/phabricator.git
./phabricator/bin/storage upgrade
```
= 配置 =
==== phabricator基本配置 ====
* 创建Administrator用户
* 设置BaseUrl
```
./bin/config set phabricator.base-uri 'http://121.40.214.114/'
```
* 增加验证支持（就是让用户以什么方式注册登录，通常用用户／密码方式就好）
* 增加仓库
```
mkdir /var/repo
chmod a+w -R /var/repo
```
* Phabricator Daemons(开启守护线程)，这样才可以发邮件等
```
./bin/phd start
```
* 支持语法高亮
pygments.enabled=true
==== 使phabricator支持发送邮件 ====
* 安装sendmail
根据[sendmail安装和使用](http://121.40.214.114/w/devenv/sendmail)安装配置好相应软件
* 修改
1. Config->Core->Mail->metamta.default-address = hookbrother@gmail.com（设置为邮件发送者，如果不设置，则为root@hostname，一般情况会被当作垃圾邮件处理）
2. Config->Core->Mail->metamta.mail-adapter = PhabricatorMailImplementationPHPMailerLiteAdapter
==== diffusion配置(仅介绍git，因为不用svn) ====
* 建立git-http-backend软链接，默认是在/usr/www/phabricator/support/bin/下的
```
ln -s /usr/libexec/git-core/git-http-backend /usr/www/phabricator/support/bin/
```
* 三种用户
1. daemon-user，作为守护进程运行，一般我们使用root用户
2. www-user，作为web主机运行，一般我们使用nginx用户，用来使用http协议访问代码库
3. vcs-user，通过ssh连接运行，一般我们使用git用户，用来使用ssh协议访问代码库
* 新建git用户
```
useradd -r -s /bin/sh -c 'git version control' -d /home/git git
```
* 修改/etc/shadow文件
找到git那行，将两个!改成NP
* 修改/etc/sudoers
注释掉"Defaults    requiretty"，并增加下面两个用户
```
nginx ALL=(root)SETENV:NOPASSWD:/usr/libexec/git-core/git-http-backend,/usr/bin/git-upload-pack,/usr/bin/git-receive-pack
git ALL=(root)SETENV:NOPASSWD:/usr/libexec/git-core/git-http-backend,/usr/bin/git-upload-pack,/usr/bin/git-receive-pack
```
* 配置http
* Config->Application->Diffusion->diffusion.allow-http-auth=true
* Settings->VCS Password(要求和登录密码设的不同)
* 配置ssh
1. 设置守护进程用户(如果使用http则不要设置)
```
./bin/config set phd.user root
```
==== 国际化 ====
```
cd /usr/www/phabricator/src/extensions
git clone https://github.com/wanthings/phabricator-zh_CN.git
```
Configure Settings -> Account -> Account Settings -> Translation via 选择简体中文
= arc和differential使用 =
differential使用就是在网站上根据arc提交的一些审核申请作出处理，主要介绍arc的使用
==== mac ====

==== windows ====
