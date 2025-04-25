# hadoop学习笔记


## VMware虚拟机部署HDFS集群
### 1.安装包下载
```sh
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.1/hadoop-3.4.1.tar.gz
```

### 2.集群规划
|节点|CPU|内存|服务|
|---|---|---|---|
|node1|1核心|4GB|NameNode、DataNode、SecondaryNameNode|
|node2|1核心|2GB|DataNode|
|node3|1核心|2GB|DataNode|

### 3.上传&&解压

1. 上传hadoop安装包到node1节点中
2. 解压缩安装包到/export/server中
```sh
tar -zxvf hadoop-3.4.1.tar.gz -C /export/server
```
3. 构建软链接
```sh
cd /export/server
ln -s /export/server/hadoop-3.4.1 hadoop
```
4. 进入hadoop安装包内
```sh
cd hadoop
```

### 4.Hadoop安装包目录结构
- bin，存放Hadoop的各类程序（命令）
- etc，存放Hadoop的配置文件
- include，C语言的头文件
- lib，存放Linux系统的动态链接库（.so文件）
- libexec，存放配置Hadoop系统的脚本文件（.sh和.cmd）
- licenses-binary，存放许可证文件
- sbin，管理员程序（super bin）
- share，存放二进制源码（java jar包）

### 5.修改配置文件

> - workers：配置从节点（DataNode）
> - hadoop-env.sh：喷子和Hadoop的相关环境变量
> - core-site.xml：Hadoop核心配置文件
> - hdfs-site.xml：hdfs核心配置文件
1. 配置workers文件
```sh
cd etc/hadoop
vim workers
node1
node2
node3
```
2. 配置hadoop-env.sh文件
```sh
export JAVA_HOME=/export/server/jdk
export HADOOP_HOME=/export/server/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_LOG_DIR=$HADOOP_HOME/logs
```

3. 配置core-site.xml文件
```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://node1:8020</value>
  </property>

  <property>
    <name>io.file.buffer.size</name>
    <value>131072</value>
  </property>
</configuration>
```

4. 配置hdfs-site.xml
```xml
<configuration>
  <property>
    <name>dfs.datanode.data.dir.perm</name>
    <value>700</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/data/nn</value>
  </property>
  <property>
    <name>dfs.namenode.hosts</name>
    <value>node1,node2,node3</value>
  </property>
<property>
    <name>dfs.blocksize</name>
    <value>268435456</value>
  </property>
  <property>
    <name>dfs.namenode.handler.count</name>
    <value>100</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>/data/dn</value>
  </property>
</configuration>
```

5. 配置环境变量
```sh
vim /etp/profile
export HADOOP_HOME=/export/server/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

6. 复制hadoop目录至node2和node3节点
```sh
scp -r hadoop-3.4.1 node2:`pwd`/
scp -r hadoop-3.4.1 node3:`pwd`/
ln -s hadoop-3.4.1 hadoop
```

7. 创建namenode和datanode存放目录
```sh
mkdir -p /data/nn
mkdir -p /data/dn
```

8. 授权为hadoop用户
```sh
chown -R hadoop:hadoop /data
chown -R hadoop:hadoop /export
```

9. 格式化文件系统
```sh
su - hadoop
hadoop namenode -format
```

10. 启动
```sh
# 一键启动集群
start-dfs.sh
# 一键关闭dfs集群
stop-dfs.sh
```

## MapReduce & YARN 的部署

### 1.部署说明
|组件|配置文件|启动进程|备注|
|---|---|---|---|
|Hadoop HDFS|需修改|<div>需启动</div><ul><li>NameNode作为主节点</li><li>DataNode作为从节点</li><li>SecondaryNameNode主节点辅助</li></ul>|分布式文件系统|
|Hadoop YARN|需修改|<div>需启动</div><ul><li>ResourceManager作为集群资源管理者</li><li>NodeManager作为单机资源管理者</li><li>ProxyServer代理服务器提供安全性</li><li>JobHistoryServer记录历史信息和日志</li></ul>|分布式资源调度|
|Hadoop MapReduce|需修改|<div>无需启动任何进程</div><div>MapReduce程序运行在YARN容器内</div>|分布式数据计算|

### 2.集群规划
|主机|角色|
|--|--|
|node1|<ul><li>ResourceManager</li><li>NodeManager</li><li>ProxyServer</li><li>JobHistoryServer</li></ul>|
|node2|NodeManager|
|node3|NodeManager|

### 3.修改MapReduce配置文件

1. 修改mapred-env.sh文件
```sh
cd $HADOOP_HOME/etc/hadoop
vim mapred-env.sh
export JAVA_HOME=/export/server/jdk
export HADOOP_JOB_HISTORYSERVER_HEAPSIZE=1000
export HADOOP_MAPRED_ROOT_LOGGER=INFO,RFA
```

2. 修改mapred-site.xml
```sh
vim mapred-site.xml
```
```xml
<configuration>
  <property>
    <name>dfs.datanode.data.dir.perm</name>
    <value>700</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/data/nn</value>
  </property>
  <property>
    <name>dfs.namenode.hosts</name>
    <value>node1,node2,node3</value>
  </property>
