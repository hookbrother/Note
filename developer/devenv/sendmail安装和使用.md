= 安装 =
```
yum install sendmail sendmail-cf
systemctl enable saslauthd
```
= 配置 =
==== 配置sendmail的SMTP认证 ====
编辑/etc/mail/sendmail.mc，去掉下面两行的行首注释符号dnl
```
dnl TRUST_AUTH_MECH(`EXTERNAL DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
dnl define(`confAUTH_MECHANISMS', `EXTERNAL GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
```
==== 设置Sendmail服务的网络访问权限 ====
编辑/etc/mail/sendmail.mc，将下面这行的127.0.0.1改成0.0.0.0以便任何主机可以访问
```
DAEMON_OPTIONS(`Port=smtp,Addr=127.0.0.1, Name=MTA')dnl
```
==== 生成配置文件 ====
```
m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf
```
= 启动 =
```
systemctl enable sendmail
systemctl start sendmail
```
