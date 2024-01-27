# Ansible 示例
## prometheus
使用 ansible 部署 Prometheus 监控组件
<br/>

**参数说明：**
`e` ：配置文件/配置项
  `cmd` ：
    - `install` ：安装
    - `remove` ：卸载
`t` ：Tag标签
  - `all` ：全部服务
  - `server` ：Prometheus Server 相关服务，含：Prometheus Server、Grafana、Alertmanager、Prometheus Alert、Prometheus Webhook Dingtalk、Loki
  - `exporter` ：Prometheus Exporter 相关服务，含：Node Exporter、CAdvisor、Mysqld Exporter、Mongodb Exporter、Redis Exporter、Rabbitmq Exporter、Rocketmq Exporter、Elasticsearch Exporter、Zookeeper Exporter、JMX Exporter、Nginx Prometheus Exporter、Blackbox Exporter、Grafana Promtail
  - `dockerce` ：Docker 服务
  - `node` ：Prometheus Node Exporter
  - `docker` ：Prometheus CAdvisor
  - `mysql` ：Prometheus Mysqld Exporter
  - `mongodb` ：Prometheus Mongodb Exporter
  - `redis` ：Prometheus Redis Exporter
  - `rabbitmq` ：Prometheus Rabbitmq Exporter
  - `rocketmq` ：Prometheus Rocketmq Exporter
  - `elasticsearch` ：Prometheus Elasticsearch Exporter
  - `zookeeper` ：Prometheus Zookeeper Exporter
  - `jmx` ：Prometheus JMX Exporter
  - `nginx` ：Nginx Prometheus Exporter
  - `blackbox` ：Prometheus Blackbox Exporter
  - `promtail` ：Grafana Promtail

<br/>

**安装：**
**安装全部组件：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=install -t all
```
**仅安装 server 相关组件：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=install -t server
```
**仅安装 exporter 相关组件：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=install -t exporter
```
**安装自定义组件：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=install -t 组件名称（名单见上方说明）
```
示例：安装 server 和 node exporter、cadvisor、blackbox exporter、promtail
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=install -t server,node,docker,blackbox,promtail
```
**仅安装 Docker 服务：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=install -t dockerce
```
**卸载：**
**卸载全部组件：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=remove -t all
```
**仅卸载 server 相关组件：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=remove -t server
```
**仅卸载 exporter 相关组件：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=remove -t exporter
```
**卸载自定义组件：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=remove -t 组件名称（名单见上方说明）
```
示例：卸载 server 和 node exporter、cadvisor、blackbox exporter、promtail
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=remove -t server,node,docker,blackbox,promtail
```
**卸载 Docker 服务：**
```shell
ansible-playbook /etc/ansible/roles/role.yml -e @/etc/ansible/roles/conf/extend.yml -e cmd=remove -t dockerce
```
**注意：**
1. 当前仅测试部署 Server 相关、Node Exporter、cAdvisor
2. Prometheus Alert、Prometheus Mongodb Exporter、Prometheus Rocketmq Exporter、Prometheus JMX Exporter、Prometheus Zookeeper Exporter 仅支持 x86_64 架构处理器，故当服务器 CPU 架构不为 x86_64 时，对应的服务不会安装，tag 也无效。
3. 当主机已经安装 Docker 服务时，需要先创建对应的 Docker 网络，然后再执行脚本
```bash
docker network create Docker网络名称 --subnet Docker网络网段
```
示例如下：
```bash
docker network create --driver bridge --opt encrypted:'true' --subnet 10.21.22.0/24 prometheus
```
