rm

Notes:  
The routes.php has been move to app/ since the update committed on Dec 18, 2019 


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
