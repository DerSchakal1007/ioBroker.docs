---
BADGE-NPM version: http://img.shields.io/npm/v/iobroker.ds18b20.svg
BADGE-Downloads: https://img.shields.io/npm/dm/iobroker.ds18b20.svg
BADGE-Dependency Status: https://img.shields.io/david/crycode-de/iobroker.ds18b20.svg
BADGE-Known Vulnerabilities: https://snyk.io/test/github/crycode-de/ioBroker.ds18b20/badge.svg
BADGE-NPM: https://nodei.co/npm/iobroker.ds18b20.png?downloads=true
BADGE-Travis-CI: http://img.shields.io/travis/crycode-de/ioBroker.ds18b20/master.svg
translatedFrom: de
translatedWarning: 如果您想编辑此文档，请删除“translatedFrom”字段，否则此文档将再次自动翻译
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/zh-cn/adapterref/iobroker.ds18b20/README.md
title: 的ioBroker.ds18b20
hash: LjduaFrCQh7eq3mGR9Rnzp7lahaIy7p6MYD502wbBYM=
---
![商标](../../../de/adapterref/iobroker.ds18b20/../../admin/ds18b20.png)

＃ioBroker.ds18b20
适配器`ds18b20`可以将ioBroker中DS18B20类型的1-Wire温度传感器直接集成。

需要具有支持1-Wire总线的适当硬件（例如Raspberry Pi），并且必须设置1-Wire总线以在系统上运行（传感器位于`/sys/bus/w1/devices/`中）。

DS18B20传感器与Raspberry Pi的连接示例如下。

＃＃ 特征
*读取当前温度值
*自动检测连接的传感器
*查询传感器时的错误检测（校验和，通信错误，设备断开连接）
*可以调整每个传感器的查询间隔
*可以调整每个传感器的测量值的四舍五入和转换

##安装
当前可以从最新的存储库中获得适配器。

或者，可以通过URL`https://github.com/crycode-de/ioBroker.ds18b20.git`安装。

##配置
在适配器配置中，可以以毫秒为单位设置所有传感器的“标准轮询间隔” **。最小值为500。

各个传感器可以手动添加或通过*搜索传感器*添加到表格中。

![组态](../../../de/adapterref/iobroker.ds18b20/./img/konfiguration.png)

**地址**是传感器的1线地址/ ID，并且还确定对象ID。
例如，地址为`28-0000077ba131`的传感器接收对象ID`ds18b20.0.sensors.28-0000077ba131`。

可以自由选择**名称**以识别传感器。

可以为每个传感器指定一个额外的**查询间隔**（以毫秒为单位）。
如果该字段保留为空白，则应用标准查询间隔。
最小值为500。

**单位**确定ioBroker对象中存储的值的单位。
默认情况下，这是`°C`。

通过**因子**和**偏移**，可以根据公式`Wert = (Wert * Faktor) + Offset`调整传感器读取的值。

**小数点**表示该值四舍五入到小数点后多少位。
在计算后使用因子和偏移进行舍入。

**如果出现错误，则为零**定义读取传感器时如何处理错误。
如果设置了该选项，则在发生错误的情况下会将`null`值写入传感器的状态。
如果没有此选项，则在发生错误时不会更新状态。

###将`°C`转换为`°F`
因此，适配器在`°F`中返回测得的温度，必须将`1.8`作为因子，并将`32`作为偏移量。

##动作
通过写入状态`ds18b20.0.actions.readNow`，可以触发立即读取所有或特定传感器。

为了立即读取所有传感器，必须在该状态下编写关键字`all`。

如果仅要读取某个传感器，则必须将传感器的地址或ioBroker对象ID写入该状态。

##在脚本中使用
可以向适配器发送命令以读取传感器数据或搜索传感器。

###`readNow`
命令`readNow`触发立即查询所有或特定传感器。
要查询所有传感器，可以将消息部分保留为空，或者可以使用字符串`all`。
要读取特定的传感器，必须将消息部分设置为传感器的地址或ioBroker ID。

命令`readNow`不返回任何数据。它仅触发立即读取传感器。

```js
sendTo('ds18b20.0', 'readNow');
sendTo('ds18b20.0', 'readNow', '28-0000077ba131');
```

###`read`
可以使用`read`命令读取单个传感器。
必须将要读取的传感器的地址或ioBroker对象ID指定为消息部分。
使用回调函数可以进一步处理读取的值。

```js
sendTo('ds18b20.0', 'read', '28-0000077ba131', (ret) => {
    log('ret: ' + JSON.stringify(ret));
    if (ret.err) {
        log(ret.err, 'warn');
    }
});
```

###`search`
`search`命令搜索当前连接的1-wire传感器，并返回通过回调函数找到的传感器的地址。

