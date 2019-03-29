# maxmcd/webtty [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 WebTTY 允许您使用 WebRTC 共享 你计算机的终端会话。 」

[中文](./readme.md) | [english](https://github.com/maxmcd/webtty)

---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'maxmcd/webtty' -->
<!-- commit = '322d619be964b422be109110b814de3de9c9bae5' -->
<!-- time = '2018-11-15' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018-11-15 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/maxmcd/webtty.svg
[commit]: https://github.com/maxmcd/webtty/tree/322d619be964b422be109110b814de3de9c9bae5

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[If help, **buy** me coffee —— 营养跟不上了，给我来瓶营养快线吧! 💰](https://github.com/chinanf-boy/live-need-money)

---

![](https://maxmcd.github.io/webtty/out.gif)

## WebTTY

WebTTY 允许您使用 WebRTC 共享 你计算机的终端会话。您可以在不设置代理服务器,调试 NAT 后面的服务器等的情况下，与朋友配对。

- WebTTY 也可以在浏览器中使用, 通过此静态页面:<https://maxmcd.github.io/webtty/>试试连接到 WebTTY 会话。

### 状态

有一些 bug 需要修复,但目前一切都运行良好。如果您发现错误,请打开一个问题.

### 目录

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [安装](#%E5%AE%89%E8%A3%85)
- [运行](#%E8%BF%90%E8%A1%8C)
  - [主机](#%E4%B8%BB%E6%9C%BA)
  - [客户端](#%E5%AE%A2%E6%88%B7%E7%AB%AF)
- [终端尺寸](#%E7%BB%88%E7%AB%AF%E5%B0%BA%E5%AF%B8)
- [单向连接](#%E5%8D%95%E5%90%91%E8%BF%9E%E6%8E%A5)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### 安装

WebTTY 要求使用 Go 1.9 或更高版本.

```bash
go get -u github.com/maxmcd/webtty
```

WebTTY 使用了很棒的[pions/webrtc](https://github.com/pions/webrtc)，于 WebRTC 通信。它目前需要 OpenSSL 来构建。更多信息:<https://github.com/pions/webrtc#install>

### 运行

```shell
> webtty -h
Usage of webtty:
  -cmd
        运行的 command . 默认 是 "bash -l"
        因为此标志会占用命令行的其余部分，
        所有其他命令后参数选项 (若是存在) 必须出现在此标志之前.
        eg: webtty -o -v -ni -cmd docker run -it --rm alpine:latest sh
  -ni
        设 host 为 非交互
  -non-interactive
        设 host 为 非交互
  -o    不需要 响应的单向链接
  -s string
        使用的 stun 服务器 (默认 "stun:stun.l.google.com:19302")
  -v    Verbose logging
```

#### 主机

```shell
> webtty
Setting up a WebTTY connection.

Connection ready. Here is your connection data:

25FrtDEjh7yuGdWMk7R9PhzPmphst7FdsotL11iXa4r9xyTM4koAauQYivKViWYBskf8habEc5vHf3DZge5VivuAT79uSCvzc6aL2M11kcUn9rzb4DX4...

Paste it in the terminal after the webtty command
Or in a browser: https://maxmcd.github.io/webtty/

When you have the answer, paste it below and hit enter.
```

#### 客户端

```shell
> webtty 25FrtDEjh7yuGdWMk7R9PhzPmphst7FdsotL11iXa4r9xyTM4koAauQYivKViWYBskf8habEc5vHf3DZge5VivuAT79uSCvzc6aL2M11kcUn9rzb4DX4...
```

### 终端尺寸

默认情况下,WebTTY 会强制锁定客户端的大小。这意味着主机大小经常会错误地呈现。解决这个问题的一种方法是使用 tmux:

```bash
tmux new-session -s shared
# 其他终端
webtty -ni -cmd tmux attach-session -t shared
```

现在 Tmux，将会话大小调整为最小的终端视口.

### 单向连接

可以使用`-o`选项启用单向连接。典型的 webrtc 连接需要双方的 SDP 交换。默认情况下,WebTTY 将创建 SDP '请求'，并等待您输入 SDP 回应。`-o`标记随着初始'请求'一起发送到公共 URL，接收者应会 post 他们的回复。这用到了我的服务器[10kb.site](https://www.10kb.site)。然后主机会不断轮询网址，直到得到回应.

我认为这有点违反了这个工具的精神,因为它依赖于第三方服务。但是,单向连接可以让你做很酷的事情。例如:我可以在构建服务器出错时，输出一个 WebTTY 连接字符串,并允许任何人连接到会话。

上传时，会加密 SDP 描述,并在连接上共享加密密钥，让数据能被解密。因此,想必被破坏的服务也不成问题.

对于如何启用可信单向连接的任何想法都非常欢迎。如果您有任何想法,请打开一个问题或联系我们。就目前而言，`-o`选项会打印警告，并链接到此说明。
