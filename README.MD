rm

Notes:  
The routes.php has been move to app/ since the update committed on Dec 18, 2019 

The working fine commit number:  
```  
5801a62 HEAD@{0}: pull: Fast-forward
f38daf7 HEAD@{1}: checkout: moving from 78df3dacc56170f4ac33d27623c99767ac85b357 to dev
78df3da HEAD@{2}: checkout: moving from 74a1afac36331ab972d62e8ed74bacdddc1ca0a0 to 78df3da
74a1afa HEAD@{3}: checkout: moving from dev to 74a1afa
f38daf7 HEAD@{4}: pull: Fast-forward
74a1afa HEAD@{5}: pull: Fast-forward
d6861ec
```  


0x32 安装网站程序  

如果想使用 master 分支请把 git clone -b dev 中的 dev 改为 master  

请自行将 你的文件夹名 替换为相应内容。  

在机器 Shell 上执行以下命令：  

cd /www/wwwroot/你的文件夹名  
git clone -b dev https://github.com/Anankke/SSPanel-Uim.git ${PWD}  
git config core.filemode false   
wget https://getcomposer.org/installer -O composer.phar  
php composer.phar  
php composer.phar install  
chmod -R 755 ${PWD}  
chown -R www:www ${PWD}  
ln -s ${PWD}/sql/glzjin_all.sql /www/backup/database/  

0x33 配置网站设置  

点击 网站 最右侧的设置，在 网站目录 里取消 防跨站攻击，运行目录里面选择 /public，点击 保存。  

在 伪静态 中填入下面内容：  

location / {  
    try_files $uri /index.php$is_args$args;  
}  
 
点击 保存 生效。  
0x34 导入初始数据库  

点击删库塔菜单的 数据库 按钮，找到你刚 Link 的数据库，点击导入。  

导入界面会出现 glzjin_all.sql，直接导入。  
0x35 配置网站程序  

在机器 Shell 上执行以下命令：  

cd /www/wwwroot/你的文件夹名/  
cp config/.config.example.php config/.config.php  
cp config/appprofile.example.php config/appprofile.php  
nano config/.config.php  

请按照自己的需求修改 config/.config.php，配置项比较多可以以后再改。为了下一步的正确执行请先务必确保数据库连接信息正确。  

数据库设置  

虽然并未完成所有配置，但是这个时候网站就可以打开了。  


0x36 创建管理员并同步用户    

依次执行以下命令：   

php xcat User createAdmin  
php xcat User resetTraffic  
php xcat Tool initQQWry  
php xcat Tool initdownload  

如果创建管理员出错请检查 config/.config.php 中的数据库连接信息。  

0x37 配置定时任务  

执行 crontab -e 命令，添加以下四条：  

30 22 * * * php /www/wwwroot/你的文件夹名/xcat SendDiaryMail  
0 0 * * * php -n /www/wwwroot/你的文件夹名/xcat Job DailyJob  
*/1 * * * * php /www/wwwroot/你的文件夹名/xcat Job CheckJob  

如果需要自动备份，可模仿以下两例，自行添加一条。  

每 20 分钟备份 1 次（若间隔大于 60 分钟看下个例子）：  

*/20 * * * * php -n /www/wwwroot/你的文件夹名/xcat Backup simple  

每 20 小时备份 1 次（若间隔大于 24 小时请学会熟练使用搜索引擎）：  

0 */20 * * * php -n /www/wwwroot/你的文件夹名/xcat Backup simple  

如果需要财务报表，可选添加以下三条：  
  
5 0 * * * php /www/wwwroot/你的文件夹名/xcat FinanceMail day  
6 0 * * 0 php /www/wwwroot/你的文件夹名/xcat FinanceMail week  
7 0 1 * * php /www/wwwroot/你的文件夹名/xcat FinanceMail month  
    
如果需要检测被墙，需要添加：  

*/1 * * * * php /www/wwwroot/你的文件夹名/xcat DetectGFW  

如果要用到 radius，需要添加下面这三条：  

*/1 * * * * php /www/wwwroot/你的文件夹名/xcat SyncRadius synclogin  
*/1 * * * * php /www/wwwroot/你的文件夹名/xcat SyncRadius syncvpn  
*/1 * * * * php -n /www/wwwroot/你的文件夹名/xcat SyncRadius syncnas  
