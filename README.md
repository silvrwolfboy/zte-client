# 城院校园网Linux客户端 v3.0 Beta

### 简介
本客户端为Linux中兴认证客户端，已适配东莞理工学院城市学院。

参考Dot1x

### 编译
需要安装以下运行库:
libcurl, libleptonica, tesseract


####For Debian Jessie
```
apt-get install libcurl4-openssl-dev libleptonica-dev tesseract-ocr-dev tesseract-ocr-eng gcc git make cmake
git clone https://git.coding.net/yzs/zte-client.git
cd zte-client
cmake CMakeLists.txt
make
cp ./zte_client /usr/sbin/zte-client
```

####交叉编译
Debian Jessie可参考此处安装Toolchains: [CrossToolchains#For_jessie_.28Debian_8.29](https://wiki.debian.org/CrossToolchains#For_jessie_.28Debian_8.29) 

OpenWRT/PandoraBox可到官网下载Toolchains

需要先交叉编译好curl, leptonica, tesseract, libjpeg，库编译完毕后，参考以下命令编译客户端
```
git clone https://git.coding.net/yzs/zte-client.git
cd zte-client
mkdir -p mipsel-build
cd mipsel-build
mipsel-linux-gnu-gcc -I/usr/local/libcurl-mipsel/include/ -I/usr/local/libleptonica-mipsel/include/ ../main.c ../src/zte.c ../src/dhcpClient.c ../src/exception.c ../src/webAuth.c -c
mipsel-linux-gnu-g++ main.o zte.o dhcpClient.o exception.o webAuth.o /usr/local/libcurl-mipsel/lib/libcurl.a /usr/local/libleptonica-mipsel/lib/liblept.a /usr/local/tesseract-mipsel/lib/libtesseract.a /usr/lib/mipsel-linux-gnu/libjpeg.a -lpthread -static-libstdc++ -static-libgcc -lrt -o zte-client
cp ./zte_client /usr/sbin/zte-client
```



### 使用说明
```
必要参数(使用-l, -r参数不要求以下参数):

	-u, --zteuser		中兴认证用户名
	-p, --ztepass		中兴认证密码
	-d, --device		指定网卡设备，默认为第一个可用网卡设备


可选参数:

	-w, --webuser		天翼认证用户名
	-k, --webpass		天翼认证密码
	-f, --pidfile		pid文件路径，默认为/tmp/zte-client.pid
	-m, --logfile       日志文件路径，前台运行模式下默认输出到标准输出，后台运行模式下默认重定向到/dev/null
	-i, --dhcp		指定DHCP客户端，可以选择dhclient, udhcpc或none(代表不启用DHCP客户端)，默认为dhclient
	-b, --daemon		以守护进程模式运行
	-r, --reconnect		重新连接
	-l, --logoff		注销
	-h, --help		显示帮助信息
```
### 示例
进行中兴认证与天翼认证，并以守护进程模式运行:
```
/usr/sbin/zte-client --zteuser username --ztepass password --webuser webusername --webpass webpassword --device eth0 --daemon
```

注销:
```
/usr/sbin/zte-client -l
```

重新进行认证:
```
/usr/sbin/zte-client -r
```

### 感谢
Dot1x

802.11x协议信息

### 链接
[MIT License](https://opensource.org/licenses/MIT)

[Zhensheng Yuan's weblog: http://zhensheng.im](http://zhensheng.im)

### 协议
**The MIT License (MIT)**

Copyright (c) 2016 Zhensheng Yuan

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.