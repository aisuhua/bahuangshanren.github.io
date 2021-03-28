# Gnirehtet使用笔记


{{< admonition type=tip title="注意" open=true >}}
- 项目仓库地址：[https://github.com/Genymobile/gnirehtet](https://github.com/Genymobile/gnirehtet)
- 别忘了打开 `开发者选项` 中的 `USB调试` 和 `USB安装` 
- 在任意终端中使用 gnirehtet 的以下命令
- 推荐将 gnirehtet 添加到系统环境变量中方便调用
{{< /admonition >}}

## 使用场景

{{< admonition type=example title="使用场景" open=true >}}
1. 电脑插着网线可以联网，但没有无线网卡，也没有随身WiFi，没法开热点。
2. 打游戏时想体验更快的速度。
3. ~~所以上面两条总结下就是在学校机房打游戏？~~ 🤣
4. 使用场景确实不多，但有意思的是，锐捷校园网客户端v6.80 并不能检测到这种操作，更别提禁止了，所以😏
{{< /admonition >}}

## Syntax

```shell
gnirehtet (install|uninstall|reinstall|run|autorun|start|autostart|stop|restart|tunnel|relay) ...
```

### gnirehtet install

```shell
  gnirehtet install [serial]
      Install the client on the Android device and exit.
      If several devices are connected via adb, then serial must be specified.
```

- 此命令用于手动向安卓设备上安装 gnirehtet，设备的 `serial` 使用 `adb devices` 查看。
- 中括号 `[]` 内的参数是可选的，所以只有一台安卓设备与电脑连接时，可以不用输入 `serial` 。

> 如果安卓设备上没有安装 gnirehtet，后续运行 `gnirehtet run` 命令时，也会自动向设备安装 gnirehtet。

### gnirehtet uninstall

```shell
  gnirehtet uninstall [serial]
      Uninstall the client from the Android device and exit.
      If several devices are connected via adb, then serial must be specified.
```

此命令用于卸载指定安卓设备上的 gnirehtet 。

### gnirehtet reinstall

```shell
  gnirehtet reinstall [serial]
      Uninstall then install.
```

此命令用于重新安装指定安卓设备上的 gnirehtet 。

### gnirehtet run

```shell
  gnirehtet run [serial] [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Enable reverse tethering for exactly one device:
        - install the client if necessary;
        - start the client;
        - start the relay server;
        - on Ctrl+C, stop both the relay server and the client.
```

- 运行 `gnirehtet run` 命令，启动电脑和安卓设备上的 gnirehtet 。
- 默认DNS为 `8.8.8.8` ，使用 `-d` 参数指定DNS，比如 `-d 223.5.5.5,223.6.6.6` 。
- 默认端口为 `31416` ，使用参数 `-p` 指定端口。
- 默认路由为 `0.0.0.0/0` ，使用参数 `-r` 指定路由。
- 按 `Ctrl+C` 结束电脑和安卓设备上 gnirehtet 的运行。

### gnirehtet autorun

```shell
  gnirehtet autorun [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Enable reverse tethering for all devices:
        - monitor devices and start clients (autostart);
        - start the relay server.
```

`gnirehtet autorun` 和 `gnirehtet run` 的区别是：

- `gnirehtet autorun` 自动启动电脑和安卓设备上的 gnirehtet 
- 而 `gnirehtet run` 只能启动一台指定设备的 gnirehtet 。

### gnirehtet start

```shell
  gnirehtet start [serial] [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Start a client on the Android device and exit.
      If several devices are connected via adb, then serial must be specified.
      If -d is given, then make the Android device use the specified
      DNS server(s). Otherwise, use 8.8.8.8 (Google public DNS).
      If -r is given, then only reverse tether the specified routes.Otherwise, use 0.0.0.0/0 (redirect the whole traffic).
      If -p is given, then make the relay server listen on the specified port. Otherwise, use port 31416.
      If the client is already started, then do nothing, and ignore the other parameters.
      10.0.2.2 is mapped to the host 'localhost'.
```

- 此命令用于启动指定安卓设备上的 gnirehtet 。
- 只有安卓设备上的 gnirehtet 停止时，才需要手动启动 gnirehtet 。

### gnirehtet autostart

```shell
  gnirehtet autostart [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Listen for device connexions and start a client on every detected
      device.
      Accept the same parameters as the start command (excluding the
      serial, which will be taken from the detected device).
```

此命令用于自动启动所有与电脑连接的安卓设备上的 gnirehtet 。

### gnirehtet stop

```shell
  gnirehtet stop [serial]
      Stop the client on the Android device and exit.
      If several devices are connected via adb, then serial must be specified.
```

此命令用于停止指定安卓设备上的 gnirehtet 。

### gnirehtet restart

```shell
  gnirehtet restart [serial] [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Stop then start.
```

此命令用于重新启动指定安卓设备上的 gnirehtet 。

### gnirehtet tunnel

```shell
  gnirehtet tunnel [serial] [-p PORT]
      Set up the 'adb reverse' tunnel.
      If a device is unplugged then plugged back while gnirehtet is
      active, resetting the tunnel is sufficient to get the connection back.
```

此命令用于连接中断后的自动重连。

### gnirehtet relay

```shell
  gnirehtet relay [-p PORT]
      Start the relay server in the current terminal.
```
在当前终端中启动中继服务器。

{{< admonition type=tip title="提示" open=true >}}
一般只用这条命令：`gnirehtet run -d 223.5.5.5,223.6.6.6` ，偶尔用用 `gnirehtet tunnel` 
{{< /admonition >}}
