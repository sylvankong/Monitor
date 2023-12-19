## 波场能量自动闪兑，多地址监听，自定义价格，

> * 支持添加多地址监听，每个地址可设置不同价格，比如A地址，2.0TRX，B地址，3.0TRX。在data.json配置即可。
> * 如果只监听1个地址，把示例的第二个配置删掉即可，请注意检查json格式是否正确。

#### ⚠️🚫 部署成功后不要往设置的TRC地址内转TRX，因为都会转换成对应能量回到原地址，如确需转入，短暂停止程序即可。
1. 下载最新的压缩包， 👉[Release](https://github.com/AE86X/Monitor/releases/)，请按照对应的服务器架构下载，如果你服务器不是arm架构，你就下载默认x86即可，下载完成解压，目录下有3个文件，`monitor` 和  `config.yaml` 以及 `data.json`
2. `Monitor` 是程序的可执行文件，也就是主程序，运行的就是这个文件。
3. `config.yaml` 是日志配置和波场官方API请求的ApiKey，日志配置不用动，波场的APIKEY，建议去 https://www.trongrid.io/ 注册，每天可请求10W次，不申请可能没影响，但是怕请求频繁，被波场官方限制（强烈建议申请）。
4. `data.json` 是重点，可参考示例配置，key为你监听入账的地址，value 对应的是 **price**，是你需要设置的能量单价，设置 3TRX = 32000，就写3.0，**app_key**为你调用能量发送的APIKEY，提前去 https://t.me/XXTrxBot 申请，点击查看apikey复制即可。

		{
		  "TXK9aDTxFL78zCmc5QWLBr6ti5vP999999": {
		    "price": 2.0,
		    "app_key": "KEY12310212129109"
		  },
		  "TXK9aDTxFL78zCmc5QWLBr6ti5vPXXXXXX": {
		    "price": 3.0,
		    "app_key": "KEY12310212129109"
		  }
		}
		
5. 以上配置完成，即可启动程序，先给予`monitor`可执行权限，命令行输入`chmod +x monitor` 即可。
6. 启动程序，在当前目录下输入命令 `./monitor`即可启动。
7. 到这一步就算是正式启动了，但是你会发现无法后台运行，你关掉终端，程序就停了，为了保证程序持续运行，请看第4步。
8. 配置supervisor (不太熟悉下面流程的建议安装宝塔，搜索supervisor管理器，添加路径`/root/tron`，可执行文件填写`/root/tron/monitor`即可，这里的`/root/tron/monitor `不要照抄，根据你的可执行文件的位置来)
9. 如果你使用宝塔配置成功，就无需往下看了，如果不用宝塔，请看第6步。
10. 为了保证`monitor`常驻后台运行，我们需要配置`supervisor`来实现进程监听  (supervisor的安装，可参考[链接](https://learnku.com/laravel/t/3592/using-supervisor-to-manage-laravel-queue-processes))
11. 编辑配置文件`vi /etc/supervisor/conf.d/monitor.conf`

你可以参考以下配置文件，注意更改路径，编辑完记得保存。

	[program:monitor]
	process_name=monitor
	directory=/root/tron
	command=/root/tron/monitor
	autostart=true
	autorestart=true
	user=root
	numprocs=1
	redirect_stderr=true
	startsecs=3


接下来输入命令 `supervisorctl update` 不报错即执行成功

查看运行状态 `supervisorctl status` 显示 RUNNING 即正在运行


#### ⚠️其他注意事项
* 如您需要更改监听地址，更改单价，或新增监听地址，修改`data.json`即可，修改一定重启supervisor进程 `supervisorctl restart monitor `，否则最新配置无法生效。
