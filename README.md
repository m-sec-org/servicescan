# EZ ServiceScan - 强大且 niubility 的网络服务扫描工具

## 概述

ServiceScan 是一款高效且灵活的网络服务探测工具，旨在快速识别目标网络中的开放端口及服务。它支持多种扫描技术、灵活的参数配置以及被动 API 扫描，能够满足安全测试人员、系统管理员以及网络爱好者在不同场景下的需求。

ServiceScan 专注于快速准确地发现网络中暴露的服务，并提供丰富的扫描选项和结果输出，帮助用户全面了解目标网络的开放服务情况。

ServiceScan 和 EZ 共用同一套 license 授权验证机制，用户可注册 M-SEC 社区（[https://msec.nsfocus.com/）后在个人中心申请证书，证书下载后放至 ServiceScan 工具同目录即可启动使用。](https://msec.nsfocus.com/%EF%BC%89%E5%90%8E%E5%9C%A8%E4%B8%AA%E4%BA%BA%E4%B8%AD%E5%BF%83%E7%94%B3%E8%AF%B7%E8%AF%81%E4%B9%A6%EF%BC%8C%E8%AF%81%E4%B9%A6%E4%B8%8B%E8%BD%BD%E5%90%8E%E6%94%BE%E8%87%B3ServiceScan%E5%B7%A5%E5%85%B7%E5%90%8C%E7%9B%AE%E5%BD%95%E5%8D%B3%E5%8F%AF%E5%90%AF%E5%8A%A8%E4%BD%BF%E7%94%A8%E3%80%82)

功能特点

-   灵活的目标指定：支持 IP 地址、IP 范围 (e.g., 192.168.1.1-10)、CIDR 网络 (e.g., 192.168.1.0/24) 以及从文件批量导入目标 IP 列表。
-   全面的端口扫描：支持自定义扫描端口，预置 top100、top1000 常用端口，以及 all 全端口扫描。允许从文件导入自定义端口列表。
-   **API 和 MCP Server 功能暂不对个人用户开放，企业可关注 MSEC 运营号申请体验。**

    -   **支持 API 模式：支持通过 HTTP API 创建和管理扫描任务，方便用户在自动化流程中集成使用。**

    -   **支持 MCP 模式：通过标准化的模型上下文协议（Model Context Protocol）实现与大语言模型的深度集成，支持 stdio、SSE 通信模式，为 AI 工具提供统一的接口层，实现扫描任务的创建、暂停、恢复和结果查询等功能的智能交互。**

-   **暂停/恢复扫描：支持暂停/恢复扫描，方便用户在扫描过程中暂停扫描，恢复扫描，或者在扫描过程中暂停扫描，恢复扫描。**
-   多种扫描模式：
    -   主机发现 (Host Discovery)：支持 icmp,icmp6, arp, ndp, tcp, tcpsyn, ping, oxid, netbios 多种主机发现类型，支持内网场景下快速识别存活主机。
    -   端口扫描 (Port Scan)：支持 tcpconn, tcphalf, udp 三种端口扫描类型，满足不同网络环境的需求。
    -   被动 API 探测 (Passive API Scan)：集成被动 API 探测资产功能，结合多种在线服务接口，辅助发现开放服务。
-   服务指纹识别 (Service Fingerprint)：内置服务指纹探测功能，能够识别常见服务的版本信息，并支持用户自定义指纹规则目录。
-   服务指纹自定义：支持 yaml 标准格式指纹插件，专业人员可开发插件扩展服务指纹探测能力。
-   结果输出：支持多种输出格式 CSV、JSON、TEXT，方便后续分析和处理。提供详细输出模式，展示更全面的扫描信息。
-   日志记录：提供多种日志级别 (debug, info, warning, error, success, disable)，方便问题排查和监控。

## 使用方法

ServiceScan 通过命令行进行操作，提供了丰富的选项来定制扫描行为。

命令行选项

```
NAME:
   servicescan

USAGE:
   servicescan [全局选项] [命令 [命令选项]]

VERSION:
   1.3

COMMANDS:
   gen      生成被动API配置
   debug    显示调试信息
   api      启动API服务器
   mcp      启动MCP服务器
   help, h  显示命令列表或某个命令的帮助信息

GLOBAL OPTIONS:
   --help, -h     显示帮助信息
   --version, -v  打印版本信息

   1. 目标设置

   --ip-ports value, -i value        IP:端口对 (例如: 192.168.1.1:80)
   --ipport-file value, --if value   包含ip:端口对的文件
   --ports value, -p value           要扫描的端口 (例如: 80,443,80-90,all,top100) (默认: "top100")
   --ports-file value, --pf value    包含端口的文件
   --targets value, -t value         目标IP (例如: 192.168.1.1,192.168.1.1-10,192.168.1.0/24)
   --targets-file value, --tf value  包含目标IP的文件

   2. 输出设置

   --output-db value, --ob value    输出数据库文件
   --output-host value, --oh value   主机扫描输出文件 (.csv, .json, .txt) (默认: "servicescan_host.csv")
   --output-port value, --op value   端口扫描输出文件 (.csv, .json, .txt) (默认: "servicescan_port.csv")
   --outputx, --ox                   启用详细输出模式 (默认: false)

   3. 行为设置

   --only-discovery, --od  仅执行主机发现 (默认: false)
   --passive-api, --ps     启用被动API扫描 (默认: false)
   --port-scan, --pn       启用端口扫描。设置 -port-scan=false 禁用端口扫描 (默认: true)
   --progress, --pg        显示同步扫描模式进度条 (默认: true)
   --skip-discovery, --nd  跳过主机发现 (默认: false)
   --svc-finger, --sv      启用服务探测。设置 -svc-finger=false 禁用服务探测 (默认: true)
   --use-state, --us       使用状态文件恢复扫描 (默认: true)

   高级主机发现配置

   --discovery-tcp-port value           TCP扫描模式的默认端口 (默认: 80)
   --discovery-timeout value            发现超时时间 (默认: 2s)
   --discovery-type value, --dt value   发现扫描类型 (icmp, arp, ndp, tcp, tcpsyn, ping, oxid, netbios) (默认: icmp)
   --discovery-retry value              发现重试次数 (默认: 2)

   高级被动API配置

   --passive-timeout value  被动API超时时间 (默认: 5s)

   高级端口扫描器配置

   --portscan-retry value             端口扫描重试次数 (默认: 3)
   --portscan-tcp-flag value          TCP半开扫描标志 (syn, ack, fin, null, xmas, window) (默认: syn)
   --portscan-timeout value           端口扫描超时时间 (默认: 2s)
   --portscan-type value, --pt value  端口扫描类型 (tcpconn, tcphalf, udp) (默认: tcphalf)

   高级服务探测配置

   --svc-conn-timeout value  服务探测连接超时时间 (默认: 3s)
   --svc-fullprobe, --fp     向目标发送所有探测 (默认: false)
   --svc-noneprobe, --np     仅向目标发送被动探测 (默认: false)
   --svc-read-timeout value  服务探测读取超时时间 (默认: 3s)
   --svc-retry value         服务探测重试次数 (默认: 1)

   全局配置

   --lic value                      EZ许可证文件 (默认: "ez.lic")
   --log-level value                日志级别 (debug, info, warning, error, success, disable) (默认: "info")
   --network-iface value, -n value  原始套接字网络接口 (default,auto,ifaceName) default: 使用默认接口, auto: 根据目标IP自动选择接口, 或指定接口 (默认: "default")
   --passive-config value           被动API配置文件 (默认: "passive_config.yaml")
   --passive-proxy value            被动API代理地址 (例如: http://127.0.0.1:7879)
   --passive-ua value               被动API用户代理
   --scan-rate value                主机发现和端口扫描的扫描速率 (默认: 500)
   --svc-parallel value             服务探测并行度 (默认: 200)
   --svc-ruler value                服务探测用户自定义规则目录 (默认: "rules")
```

## 使用示例

1. 基本扫描: 扫描 192.168.1.1 的 top100 常用端口，默认使用 TCP SYN 扫描，并将结果保存到默认文件 servicescan_result.csv。  
   `servicescan -t 192.168.1.1`
2. 扫描 IP 范围: 扫描 192.168.1.1 到 192.168.1.10 的 80,443,8080 端口。  
   `servicescan -t 192.168.1.1-10 -p 80,443,8080`
3. 扫描 CIDR 网络: 扫描 192.168.1.0/24 网段的 top1000 端口，并将结果保存到 my_scan_result.csv。  
   `servicescan -t 192.168.1.0/24 -p top1000 -op my_scan_result.csv`
4. 扫描 IP:Port  
   `servicescan -i 192.168.1.1:80`
   `servicescan -i 192.168.1.1:2152 -pt udp`
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
13. 指定扫描使用的网卡
    `servicescan -t 192.168.4.1 -n eth0`
    `servicescan -t 192.168.4.1 -n auto` auto 表示根据目标 IP 自动路由选择，主要针对多网卡环境扫描

## API 配置

[API 接口文档](https://apifox.com/apidoc/shared-359830e7-2e3b-4c5b-b1fa-311ac94016cc)

```cmd
NAME:
   servicescan api - 启动API服务器

USAGE:
   servicescan api [命令 [命令选项]]

OPTIONS:
   --listen value, -l value   监听地址 (默认: "127.0.0.1:8080")
   --workers value, -w value  工作进程数量 (默认: 1)
   --secret value, -s value   密钥 (默认: "gYS9nmYotWGNxcaw")
   --help, -h                 显示帮助信息

GLOBAL OPTIONS:
   --svc-ruler value                服务探测用户自定义规则目录 (默认: "rules")
   --svc-parallel value             服务探测并行度 (默认: 200)
   --passive-proxy value            被动API代理地址 (例如: http://127.0.0.1:7879)
   --passive-ua value               被动API用户代理
   --passive-config value           被动API配置文件 (默认: "passive_config.yaml")
   --network-iface value, -n value  原始套接字网络接口 (default,auto,ifaceName) default: 使用默认接口, auto: 根据目标IP自动选择接口, 或指定接口 (默认: "default")
   --scan-rate value                主机发现和端口扫描的扫描速率 (默认: 500)
   --log-level value                日志级别 (debug, info, warning, error, success, disable) (默认: "info")
   --lic value                      EZ许可证文件 (默认: "ez.lic")
```

## MCP 配置

```cmd
NAME:
   servicescan mcp - 启动MCP服务器

USAGE:
   servicescan mcp [命令 [命令选项]]

OPTIONS:
   --mode value, -m value     MCP服务器模式 (stdio,sse) (默认: sse)
   --listen value, -l value   SSE监听地址 (默认: "127.0.0.1:3000")
   --workers value, -w value  工作进程数量 (默认: 1)
   --help, -h                 显示帮助信息

GLOBAL OPTIONS:
   --svc-ruler value                服务探测用户自定义规则目录 (默认: "rules")
   --svc-parallel value             服务探测并行度 (默认: 200)
   --passive-proxy value            被动API代理地址 (例如: http://127.0.0.1:7879)
   --passive-ua value               被动API用户代理
   --passive-config value           被动API配置文件 (默认: "passive_config.yaml")
   --network-iface value, -n value  原始套接字网络接口 (default,auto,ifaceName) default: 使用默认接口, auto: 根据目标IP自动选择接口, 或指定接口 (默认: "default")
   --scan-rate value                主机发现和端口扫描的扫描速率 (默认: 500)
   --log-level value                日志级别 (debug, info, warning, error, success, disable) (默认: "info")
   --lic value                      EZ许可证文件 (默认: "ez.lic")
```

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

-   TCP_NULL: 发送空数据包，等待被动回应。
-   TCP_GetRequest: 发送 GET / HTTP/1.0\r\n\r\n
-   TCP_HTTPOptions: 发送 OPTIONS / HTTP/1.0\r\n\r\n
-   TCP_GenericLines: 发送 \r\n\r\n
-   TCP_RTSPRequest: 发送 OPTIONS / RTSP/1.0\r\n\r\n
-   ...敬请期待更多内置数据包

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

-   运行 ServiceScan 可能需要 root 或管理员权限，尤其是在使用某些主机发现和端口扫描类型时 (例如 icmp, arp, tcphalf)。
-   windows 环境若使用 TCP SYN(默认)扫描，请先下载 npcap。
-   linux 环境若使用 TCP SYN(默认)扫描，请先安装 [libpcap-dev]（apt-get install libpcap-dev）。
-   Linux/Mac 下的扫描速度会比 Windows 更快，更好的体验请使用 Linux/Mac。
-   passive-config.yaml 文件用于配置被动 API 扫描的相关参数，例如 API 密钥等。需要使用`servicescan gen` 生成配置文件
-   使用被动 API 扫描时，可能需要配置代理 (--passive-proxy) 以提高访问速度和成功率。
-   详细输出模式 (--outputx) 会产生更详细的扫描结果，文件体积也会相对较大。

## 更新日志

## V1.3 版本更新 new

### 新增功能

-   支持扫描任务的暂停和恢复，提供灵活的任务控制
-   提供 API 服务接口，支持通过 API 创建和管理扫描任务
-   适配 MCP 模式，对接大模型，方便 AI 集成本工具
-   支持自定义扫描网卡配置，默认使用系统默认网卡
-   新增 Sqlite 输出格式，支持输出到 sqlite 数据库
-   扩展输出新增 PortBanner 字段，支持输出端口 Banner

### 问题修复

-   修复 macOS IPv6 扫描问题

## V1.2 版本更新

### 新增功能

-   **IPv6 端口扫描支持**（实验性功能）

    -   Linux 及 Windows 平台完全支持
    -   macOS 平台限制：仅支持 TCP 全连接扫描模式（需使用`-pt tcpconn`参数）

-   **扩展输入方式**

    -   新增`-i/--ip-ports`参数支持直接指定"IP:Port"格式的扫描目标

-   **多格式输出支持**

    -   新增 JSON、TXT 输出格式
    -   智能识别：根据输出文件后缀名（.csv/.json/.txt）自动选择输出格式

-   **新增指纹**

    -   新增 UDP GTPv0,GTPv0,GTPv2 服务指纹

### 问题修复与优化

-   **修复 UDP 扫描功能**，解决之前无法正常使用的问题

-   **架构优化**

    -   移除异步扫描机制
    -   提升扫描结果一致性和可靠性
    -   优化整体扫描性能

## V1.1 版本更新

### 性能优化

-   重构扫描进度展示系统

    -   同步模式下使用精确进度条
    -   异步模式下采用实时状态展示

-   显著降低内存占用，提升扫描稳定性

-   优化权限适配机制

    -   无 Raw Socket 权限时自动降级为 TCP Connect 扫描
    -   保障工具在低权限环境下依然可用

### 输出优化

-   改进文件输出机制

    -   实现实时写入，避免内存积压
    -   分离主机与端口扫描结果
    -   优化 CSV 输出格式，提升数据可读性

### 架构优化

-   重构结果存储模块

    -   主机发现结果独立存储
    -   端口扫描结果独立存储

-   优化内存管理策略

## 下个版本计划功能

-   优化扫描性能