```js
sendTo('ds18b20.0', 'search', {}, (ret) => {
    log('ret: ' + JSON.stringify(ret));
    if (ret.err) {
        log(ret.err, 'warn');
    } else {
        for (let s of ret.sensors) {
            log('Sensor: ' + s);
        }
    }
});
```

##适配器信息
通过`ds18b20.*.info.connection`状态，每个适配器实例都提供有关是否所有已配置的传感器都在传递数据的信息。
如果从所有传感器最后一次读取成功，则此状态为`true`。
一旦其中一个传感器显示错误，此状态即为`false`。

Raspberry Pi上的## DS18B20
DS18B20温度传感器已连接到Raspberry Pi，如下图所示。
请注意，上拉电阻必须连接到+ 3.3V而不是+ 5V，因为这会损坏GPIO。
在此示例中，使用了GPIO.04（BCM）。

![DS18B20 Raspberry Pi](../../../de/adapterref/iobroker.ds18b20/./img/raspi-ds18b20.png)

要在Raspberry Pi上激活1-Wire总线，必须将以下行添加到文件`/boot/config.txt`中，然后重新启动Raspberry Pi。

```
dtoverlay=w1-gpio,gpiopin=4
```

如果一切正常，则在`/sys/bus/w1/devices/`下可以看到已连接的传感器。

```
$ ls -l /sys/bus/w1/devices/
insgesamt 0
lrwxrwxrwx 1 root root 0 Nov  2 11:18 28-0000077b4592 -> ../../../devices/w1_bus_master1/28-0000077b4592
lrwxrwxrwx 1 root root 0 Nov  2 11:18 28-0000077b9fea -> ../../../devices/w1_bus_master1/28-0000077b9fea
lrwxrwxrwx 1 root root 0 Nov  2 10:49 w1_bus_master1 -> ../../../devices/w1_bus_master1
```

##在远程Raspberry Pi上集成传感器
还可以集成连接到远程Raspberry Pi的传感器。
为此，使用* Samba *在远程Raspberry Pi上释放相应的目录，然后将其安装在ioBorker系统上。

###远程Raspberry Pi上的配置
安装Samba：

```sh
sudo apt install samba
```

在文件`/etc/samba/smb.conf`中进行配置：

```ini
[ds1820]
path = /sys/devices/w1_bus_master1
comment = DS1820 Temperature sensors.
available = yes
browseable = yes
guest ok = yes
writeable = no
force user = root
force group = root
```

然后重新启动Samba以应用更改：

```sh
sudo systemctl restart samba
```

ioBroker系统上的配置
Samba客户端的安装：

```sh
sudo apt install smbclient
```

在文件`/etc/fstab`中为安装添加条目：

```
//<IP-ADRESSE-REMOTE-RPI>/ds1820 /mnt/remote-ds1820 cifs defaults,vers=1.0 0 0
```

为安装点创建目录：

```sh
sudo mkdir -p /mnt/remote-ds1820
```

挂载目录：

```sh
sudo mount /mnt/remote-ds1820
```

###适配器配置
在适配器配置中，然后必须将1-Wire器件的系统路径设置为安装点`/mnt/remote-ds1820`。

如果DS1820传感器也直接连接到ioBroker系统，则最好为远程传感器创建第二个适配器实例。

## Changelog
### 1.1.5 (2020-10-14)
* (Peter Müller) Fixed incorrect data type of object
* (Peter Müller) Updated dependencies

### 1.1.4 (2020-02-03)
* (Peter Müller) Updated connectionType and dataSource in io-package.json.

### 1.1.3 (2020-01-23)
* (Peter Müller) Added `connectionType` in `io-package.json` and updated dependencies.

### 1.1.2 (2020-01-22)
* (Peter Müller) Better handling of changed objects in admin.

### 1.1.1 (2020-01-09)
* (Peter Müller) Fixed wrong communication errror detection on some sensors.

### 1.1.0 (2019-11-11)
* (Peter Müller) Own implementation of reading the sensor data.
* (Peter Müller) Fixed bug on decimals rounding.
* (Peter Müller) 1-wire devices path is now configurable.

### 1.0.3 (2019-11-03)
* (Peter Müller) Added documentation about DS18B20 at a Raspberry Pi; Dependencies updated

### 1.0.2 (2019-10-07)
* (Peter Müller) Display error message when tried to search for sensors without adapter running.

### 1.0.1 (2019-10-01)
* (Peter Müller) Type changed to hardware, Renamed command, Added missing documentation

### 1.0.0 (2019-09-09)
* (Peter Müller) initial release

## License

Copyright (c) 2019-2020 Peter Müller <peter@crycode.de>

### MIT License

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.