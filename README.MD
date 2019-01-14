http://www.xiaogen.club/archives/207.html


一、准备阶段 
在进行ss-peanlV3魔改版安装时，请你先做好下面的准备工作：  
1、挑选一台合适的vps，不要用废品vps来试教程。如果你还没有购买合适的vps可以查看的：入门级vps选购指南  
2、对vps进行优化，安装锐速或者是bbr这类的vps加速工具。查看：1、简单几部优化服务器，观看YouTuBe无压力(锐速)；2、比肩锐速，新一代vps单边加速BBR

二、安装宝塔管理面板 
建议安装官网最新版（www.bt.cn）这里我安装的是linux3.7版。 
使用 SSH 连接工具（putty)连接到您的 Linux 服务器后，执行以下命令开始安装（大约2分钟完成面板安装）：  
    yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh 
注意：宝塔面板对操作系统的要求是：全新系统CentOS 6.x 32/64位、CentOS 7.x 32/64位（暂不支持 Ubuntu、Debian）， 
          确保是干净的操作系统，没有安装过其它环境带的Apache/Nginx/php/MySQL


1、使用putty工具登陆vps             
2、安装宝塔一建端 
拷贝安装代码 
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
在putty中单击右键粘贴，回车。 
 
等待安装完成（官网说只要两分钟就可以完成，这个还是要根据服务器性能来确定，我的装了大概4分钟） 
安装完成后，会提示后台登录地址和管理员账号、密码等信息，最好保存下来，然后打开浏览器进行登录。 
         
登陆完成后第一次进入面板，会提示你要安装哪些软件。  
            
这里我们选择LNMP 
Mysql选择5.6以上版本 
PHP选择7.0，提高性能 
点击“一键安装”，这样软件会添加到序列进行安装，耐心等待这些软件安装完成。（点击“软件管理”–“所有软件”可以查看当前软件安装进度） 
此时，你可以修改一下后台登录密码：面板设置–面板密码，点击修改。 
然后，坐下来休息一下，等待安装完成吧(*^__^*) 。 
所有套件安装完成： 
           

3．新建网站 
（1）点击“网站”–“添加站点” 
          
 按照提示创建网站： 
         
（2）配置nginx 
网站创建成功后，访问域名可以看到创建成功的提示（前提是你绑定的域名也已经解析到当前vps的ip上了，如果没有，请进行解析），接下来，我们进行一些配置。 
点击“网站”–“修改”，进入nginx配置界面 
          
找到配置文件 
          
将root
/www/wwwroot/ss.naoxiaogen.com（此处为你的域名）改为root
/www/wwwroot/ss.naoxiaogen.com/public 
在配置文件结尾添加： 
location / {
    try_files $uri $uri/ /index.php$is_args$args;
}
修改后的配置文件： 
          
         
点击保存使其生效。 
此时，我们访问我们的域名，提示404错误，这是正常的，因为我们的根目录已经绑定在了public这个文件夹下，但是现在public还不存在。 
那么接下来，我们安装sspanel v3魔改版来获得这个public文件夹。

三、安装sspanel v3魔改版 
这里我们用G大的sspanel v3魔改版来进行安装，魔改版的github地址为https://github.com/esdeathlove/ss-panel-v3-mod 
1.    下载sspanel v3 
进入我们的网站跟目录 
cd  /www/wwwroot/ss.naoxiaogen.com 
  
运行 
yum install git –y 

git clone https://github.com/stardock/ss-panel-v3-mod.git tmp -b new_master && mv tmp/.git . && rm -rf tmp && git reset --hard 
进行安装 
给权限 
chown -R root:root *
chmod -R 777 *
chown -R www:www storage 
修改完了之后，到网站目录下进行一些修改。 
cd  /www/wwwroot/ss.naoxiaogen.com
chattr -i .user.ini
mv .user.ini public
cd public 
然后就是重新添加回权限。 
chattr +i .user.ini 
OK，重启一下 nginx 
service nginx restart 
好，这个暂时到这里，我们待会再回来进行配置。 
因为不打算使用vpn，所以本文不涉及redis的配置。 
2.    配置sspanel v3数据库 
配置之前先导入数据库。 
首先获得我们需要的sql文件。 
进入宝塔控制面板，点开网站目录。 
 
点开sql文件夹，找到glzjin_all.sql文件，并下载它到本地。 
 
 
然后，按照下面的提示打开刚才我们网站的数据库。 
 
然后找到我们创建的数据库，我刚才创建的是sspro，点击“导入”，选择我们刚才下载的sql文件，点击“执行”导入数据库。 
 
  
至此，我们的数据库已经安装好了。

3.    配置sspanel v3 
然后让我们回到 sspanel 的配置上来 
cd  /www/wwwroot/ss.naoxiaogen.com 
php composer.phar install 
 (会出现一些黄色警告，无视掉即可) 
cp config/.config.php.example config/.config.php 
 然后，我们打开宝塔配置面板 ，进入网站目录，找到config文件夹下的config.php文件，点击编辑进行修改。 
  
根据文字提示进行配置，都有中文说明，应该不难，注意填写数据库的时候要填写准确。 
配置完成后点击保存，然后访问你的域名，可以显示主页了。 
4. 为sspanel v3魔改版添加管理员 
在putty中输入   php -n xcat createAdmin    根据提示设置管理员账号、密码 
然后就是来同步一下用户 
 php xcat syncusers 
 然后 crontab -e ，添加以下六段，（*输入i进入编辑，按esc后输入:wq保存） 
30 22 * * * php /www/wwwroot/ss.naoxiaogen.com/xcat sendDiaryMail
*/1 * * * * php /www/wwwroot/ss.naoxiaogen.com/xcat synclogin
*/1 * * * * php /www/wwwroot/ss.naoxiaogen.com/xcat syncvpn
0 0 * * * php -n /www/wwwroot/ss.naoxiaogen.com/xcat dailyjob
*/1 * * * * php /www/wwwroot/ss.naoxiaogen.com/xcat checkjob    
*/1 * * * * php -n /www/wwwroot/ss.naoxiaogen.com/xcat syncnas 
注意将代码中的ss.naoxiaogen.com替换成你的网站域名。 
至此，sspaenl v3魔改版前端已经搭建好了。


常见问题： 
      
1.安装完成后，除主页外其余页面404 
        配置文件未开启伪静态，在宝塔控制面板中，进入网站配置，选择伪静态规则为wordpress。 
  
        相关ss后端搭建教程，请看： 
 重要提示：按照教程搭建是一定可以成功的，推荐几个搭建好的梯子给大家： 
 喵雲T：http://ss.949538.com



