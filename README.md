# 2019-public
# 脚本解析

## Init.sh
##### 变量介绍
| 全局变量名 | 变量含义  |
|:------------- |:---------------:| 
| HOMEDIR | 所有基本操作的工作目录 |
| DOCKERDIR | 所有微服务的工作目录 |
| DIRLIST | 工作目录下的一级子目录 |
| USER | 服务的启动用户、基本操作的工作用户 |
| PASSWORD | 用户密码 |
| MYSQLPASSWORD | 数据库密码 |
| DATE | 获取当前日期，方便日志的记录 |
| DOWNLOADWEB | 相关脚本的下载地址 |
| LNMPSHELL | lnmp自动安装脚本名 |
| DETAILLOG | 安装过程中的详细信息的记录日志 |
| LOGFILE | 安装历史信息的记录日志 |
| ERROTNUMBER | 记录脚本执行过程中的各种异常情况 |

##### 函数介绍
| 函数名 | 函数作用 |
|:------------- |:---------------:| 
| SetHostname | 根据出口ip的地址对照hosts信息自动设定主机名 |
| BuildDir | 创建所有标准化的目录结构 |
| CreateUSER | 创建需要工作用户、设置密码 |
| SetPermission | 将工作用户设为工作目录的所有者 |
| SetSudo | 安装启动sudo、设置工作用户的sudo权限 |
| InstallNginxAndMysql | 下载lnmp安装脚本，进行编译安装 |
| Usage | 提示各个参数的使用方法和作用 |
| PrintError | 打印各个错误编号的含义 |
| Start | 开始初始化部署 |
| Main | 主函数、解析参数信息 |

##### 使用方法
1. 查看帮助信息 `bash Init.sh -h` 或者 `bash Init.sh --help`
2. 查看错误序号含义 `bash Init.sh -e` 或者 `bash Init.sh --error`
3. 开始初始化部署 `bash Init.sh -s` 或者 `bash Init.sh --start`

##### 注意事项
1. 要注意检查yum的可用性。
2. 根据要求下发好hosts文件。
3. 脚本执行后根据报告的错误参数排查错误并予以相应的补救操作。

## BuildGateway.sh

##### 变量介绍
| 全局变量名 | 变量含义  |
|:------------- |:---------------:| 
| YUMLIST | 所有需要yum安装的软件包 |
| USER | 服务的启动用户、基本操作的工作用户 |
| HOMEDIR | 所有基本操作的工作目录 |
| MYSQLPASSWORD | 数据库密码 |
| CERTKEY | ijunhai证书的秘钥存放位置 |
| CERTPEM | ijunhai证书存放位置 |
| APIUSERNAME | api登录用户名 |
| APIPASSWORD | api登录密码 |
| DETAILLOG | 安装过程中的详细信息的记录日志 |
| LOGDIR | orange日志信息存放位置 |

##### 函数介绍
| 函数名 | 函数作用 |
|:------------- |:---------------:| 
| Init | 检查端口是否被占用、完成yum安装 |
| InstallOpenresty | 源码编译安装openresty |
| InstallLor | 编译安装lor框架 |
| InstallOrange | 下载安装自研orange系统、修改相关参数信息 |
| InitMaster | 为安装主服务器，进行导库操作，修改先关库中的数据 |
| InitSlaver | 为安装从服务器，修改配置文件指定主服务器的IP |
| StartOrange | 启动orange系统 |
| InstallOrangeCtl | 安装自研orangle_ctl控制系统 |
| SetPermission | 将工作目录及其子目录权限设置为工作用户(视情况开启或注销) |
| Usage | 提示各个参数的使用方法和作用 |
| PrintError | 打印各个错误编号的含义 |
| Master | 安装主服务器的流程控制函数 |
| Slaver | 安装从服务器的流程控制函数 |
| Main | 主函数 |


##### 使用方法
1. 查看帮助信息 `bash BuildGateway.sh -h` 或者 `bash BuildGateway.sh --help`
2. 查看错误序号含义 `bash BuildGateway.sh -e` 或者 `bash BuildGateway.sh --error`
3. 开始部署主服务器 `bash BuildGateway.sh -m` 或者 `bash BuildGateway.sh --master`
4. 开始部署从服务器 `bash BuildGateway.sh -s 主服务器IP` 或者 `bash BuildGateway.sh --slaver 主服务器IP`

##### 注意事项
1. 开始集群前确定初始化正常可用、数据库服务启动、证书已经部署到位。
2. 主服务器部署完毕后，需要给`sqlite3库`导入集群信息，然后启动`orange_ctl`控制服务。
3. 如果`/work/`工作目录下已经有文件，且权限不可变为`work`工作用户，请注释了`SetPermission()`函数模块内容。

## orange_control

##### 变量介绍
| 全局变量名 | 变量含义  |
|:------------- |:---------------:| 
| USER | 服务的启动用户、基本操作的工作用户 |
| HOMEDIR | 所有基本操作的工作目录 |
| CTLDIR | orange_ctl控制程序安装目录位置 |
| VENVFILE | orange_ctl需要的venv参数文件目录 |
| ORANGECONF | orange程序配置文件 |
| NGINXCONF | orange的nginx服务配置文件 |
| PREFIX | orange程序安装位置 |

##### 函数介绍
| 函数名 | 函数作用 |
|:------------- |:---------------:| 
| Start | 启动orange程序 |
| Reload | 是orange程序重新读取配置文件 |
| Stop | 停止orange程序 |
| Test | 检测orange配置是否符合规范 |
| Usage | 提示各个参数的使用方法和作用 |
| Main | 主函数 |

##### 使用方法
1. 查看帮助信息 `bash orange_control help`
2. 启动orange程序 `bash orange_control start`
3. 停止orange程序 `bash orange_control stop `
4. 重启orange程序 `bash orange_control restart`
5. 使orange程序重新读取配置 `bash orange_control reload`
6. 检查orange程序配置文件 `bash orange_control test`

##### 注意事项
1. 如果修改了相关程序的配置等目录位置，必须修改相应的系统参数。
2. 启动服务前注意观察端口的占用情况。