<property>
    <name>dfs.blocksize</name>
    <value>268435456</value>
  </property>
  <property>
    <name>dfs.namenode.handler.count</name>
    <value>100</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>/data/dn</value>
  </property>
</configuration>
```

3. 修改yarn-env.sh文件
```sh
vim yarn-env.sh
export JAVA_HOME=/export/server/jdk
export HADOOP_HOME=/export/server/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_LOG_DIR=$HADOOP_HOME/logs
```

4. 修改yarn-site.xml文件
```sh
vim yarn-site.xml
```
```xml
<configuration>
<!-- Site specific YARN configuration properties -->
<property>
    <name>yarn.log.server.url</name>
    <value>http://node1:19888/jobhistory/logs</value>
    <description></description>
</property>
  <property>
    <name>yarn.web-proxy.address</name>
    <value>node1:8089</value>
    <description>proxy server hostname and port</description>
  </property>
  <property>
    <name>yarn.log-aggregation-enable</name>
    <value>true</value>
    <description>Configuration to enable or disable log aggregation</description>
  </property>
  <property>
    <name>yarn.nodemanager.remote-app-log-dir</name>
    <value>/tmp/logs</value>
    <description>Configuration to enable or disable log aggregation</description>
  </property>
<!-- Site specific YARN configuration properties -->
  <property>
    <name>yarn.resourcemanager.hostname</name>
    <value>node1</value>
    <description></description>
  </property>
  <property>
    <name>yarn.resourcemanager.scheduler.class</name>
    <value>org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler</value>
    <description></description>
  </property>
  <property>
    <name>yarn.nodemanager.local-dirs</name>
    <value>/data/nm-local</value>
    <description>Comma-separated list of paths on the local filesystem where intermediate data is written.</description>
  </property>
  <property>
    <name>yarn.nodemanager.log-dirs</name>
    <value>/data/nm-log</value>
    <description>Comma-separated list of paths on the local filesystem where logs are written.</description>
  </property>
  <property>
    <name>yarn.nodemanager.log.retain-seconds</name>
    <value>10800</value>
    <description>Default time (in seconds) to retain log files on the NodeManager Only applicable if log-aggregation is disabled.</description>
  </property>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
    <description>Shuffle service that needs to be set for Map Reduce applications.</description>
  </property>
</configuration>
```

5. 文件分发至其他服务器
```sh
scp mapred-env.sh mapred-site.xml yarn-env.sh yarn-site.xml node2:`pwd`/
scp mapred-env.sh mapred-site.xml yarn-env.sh yarn-site.xml node3:`pwd`/
```

6. 启动集群命令介绍
```sh
# 一键启动YARN集群： 
$HADOOP_HOME/sbin/start-yarn.sh
# 一键停止YARN集群： 
$HADOOP_HOME/sbin/stop-yarn.sh
# 在当前机器，单独启动或停止进程
$HADOOP_HOME/bin/yarn --daemon start|stop resourcemanager|nodemanager|proxyserver
# 历史服务器启动和停止
$HADOOP_HOME/bin/mapred --daemon start|stop historyserver
```

7. 开始启动YARN集群
```sh
$HADOOP_HOME/sbin/start-yarn.sh
$HADOOP_HOME/bin/mapred --daemon start historyserver
```