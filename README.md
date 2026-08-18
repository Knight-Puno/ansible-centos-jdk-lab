# Ansible CentOS + JDK 入门小项目

## 项目目标

使用 `192.168.205.140` 作为 Ansible 控制机，统一管理：

- `192.168.205.138`
- `192.168.205.139`
- `192.168.205.140`

完成：CentOS 基础初始化、批量安装配置 JDK，并练习常用关键字。

## 项目结构

```text
ansible-centos-jdk-lab/
├── inventory/
│   └── hosts.ini
├── vars/
│   └── common.yml
├── playbooks/
│   ├── init.yml
│   ├── install_jdk.yml
│   └── demo.yml
└── README.md
```

## 1. 测试主机连通性

```bash
ansible jdk_servers -i inventory/hosts.ini -m ping
```

## 2. 初始化系统

```bash
ansible-playbook -i inventory/hosts.ini playbooks/init.yml
```

只换源：

```bash
ansible-playbook -i inventory/hosts.ini playbooks/init.yml --tags repo
```

只安装基础软件：

```bash
ansible-playbook -i inventory/hosts.ini playbooks/init.yml --tags packages
```

## 3. 安装 JDK

确保控制机存在：

```text
/opt/download/jdk1.8.0_121.zip
```

执行：

```bash
ansible-playbook -i inventory/hosts.ini playbooks/install_jdk.yml
```

验证：

```bash
java -version
javac -version
echo $JAVA_HOME
```

## 4. 练习关键字

```bash
ansible-playbook -i inventory/hosts.ini playbooks/demo.yml
```

`demo.yml` 演示：

- `vars`
- `loop`
- `register`
- `when`
- `debug`
- facts
- `tags`

## 5. 本项目涉及的常用关键字

Play 级别：

```text
name
hosts
become
vars
vars_files
tasks
gather_facts
```

Task 级别：

```text
when
register
loop
tags
changed_when
failed_when
```

下一阶段：

```text
notify
handlers
block
rescue
always
delegate_to
run_once
serial
```

## 6. 本项目涉及的常用模块

```text
file
copy
unarchive
yum
systemd
command
debug
stat
get_url
replace
blockinfile
find
```

## 7. 最重要的 Ansible 思维

Shell 更像：

```text
执行 mkdir
执行 cp
执行 chmod
执行 rm
```

Ansible 更像：

```text
确保目录存在
确保文件已经复制
确保权限正确
确保临时文件不存在
```

也就是尽量让 Playbook 具有幂等性：执行一次达到目标状态，重复执行仍保持目标状态。

## 8. 推荐学习顺序

```text
inventory
  ↓
Playbook 基础
  ↓
vars / facts
  ↓
when
  ↓
loop
  ↓
register
  ↓
handlers + notify
  ↓
template
  ↓
group_vars / host_vars
  ↓
roles
```

> 注意：项目里的系统初始化适合实验环境。网络配置和重启 network 没有放进默认剧本，避免远程执行时把 SSH 连接直接断掉。
# ansible-centos-jdk-lab
