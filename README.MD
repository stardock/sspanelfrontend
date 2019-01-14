https://blog.csdn.net/zhaokaiqiang1992/article/details/41349819  

## 准备阶段

* 挑选一台合适的vps，不要用废品vps来试教程。  
* 对vps进行优化，安装锐速或者是bbr这类的vps加速工具。  
* 系统要求：Centos7  

## 安装宝塔管理面板  
* 使用putty工具登陆vps  
* 安装宝塔一建端  
`yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh`
  ![](/data/bt1.jpg)  
  * 等待安装完成（官网说只要两分钟就可以完成，这个还是要根据服务器性能来确定，我的装了大概4分钟）  
  * 安装完成后，会提示后台登录地址和管理员账号、密码等信息，最好保存下来，然后打开浏览器进行登录。  
  ![](/data/bt2.jpg)  
  * 登陆完成后第一次进入面板，会提示你要安装哪些软件
  ![](/data/bt3.jpg)  
  * 这里我们选择LNMP, Mysql选择5.5 (低配小鸡), PHP选择7.1 **(必须是这个版本)**, 选择快速安装  
  * 此时，你可以修改一下后台登录密码：面板设置–面板密码，点击修改。  
然后，坐下来休息一下，等待安装完成吧(*^__^*)  
  ![](data/bt4.jpg)

## 新建网站
* 点击“网站”–“添加站点”  
![](/data/bt4.jpg)  
* 按照提示创建网站
![](/data/bt5.jpg)  
* 配置nginx  
网站创建成功后，访问域名可以看到创建成功的提示（前提是你绑定的域名也已经解析到当前vps的ip上了，如果没有，请进行解析），接下来，我们进行一些配置。  
   * 点击“网站”–“修改”，进入nginx配置界面  
    ![](/data/bt6.jpg)
   * 找到配置文件
    ![](/data/bt7.jpg)
        * 将  
         root /www/wwwroot/ss.naoxiaogen.com（此处为你的域名）改为  
         root /www/wwwroot/ss.naoxiaogen.com/public  
         在配置文件结尾添加：   
         ```
         location / {  
         try_files $uri $uri/ /index.php$is_args$args;  
         }  
         ```
         修改后的配置文件：  
         ![](/data/bt8.jpg)  
         ![](/data/bt9.jpg)  

## 安装Nimaqu sspanel   





