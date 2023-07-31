# 启动API服务

## 通过py文件启动
可以通过直接执行`api.py`文件启动API服务，默认以ip:0.0.0.0和port:7861启动http和ws服务。
```shell
python api.py
```
同时，启动时支持StartOption所列的模型加载参数，同时还支持IP和端口设置。
```shell
python api.py --model-name chatglm-6b-int8 --port 7862 
```

## 通过cli.bat/cli.sh启动
也可以通过命令行控制文件继续启动。
```shell
cli.sh api --help
```
其他可设置参数和上述py文件启动方式相同。


# 以https、wss启动API服务
## 本地创建ssl相关证书文件
如果没有正式签发的CA证书，可以[安装mkcert](https://github.com/FiloSottile/mkcert#installation)工具， 然后用如下指令生成本地CA证书：
```shell
mkcert -install
mkcert api.example.com 47.123.123.123 localhost 127.0.0.1 ::1
```
默认回车保存在当前目录下，会有以生成指令第一个域名命名为前缀命名的两个pem文件。

附带两个文件参数启动即可。
````shell
python api --port 7862 --ssl_keyfile api.example.com+4-key.pem --ssl_certfile api.example.com+4.pem

./cli.sh api --port 7862 --ssl_keyfile api.example.com+4-key.pem --ssl_certfile api.example.com+4.pem
````

此外可以通过前置Nginx转发实现类似效果，可另行查阅相关资料。

## 多卡启动ChatGLM-6B和ChatGLM2-6B步骤
### 修改.env.template或则pilot/configs/config.py文件NUM_GPUS数量(数量为启动所需要实际的显卡数量)
### 同时需要在启动命令前指定所需要的显卡id（注意所指定显卡个数与NUM_GPUS数量保持一致），如下所示：
````shell
# 指定1块显卡
NUM_GPUS = 1
CUDA_VISIBLE_DEVICES=0 python3 pilot/server/dbgpt_server.py

# 指定4块显卡
NUM_GPUS = 4
CUDA_VISIBLE_DEVICES=3,4,5,6 python3 pilot/server/dbgpt_server.py
````
