# EZ ServiceScan - 强大且 niubility 的网络服务扫描工具

## 概述

ServiceScan 是一款高效且灵活的网络服务探测工具，旨在快速识别目标网络中的开放端口及服务。它支持多种扫描技术、灵活的参数配置以及被动 API 扫描，能够满足安全测试人员、系统管理员以及网络爱好者在不同场景下的需求。

ServiceScan 专注于快速准确地发现网络中暴露的服务，并提供丰富的扫描选项和结果输出，帮助用户全面了解目标网络的开放服务情况。

ServiceScan 和 EZ 共用同一套 license 授权验证机制，用户可注册 M-SEC 社区（[https://msec.nsfocus.com/）后在个人中心申请证书，证书下载后放至 ServiceScan 工具同目录即可启动使用。](https://msec.nsfocus.com/%EF%BC%89%E5%90%8E%E5%9C%A8%E4%B8%AA%E4%BA%BA%E4%B8%AD%E5%BF%83%E7%94%B3%E8%AF%B7%E8%AF%81%E4%B9%A6%EF%BC%8C%E8%AF%81%E4%B9%A6%E4%B8%8B%E8%BD%BD%E5%90%8E%E6%94%BE%E8%87%B3ServiceScan%E5%B7%A5%E5%85%B7%E5%90%8C%E7%9B%AE%E5%BD%95%E5%8D%B3%E5%8F%AF%E5%90%AF%E5%8A%A8%E4%BD%BF%E7%94%A8%E3%80%82)

功能特点

- 灵活的目标指定：支持 IP 地址、IP 范围 (e.g., 192.168.1.1-10)、CIDR 网络 (e.g., 192.168.1.0/24) 以及从文件批量导入目标 IP 列表。
- 全面的端口扫描：支持自定义扫描端口，预置 top100、top1000 常用端口，以及 all 全端口扫描。允许从文件导入自定义端口列表。
- 多种扫描模式：

  - 主机发现 (Host Discovery)：支持 icmp,icmp6, arp, ndp, tcp, tcpsyn, ping, oxid, netbios 多种主机发现类型，支持内网场景下快速识别存活主机。
  - 端口扫描 (Port Scan)：支持 tcpconn, tcphalf, udp 三种端口扫描类型，满足不同网络环境的需求。

- 服务指纹识别 (Service Fingerprint)：内置服务指纹探测功能，能够识别常见服务的版本信息，并支持用户自定义指纹规则目录。
- 服务指纹自定义：支持 yaml 标准格式指纹插件，专业人员可开发插件扩展服务指纹探测能力。
- 被动 API 探测 (Passive API Scan)：集成被动 API 探测资产功能，结合多种在线服务接口，辅助发现开放服务。
- 性能优化：

  - 扫描速度预设：提供 normal, fast, slow 三种扫描速度预设，方便用户根据需求调整扫描速度。
  - QPS 限制：支持精细化的 QPS (Queries Per Second) 限制，防止扫描行为对目标系统造成过大压力。
  - 超时和重试机制：可配置超时时间和重试次数，增强扫描的稳定性和准确性。

- 结果输出：支持多种输出格式 CSV、JSON、TEXT，方便后续分析和处理。提供详细输出模式，展示更全面的扫描信息。
- 日志记录：提供多种日志级别 (debug, info, warning, error, success, disable)，方便问题排查和监控。

## 使用方法

ServiceScan 通过命令行进行操作，提供了丰富的选项来定制扫描行为。

命令行选项

````
NAME:
   servicescan - servicescan tool by ezreal team

USAGE:
   servicescan [options]

GLOBAL OPTIONS:
   --targets value, -t value           扫描目标IP (例如 192.168.1.1,192.168.1.1-10,192.168.1.0/24,2001:4860:4860::8888)
   --ports value, -p value             要扫描的端口 (例如 80,443,80-90,all,top100,top1000,top100u,top1000u) (默认值: "top100")
   --ip-ports value, -i value          指定IP和端口进行扫描 (例如 192.168.1.1:80,2001:4860:4860::8888:443)
   --targets-file value, --tf value    包含IP列表的文件，每行一个或使用逗号分隔
   --ports-file value, --pf value      包含端口列表的文件，每行一个或使用逗号分隔
   --ipport-file value, --if value     包含ip:port格式的文件，每行一个或使用逗号分隔
   --output-port value, --op value     将端口扫描结果输出到文件。格式由文件扩展名决定(.csv, .json, .txt)。设置为'false'禁用输出 (默认值: "servicescan_port.csv")
   --output-host value, --oh value     将主机扫描结果输出到文件。格式由文件扩展名决定(.csv, .json, .txt)。设置为'false'禁用输出 (默认值: "servicescan_host.csv")
   --outputx, --ox                     启用详细输出模式 (默认值: false)
   --only-discovery, --od              仅执行主机发现 (默认值: false)
   --skip-discovery, --nd              跳过主机发现 (默认值: false)
   --port-scan, --pn                   启用端口扫描。设置-port-scan=false禁用端口扫描 (默认值: true)
   --svc-finger, --sv                  启用服务探测。设置-svc-finger=false禁用服务探测 (默认值: true)
   --passive-api, --ps                 启用被动API扫描 (默认值: false)
   --speed value, --sp value           扫描速度预设 (normal普通, fast快速, slow慢速) (默认值: normal)
   --progress, --pg                    显示同步扫描模式进度条 (默认值: true)
   --discovery-type value, --dt value  主机发现扫描类型 (icmp, arp, ndp, tcp, tcpsyn, ping, oxid, netbios) (默认值: icmp)
   --discovery-tcp-port value          TCP扫描模式的默认端口 (默认值: 80)
   --portscan-type value, --pt value   端口扫描类型 (tcpconn全连接, tcphalf半连接, udp) (默认值: tcphalf)
   --portscan-tcp-flag value           TCP半连接扫描标志 (syn, ack, fin, null, xmas, window) (默认值: syn)
   --svc-fullprobe, --fp               向目标发送所有探测 (默认值: false)
   --svc-ruler value                   服务探测用户自定义规则目录 (默认值: "rulers")
   --svc-response-size value           服务探测响应大小限制 (默认值: 10240)
   --passive-proxy value               被动API代理地址 (例如 http://127.0.0.1:7879)
   --passive-config value              被动API配置文件 (默认值: "passive-config.yaml")
   --passive-ua value                  被动API用户代理
   --discovery-qps value               主机发现QPS(每秒查询数)限制 (默认值: 500)
   --portscan-qps value                端口扫描QPS(每秒查询数)限制 (默认值: 500)
   --svc-qps value                     服务探测QPS(每秒查询数)限制 (默认值: 300)
   --passive-qps value                 被动API QPS(每秒查询数)限制 (默认值: 20)
   --discovery-retry value             主机发现重试次数 (默认值: 3)
   --portscan-retry value              端口扫描重试次数 (默认值: 3)
   --discovery-timeout value           主机发现超时时间 (默认值: 2s)
   --portscan-timeout value            端口扫描超时时间 (默认值: 2s)
   --svc-read-timeout value            服务探测读取超时时间 (默认值: 3s)
   --svc-dial-timeout value            服务探测连接超时时间 (默认值: 3s)
   --log-level value                   日志级别 (debug调试, info信息, warning警告, error错误, success成功, disable禁用) (默认值: "info")
   --lic value                         ez许可证文件 (默认值: "ez.lic")
   --help, -h                          显示帮助
````

## 使用示例

1. 基本扫描: 扫描 192.168.1.1 的 top100 常用端口，默认使用 TCP SYN 扫描，并将结果保存到默认文件 servicescan_result.csv。  
   `servicescan -t 192.168.1.1`
2. 扫描 IP 范围: 扫描 192.168.1.1 到 192.168.1.10 的 80,443,8080 端口。  
   `servicescan -t 192.168.1.1-10 -p 80,443,8080`
3. 扫描 CIDR 网络: 扫描 192.168.1.0/24 网段的 top1000 端口，并将结果保存到 my_scan_result.csv。  
   `servicescan -t 192.168.1.0/24 -p top1000 -op my_scan_result.csv`
4. 扫描 IP:Port  
   `servicescan -i 192.168.1.1:80`
5. 使用 TCP Conn 扫描: 对 192.168.2.100 进行 TCP Conn 扫描，端口为 1-1000。  
   `servicescan -t 192.168.2.100 -p 1-1000 -pt tcpconn`
6. 仅进行主机发现: 使用 ARP 方式发现 192.168.3.0/24 网段的存活主机。  
   `servicescan -t 192.168.3.0/24 -dt arp -od`
7. 禁用服务指纹识别: 扫描 172.16.0.1 的 top100 端口，不进行服务识别  
   `servicescan -t 192.168.1.1 -sv=false`
8. 使用被动 API 扫描: 扫描 172.16.0.1 的 80,443 端口，并启用被动 API 扫描  
   `servicescan -t 172.16.0.1 -p 80,443 --passive-api`
9. 调整扫描速度为快速: 扫描 10.10.10.1 的 top100 端口，并设置为快速扫描模式。  
   `servicescan -t 10.10.10.1 -p top100 -sp fast`
10. 从文件读取目标和端口: 从 targets.txt 文件读取目标 IP 列表，从 ports.txt 文件读取端口列表进行扫描。  
      `servicescan -tf targets.txt -pf ports.txt -op batch_scan_result.csv`
11. 详细输出模式: 扫描 192.168.4.1 的 80,443 端口，并启用详细输出模式。  
      `servicescan -t 192.168.4.1 -p 80,443 -ox -op detail_result.csv`
12. 取消输出文件：  
      `servicescan -t 192.168.4.1 -op false -oh false`

## 自定义指纹规则

可以在程序下创建一个 `rules` 文件夹，把编写⾃定义的指纹⽂件放到里面会自动读取

```text
.
├── rulers
│   ├── mqtt_mosquitto.yaml
│   ├── pure_ftpd.yaml
│   └── openssh.yaml
├── servicescan
```

自定义指纹规则格式：

```yaml
probe_name: TCP_MQTT # TCP 数据包名称，以 'TCP_' 开头
probe_data: "101c00064d51497364700302003c000e6d717474785f35366131613137308218868b0013245359532f62726f6b65722f76657273696f6e00" # ⼗六进制数据
product: mosquitto # 产品名称
protocol: mqtt # 协议名称
ports: 1883 # 对应的端⼝（可选）
sslPorts: 8883 # SSL 端⼝（可选）
rules:
  - action: "find" # 指纹识别动作
    type: "regex" # 可选：regex、search、match
    pattern: 'broker/versionmosquitto version ([0-9]+\.[0-9]+\.[0-9]+)' # 匹配规则
  - action: "version" # 提取版本号（可选）
  	group: 1 # 正则表达式匹配组号
```

使⽤内置 TCP 数据包的指纹:

```yaml
probe_name: TCP_NULL # 内置的 TCP 数据包，也就是发送空数据包，等待被动回应。
product: Pure-FTPd
protocol: ftp
rules:
    - action: "find"
      type: "search"
      pattern: "Pure-FTPd"
```

```yaml
probe_name: TCP_NULL
product: OpenSSH
protocol: ssh
rules:
    - action: "find"
      type: "regex"
      pattern: "OpenSSH_(\\d+\\.\\d+(p\\d+)?)"
    - action: "version"
      type: "regex"
      group: 1
```

内置的 TCP 数据包有：

- TCP_NULL: 发送空数据包，等待被动回应。
- TCP_GetRequest: 发送 GET / HTTP/1.0\r\n\r\n
- TCP_HTTPOptions: 发送 OPTIONS / HTTP/1.0\r\n\r\n
- TCP_GenericLines: 发送 \r\n\r\n
- TCP_RTSPRequest: 发送 OPTIONS / RTSP/1.0\r\n\r\n
- ...敬请期待更多内置数据包

### 服务指纹库

**服务指纹总数：11960 条**

| 服务名称      | 数量 | 服务名称     | 数量 | 服务名称         | 数量 | 服务名称     | 数量 | 服务名称          | 数量 | 服务名称      | 数量 |
| ------------- | ---- | ------------ | ---- | ---------------- | ---- | ------------ | ---- | ----------------- | ---- | ------------- | ---- |
| http          | 4405 | telnet       | 1085 | ftp              | 771  | smtp         | 481  | http-proxy        | 334  | 达梦 DB       | 1    |
| pop3          | 262  | upnp         | 250  | postgresql       | 248  | ssh          | 237  | imap              | 171  | winRM         | 1    |
| sip           | 119  | rtsp         | 110  | domain           | 110  | smtp-proxy   | 88   | ms-sql-s          | 75   | aliyunftp     | 1    |
| irc           | 59   | afp          | 59   | finger           | 56   | nntp         | 52   | sip-proxy         | 51   | feishuftpd    | 1    |
| X11           | 47   | pop3-proxy   | 43   | ftp-proxy        | 42   | ident        | 41   | printer           | 40   | indyftp       | 1    |
| ssl           | 36   | vnc-http     | 36   | microsoft-ds     | 34   | jabber       | 31   | netbios-ns        | 31   | xlightftp     | 1    |
| backdoor      | 29   | login        | 28   | ipp              | 26   | bitcoin      | 25   | ldap              | 25   | sparkweb      | 1    |
| mysql         | 25   | snmp         | 24   | vnc              | 22   | icy          | 22   | daytime           | 20   | etcd          | 1    |
| netbios-ssn   | 20   | pop3pw       | 20   | websocket        | 16   | imap-proxy   | 15   | irc-proxy         | 15   | mqtt          | 2    |
| sieve         | 15   | textui       | 15   | modbus           | 15   | drawpile     | 14   | gopher            | 14   | hiveServer    | 1    |
| am-pdp        | 14   | gnutella     | 14   | tftp             | 14   | socks5       | 14   | donkey            | 12   | ElasitcSearch | 1    |
| ms-wbt-server | 12   | bindshell    | 12   | stomp            | 12   | crestron-ctp | 11   | lmtp              | 11   | Impala        | 1    |
| klogin        | 11   | minecraft    | 11   | remoting         | 11   | mongodb      | 11   | quake3            | 11   | nacos         | 1    |
| chargen       | 10   | cvspserver   | 10   | java-object      | 10   | shell        | 10   | caldav            | 10   | redis         | 6    |
| daap          | 10   | soap         | 10   | ntp              | 10   | radmin       | 10   | adobe-crossdomain | 9    | MariaDB       | 3    |
| giop          | 9    | iscsi        | 9    | openvpn          | 9    | webdav       | 9    | landesk-rc        | 9    | ...           | ...  |
| honeypot      | 8    | boinc        | 8    | cassandra-native | 8    | whois        | 8    | docker            | 8    | ...           | ...  |
| kshell        | 8    | webmin       | 8    | apachemq         | 7    | exec         | 7    | ndmp              | 7    | ...           | ...  |
| saprouter     | 7    | telnet-proxy | 7    | warcraft         | 7    | memcached    | 7    | uucp              | 7    | ...           | ...  |
| ntop-http     | 7    | ssl/http     | 7    | dtls             | 7    | echo         | 7    | font-service      | 7    | ...           | ...  |

## 注意事项

- 运行 ServiceScan 可能需要 root 或管理员权限，尤其是在使用某些主机发现和端口扫描类型时 (例如 icmp, arp, tcphalf)。
- windows 环境若使用 TCP SYN(默认)扫描，请先下载 npcap。
- linux 环境若使用 TCP SYN(默认)扫描，请先安装 [libpcap-dev]（apt-get install libpcap-dev）。
- Linux/Mac 下的扫描速度会比 Windows 更快，更好的体验请使用 Linux/Mac。
- passive-config.yaml 文件用于配置被动 API 扫描的相关参数，例如 API 密钥等。首次运行可能会自动生成该文件。
- 合理配置扫描速度 (--speed) 和 QPS 限制 (--discovery-qps, --portscan-qps, --svc-qps, --passive-qps)，避免对目标系统造成拒绝服务 (DoS) 风险。
- 使用被动 API 扫描时，可能需要配置代理 (--passive-proxy) 以提高访问速度和成功率。
- 详细输出模式 (--outputx) 会产生更详细的扫描结果，文件体积也会相对较大。

## 更新日志

## V1.2 版本更新 new

### 新增功能

- **IPv6 端口扫描支持**（实验性功能）

  - Linux 及 Windows 平台完全支持
  - macOS 平台限制：仅支持 TCP 全连接扫描模式（需使用`-pt tcpconn`参数）
- **扩展输入方式**
	- 新增`-i/--ip-ports`参数支持直接指定"IP:Port"格式的扫描目标

- **多格式输出支持**
	- 新增 JSON、TXT 输出格式
	- 智能识别：根据输出文件后缀名（.csv/.json/.txt）自动选择输出格式

- **新增指纹**
	- 新增 UDP GTPv0,GTPv0,GTPv2服务指纹


### 问题修复与优化

- **修复 UDP 扫描功能**，解决之前无法正常使用的问题
- **架构优化**

  - 移除异步扫描机制
  - 提升扫描结果一致性和可靠性
  - 优化整体扫描性能


## V1.1 版本更新

### 性能优化

- 重构扫描进度展示系统

  - 同步模式下使用精确进度条
  - 异步模式下采用实时状态展示

- 显著降低内存占用，提升扫描稳定性
- 优化权限适配机制

  - 无 Raw Socket 权限时自动降级为 TCP Connect 扫描
  - 保障工具在低权限环境下依然可用


### 输出优化

- 改进文件输出机制

  - 实现实时写入，避免内存积压
  - 分离主机与端口扫描结果
  - 优化 CSV 输出格式，提升数据可读性


### 架构优化

- 重构结果存储模块

  - 主机发现结果独立存储
  - 端口扫描结果独立存储

- 优化内存管理策略

## 下个版本计划功能

- 完善 macOS 平台 IPv6 扫描。
- 优化扫描速度
