## 介绍

### 使用场景

- 服务和远程ftp服务器网络不通(网闸/光闸)
- 访问控制
- 安全增强
- 流量管理
- 缓存服务
- 匿名访问

### 网闸原理示例图

![FTP代理](https://wxy-md.oss-cn-shanghai.aliyuncs.com/FTP%E4%BB%A3%E7%90%86.png)

## 搭建手册

参考:https://www.cnblogs.com/guohongwei/p/10848698.html

安装ftp: yum install ftp -y

安装vsftpd: yum install vsftpd -y

**我的配置文件:**/etc/vsftpd/vsftpd.conf

```conf
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
```

## 常见错误

- 检查网络/防火墙/iptables规则
- ftp vsftpd 530 login incorrect
```
1.密码错误。

2.检查/etc/vsftpd/vsftpd.conf配置
local_enable=YES  
pam_service_name=vsftpd     //这里重要，有人说ubuntu是pam_service_name=ftp，可以试试
userlist_enable=YES 

3.检查/etc/pam.d/vsftpd
vim /etc/pam.d/vsftpd
注释掉
#auth    required pam_shells.so
4.重启

5.或者修改/etc/pam.d/vsftpd文件，把pam_shells.so改成pam_nologin.so //默认ftp用户是没有登录shell权限
```

- ftpClient获取文件listFiles为空

  ```
  ftpClient.changeWorkingDirectory(path);  
              ftpClient.enterLocalPassiveMode();  //开启被动模式  
              //由于apache不支持中文语言环境，通过定制类解析中文日期类型  
              ftpClient.configure(new FTPClientConfig("com.zznode.tnms.ra.c11n.nj.resource.ftp.UnixFTPEntryParser"));  
   FTPFile[] files = ftpClient.listFiles();
  ```

## 本程序使用方法

### 工具类

![image-20240425100744729](https://wxy-md.oss-cn-shanghai.aliyuncs.com/image-20240425100744729.png)

### 修改配置文件

![image-20240425095044445](https://wxy-md.oss-cn-shanghai.aliyuncs.com/image-20240425095044445.png)

### 修改运行路径

可以自己指定配置文件路径,也可以写死配置文件路径

![image-20240425095404001](https://wxy-md.oss-cn-shanghai.aliyuncs.com/image-20240425095404001.png)

### 本地访问代理程序地址

![image-20240425095725156](https://wxy-md.oss-cn-shanghai.aliyuncs.com/image-20240425095725156.png)

![image-20240425095657822](https://wxy-md.oss-cn-shanghai.aliyuncs.com/image-20240425095657822.png)