1. 首先用`dpkg --list|grep mysql`查看自己的mysql有哪些依赖

2. 先卸载`sudo apt-get remove mysql-common`

3. 然后：`sudo apt-get autoremove --purge mysql-server-5.X `

4. 再用`dpkg --list|grep mysql`查看，还剩什么就卸载什么

5. 最后清除残留数据：`dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P`



————————————————
版权声明：本文为CSDN博主「EricJeff_」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/w3045872817/java/article/details/77334886

