#apache2安装、基本配置及问题汇总

[TOC]
 
 
##**安装**
目前使用的操作系统是debian，可以直接apt-get获取。
`
sudo apt-get install apache2
`
##**基本配置**
成功安装Apache之后，服务器会默认开启该服务，可以查看当前服务的状态。
`
sudo service apache2 [status|start|stop|restart]
`
+ **主配置目录**

在配置文件目录 /etc/apache2/ 可以查看配置文件目录
![Directory](https://github.com/Mahsh/MarkDownPhotos/raw/master/Apache/Directory.png)

主配置文件是apache2.conf，这里定义了apache服务器中可以供用户访问的目录路径（因此当需要自定义用户访问路径时需要在这里修改配置，例如我修改了Apache的重定向路径，定向到/home/mahaishou/www/，需要增加这部分代码）
```
<Directory /home/mahaishou/www/>
	Options Indexes FollowSymLinks
	AllowOverride None
	Require all granted
</Directory>
```
+ **站点配置**
这里主要其作用的是sites-enabled这个目录下的文件。
该目录下的配置文件需要设置端口、站点ip、站点名字、站点文件的路径等内容。示例配置如下所示。
```
<VirtualHost *:8660>  #port
	#ServerName 192.168.154.128
	ServerAdmin 192.168.154.128
	DocumentRoot /home/mahaishou/www/
	#DocumentRoot /var/www/html   #default path
	#<Directory /var/www/html>
	<Directory /home/mahaishou/www/>
		Options Indexes FollowSymLinks MultiViews
		AllowOverride All
		Order allow,deny
  		Allow from all
	</Directory>
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
+ **站点配置**

主要起作用的是ports.conf 文件，只需要设置侦听的端口而已。
```
#Listen 80
Listen 8660
```
+ **测试效果**

这里显示访问上边设置的站点的测试文件，效果如下：
![Connection](https://github.com/Mahsh/MarkDownPhotos/raw/master/Apache/ConnectionResult.png)
可以看到访问192.168.154.128的8660端口，能够访问到apache服务器指定路径/home/mahaishou/www/下的文件（这里是testdir目录）。
##**问题汇总**
这里写下在配置过程中出现的一些问题
+ **代码写错**
这个非常坑，因为是在vim中编辑代码的，所以没有报错，而是在查看apache服务的状态时才报错的。所以在写代码时注意vim中一些关键词是否高亮，多留意一下，少踩一些坑。
+ **端口配错**
在自定义端口时，如我配置的8660端口，需要在ports.conf中增加监听的端口号。因为默认是使用80端口。
+ **主配置文件未修改**
这个问题出现在你自定义重定向的路径时，需要在主配置文件apache.conf中增加路径。默认的路径是 /usr/share 和/var/www/。
