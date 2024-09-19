# Shell 编程综合案例

<!-- toc -->

## 常用的 55 个 Linux Shell 脚本（包括基础案例、文件操作、实用工具、图形化、sed、gawk）

https://blog.csdn.net/cui_yonghua/article/details/131673690

## MySQL 数据库定时备份

每天凌晨 2:30 备份数据库 hspedu 到 /data/backup/db 备份开始和备份结束能够给出相应的提示信息

备份后的文件要求以备份时间为文件名，并打包成.tar.gz 的形式，比如：2021-03-12 230201.tar.gz

在备份的同时，检查是否有 10 天前备份的数据库文件，如果有就将其删除。

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost04/img/20240917110849.png)

对应脚本内容如下：

```shell
# 备份目录
BACKUP=/data/backup/db
# 当前时间
DATETIME=$(date +% Y-% m-% d_% H% M% S)
echo $DATETIME
# 数据库的地址
HOST=localhost
# 数据库用户名
DB_USER=root
# 数据库密码
DB_PW=hspedu100
# 备份的数据库名
DATABASE=hspedu
# 创建备份目录，如果不存在，就创建
[ ! -d "${BACKUP}/${DATETTME}" ] && mkdir -p "${BACKUP}/${DATETIME}"
# 备份数据库
mysqldump -u${DB_USER} -p${DB_PW} --host=${HOST} -q -R --databases ${DATABASE} | gzip > ${BACKUP}/${DATETIME}/$DATETIME.sql.gz

# 将文件处理成 tar.gz
cd ${BACKUP}
tar -zcvf $DATETIME.tar.gz ${DATETIME}
# 删除对应的备份目录
rm -rf ${BACKUP}/${DATETIME}
# 删除 10 天前的备份文件
find ${BACKUP} -atime +10 -name "*.tar.gz" -exec rm -rf {} \;

echo "备份数据库 ${DATABASE} 成功～"

```

开启定时任务指令

`crontab -e`

`30 2 * * * /usr/sbin/mysql_db_backup.sh`

`crontab -l`
