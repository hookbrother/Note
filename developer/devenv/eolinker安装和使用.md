= 安装准备 =
根据[整体开发环境搭建](http://121.40.214.114/w/devenv/globaldev)搭建好基础开发环境
= 安装 =
==== 下载源码 ====
前往[官网](https://www.eolinker.com/#/os/download)下载最新的压缩包，将下载到的压缩包上传到服务器，解压到/usr/www目录
```
cp eolinker_os_2_0.zip /usr/www/
unzip eolinker_os_2_0.zip
mv ./eolinker_os ./eolinker
rm -f ./eolinker_os_2_0.zip
chown nginx:nginx -R ./eolinker
chmod a+w -R ./eolinker
```
==== 新建数据库 ====
```
mysql -uroot
create database eolinker;
exit;
```
==== 安装 ====
访问[域名](http://121.40.214.114:8081/eolinker)(根据自己域名)，根据提示填写安装。之后注册好用户名和密码

= 使用 =
==== 管理测试环境 ====
随便点个项目进去右上，这个可以设置url的前缀，不用每个接口都写完整的域名，只需要写后面一部分即可，而且可以建多个环境，如本地环境，测试环境，正式环境，方便切换
==== 对象参数 ====
如果参数的类型不是基本类型，请求参数可以使用>>表示二级参数，返回参数可用::表示二级参数，具体可见示例项目
==== 协作管理 ====
可以将其他用户加入该项目，注意其他用户也会拥有写权限
