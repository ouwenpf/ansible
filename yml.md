# ansibe中文件的用法

## hosts

```bash
[test]
172.18.0.10[1:3]后面可接如下信息：
ansible_ssh_host  
ansible_ssh_port  
ansible_ssh_user  
ansible_ssh_pass  
ansible_sudo_pass 

172.18.0.10[1:3]后面也可以定义变量：
http_port=8080
http_port=8888

变量引用：{{ variables }}
```

条件测试when
```
- name: install conf file to centos7
  template: src=files/nginx.conf.j2 des=/files/nginx.conf
  when:  
    - ansible_distribution_major_version == "7"
```

列表迭代  
```
    yum: name={{ item.list }} state=latest
    with_items:
    - list: man
    - list: numactl-libs
    - list: libaio
```

## 角色
每个角色，以特定的层级目录进行组织结构：   
mysql/   
- files：存放有copy或script模块等调用的文件   
- tasks：至少包含一个mail.yml的文件，其它的文件需要在此文件中通过include进行包含   
- vars：至少包含一个mail.yml的文件，其它的文件需要在此文件中通过include进行包含  
- group_vars：定义全局的变量，根据不同的角色进行定义相关的文件名和变量
- handlers：至少包含一个mail.yml的文件，其它的文件需要在此文件中通过include进行包含   
- templates：templates模块查找所需要模板文件的目录   
- default：设定默认变量时使用此目录中的main.yml文件  
- meta：至少包含一个mail.yml的文件，定义当前角色的特殊设定及其依赖关系，其它的文件需要在此文件中通过include进行包含   

## 角色定义

```bash
vim /etc/ansible/ansible.cfg
roles_path    = /etc/ansible/roles:/usr/share/ansible/roles 角色默认存放目录     


```

## 实例创建MySQL为例

- 创建角色目录  
`mkdir -p /etc/ansible/roles/mysql{files,tasks,handlers,vars,templates}`  
`ansible 172.18.0.101 -m setup `  
`ansible-playbook -i hosts mysql.yml`   
 
**具体内容参考roles目录中的内容**   
