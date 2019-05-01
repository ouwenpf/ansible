# ansible使用

- -m指定模块
- -a模块参数
- -k密码
- -f每次分发的主机个数  
- -C 测试
### user模块

```bash
--创建
ansible all -m user -a 'name=mysql create_home=no system=yes shell=/sbin/nologin'
--删除
ansible all -m user -a 'name=test remove=yes state=absent'
--如需更多了解
ansible-doc -s user

ssh-keygen -t rsa -P "" -f ~/.ssh/id_rsa
ssh-copy-id -p52553 192.168.0.100
```
### ping模块

```bash
ansible  all   --list-hosts
ansible  all   -m  ping
```

### copy模块
```bash
用法：
	1. scr=		dest=
	2. content=	dest=
	owner属主,group属组,mode权限
	
	ansible all -m copy -a 'src=/etc/fstab dest=/tmp/fs.ansible'
	ansible all -m copy -a 'src=/etc/pam.d dest=/tmp/'
	
	--ansible  172.18.0.101  -m fetch -a 'src=/etc/services dest=/root'
	远程主机上复制文件到本地，使用fetch，只能复制文件不能是目录
	注意：目录不加'/'为递归复制整个目录，加'/'只复制目录中的文件不包含文件本身
```

### shell模块
```bash
ansible all -m shell -a 'command'
```

### file模块
```bash
ansible all -m file -a "path=/tmp/test1 state=directory"
state:directory touch

```
### cron模块
```bash
ansible all -m cron -a "minute=*/5 job='/usr/sbin/ntpdate  -u asia.pool.ntp.org &> /dev/null' name='date'"

--minute
--hour
--day
--month
--weekday
```

### yum模块
```bash
ansible all -m yum -a 'name=crontabs state=present' 
state：present absent latest
```

### service模块
```bash
ansible all -m service -a "name=nginx  state=started"
```


### script模块
```bash
ansible all -m script -a '/tmp/test.sh' 
```