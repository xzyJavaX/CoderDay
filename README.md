# CoderDay
## 简介
记录我们的实习生活

## 文件简介  
排名按各大公司拼音的首字母进行排序  
1.baidu_jieni  
巴拉巴拉
2.lizhi_JavaX  
巴拉巴拉


欢迎各个小伙伴对本项目进行PR，文件夹名称格式：  
公司名拼音_你的昵称  
例如Alibaba_lihua

## 使用方法
- **注意：root权限下运行，不然无法修改v2ray配置**
- **注意：root权限下运行，不然无法修改v2ray配置**
- **注意：root权限下运行，不然无法修改v2ray配置**

要求：python3环境，linux

切记切记:先修改config.py里的配置项，用户名和密码一定要改

1. 运行 sudo ./install.sh
2. 按照脚本操作后，将会部署到后台
3. 部署完成后访问http://127.0.0.1:8000

开机自启自行部署：https://github.com/Supervisor/initscripts

## 启动停止方法
1. 启动： `supervisorctl start v2rayClient`
2. 停止： `supervisorctl stop v2rayClient`


## 配置方法

具体写入的日志和配置文件在config.py里自行修改


## 免责声明

本项目只是在闲暇之际对flask框架进行的学习,只为个人linux使用,其余事项与本人概无关系!
本项目只是在闲暇之际对flask框架进行的学习,只为个人linux使用,其余事项与本人概无关系!
本项目只是在闲暇之际对flask框架进行的学习,只为个人linux使用,其余事项与本人概无关系!

#### install_test.sh 是热心网友提供的一键安装脚本，会自动安装环境,下面两个安装脚本都一样的,下载方式不同而已,也可以进行测试安装.

``` 
wget -c https://github.com/NoOne-hub/v2ray_client/archive/master.tar.gz && tar xzf master.tar.gz && cd v2ray_client-master && ./install_test.sh
```

```
git clone https://github.com/NoOne-hub/v2ray_client.git && cd /v2ray_client && ./install_test.sh
```

### 启动停止方法
1. 启动： `supervisorctl -c /etc/supervisor/supervisord.conf start v2rayClient`
2. 停止： `supervisorctl -c /etc/supervisor/supervisord.conf stop v2rayClient`
