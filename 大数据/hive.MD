# Hive学习笔记

## 安装Hive
```sh
su - hadoop
wget https://archive.apache.org/dist/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
tar -zxvf apache-hive-3.1.3-bin.tar.gz -C /export/server/
rm -rf apache-hive-3.1.3-bin.tar.gz
ln -s /export/server/apache-hive-3.1.3-bin /export/server/hive
# 下载mysql驱动包
wget https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.34/mysql-connector-java-5.1.34.jar
# 将jar包移动到hive的lib目录下
mv mysql-connector-java-5.1.34.jar /export/server/hive/lib/
# 创建hive-env.sh文件
cp /export/server/hive/conf/hive-env.sh.template /export/server/hive/conf/hive-env.sh
# 写入环境变量
echo -e "export HADOOP_HOME=/export/server/hadoop\n export HIVE_CONF_DIR=/export/server/hive/conf\n export HIVE_AUX_JARS_PATH=/export/server/hive/lib" >> /export/server/hive/conf/hive-env.sh
export HADOOP_HOME=/export/server/hadoop
export HIVE_CONF_DIR=/export/server/hive/conf
export HIVE_AUX_JARS_PATH=/export/server/hive/lib
# 创建HIVE数据库
CREATE DATABASE hive CHARSET UTF8;
# 执行元数据库初始化命令：
cd /export/server/hive
bin/schematool -initSchema -dbType mysql -verbos

# 确保Hive文件夹所属为hadoop用户
# 创建一个hive的日志文件夹： 
mkdir /export/server/hive/logs
# 启动元数据管理服务（必须启动，否则无法工作）
# 前台启动：
bin/hive --service metastore 
# 后台启动：
nohup bin/hive --service metastore >> logs/metastore.log 2>&1 &
# 启动客户端，二选一（当前先选择Hive Shell方式）
# Hive Shell方式（可以直接写SQL）： 
bin/hive
# Hive ThriftServer方式（不可直接写SQL，需要外部客户端链接使用）： 
bin/hive --service hiveserver2
nohup bin/hive --service hiveserver2 >> logs/hiveserver2.log 2>&1 &
```

## Hive使用
1. 创建表
```sql
CREATE TABLE test(id INT, name STRING, gender STRING);
```
2. 插入数据
```sql
INSERT INTO test VALUES(1, '王力红', '男'), (2, '周杰轮', '男'), (3, '林志灵', '女');
```
3. 查询数据
```sql
SELECT gender, COUNT(*) AS cnt FROM test GROUP BY gender;
```

