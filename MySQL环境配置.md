## 1.运行mysql服务器
sudo systemctl start mysql;
(**如果无法启动说明，可能已经有另一个sql在运行了，需要查看进程并强制关闭**)
（**ps aux|grep mysql**）
(sudo kill 端口号)

systemctl status mysql;     //查看服务器是否正常运行

systemctl stop mysql;   //关闭服务器

2.启动服务器后才可从用户端登录
mysql -u root -p 

输入密码进入后选择数据库A
use A;


注意：如果想全程用go语言，可以使用gorm。