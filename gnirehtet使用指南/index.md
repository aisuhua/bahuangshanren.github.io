# Gnirehtet使用指南


</br>
{{< style "text-align:center; strong{color:#00b1ff;}" >}}
**有空再更新**
{{< /style >}}
</br>

{{< admonition type=tip title="Gnirehtet项目地址" open=true >}}
[https://github.com/Genymobile/gnirehtet](https://github.com/Genymobile/gnirehtet)
{{< /admonition >}}

## 使用场景

{{< admonition type=example title="使用场景" open=true >}}
1. 电脑插着网线可以联网，但没有无线网卡，也没有随身WiFi，没法开热点。
2. 打游戏时想体验更快的速度。
3. ~~所以上面两条总结一下就是在学校机房打游戏？~~ 🤣
4. 使用场景确实不多，但有意思的是，锐捷校园网客户端v6.80 并不能检测到这种操作，更别提禁止了，所以😏
{{< /admonition >}}

```shell
Syntax: gnirehtet (install|uninstall|reinstall|run|autorun|start|autostart|stop|restart|tunnel|relay) ...
```

```shell
  gnirehtet install [serial]
      Install the client on the Android device and exit.
      If several devices are connected via adb, then serial must be
      specified.
```

```shell
  gnirehtet uninstall [serial]
      Uninstall the client from the Android device and exit.
      If several devices are connected via adb, then serial must be
      specified.
```

```shell
  gnirehtet reinstall [serial]
      Uninstall then install.
```

```shell
  gnirehtet run [serial] [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Enable reverse tethering for exactly one device:
        - install the client if necessary;
        - start the client;
        - start the relay server;
        - on Ctrl+C, stop both the relay server and the client.
```

```shell
  gnirehtet autorun [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Enable reverse tethering for all devices:
        - monitor devices and start clients (autostart);
        - start the relay server.
```

```shell
  gnirehtet start [serial] [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Start a client on the Android device and exit.
      If several devices are connected via adb, then serial must be
      specified.
      If -d is given, then make the Android device use the specified
      DNS server(s). Otherwise, use 8.8.8.8 (Google public DNS).
      If -r is given, then only reverse tether the specified routes.
      Otherwise, use 0.0.0.0/0 (redirect the whole traffic).
      If -p is given, then make the relay server listen on the specified
      port. Otherwise, use port 31416.
      If the client is already started, then do nothing, and ignore
      the other parameters.
      10.0.2.2 is mapped to the host 'localhost'.
```

```shell
  gnirehtet autostart [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Listen for device connexions and start a client on every detected
      device.
      Accept the same parameters as the start command (excluding the
      serial, which will be taken from the detected device).
```

```shell
  gnirehtet stop [serial]
      Stop the client on the Android device and exit.
      If several devices are connected via adb, then serial must be
      specified.
```

```shell
  gnirehtet restart [serial] [-d DNS[,DNS2,...]] [-p PORT] [-r ROUTE[,ROUTE2,...]]
      Stop then start.
```

```shell
  gnirehtet tunnel [serial] [-p PORT]
      Set up the 'adb reverse' tunnel.
      If a device is unplugged then plugged back while gnirehtet is
      active, resetting the tunnel is sufficient to get the
      connection back.
```

```shell
  gnirehtet relay [-p PORT]
      Start the relay server in the current terminal.
