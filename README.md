# EZ ServiceScan - 强大且niubility的网络服务扫描工具

## 概述

ServiceScan 是一款高效且灵活的网络服务探测工具，旨在快速识别目标网络中的开放端口及服务。它支持多种扫描技术、灵活的参数配置以及被动 API 扫描，能够满足安全测试人员、系统管理员以及网络爱好者在不同场景下的需求。

ServiceScan  专注于快速准确地发现网络中暴露的服务，并提供丰富的扫描选项和结果输出，帮助用户全面了解目标网络的开放服务情况。

## 功能特点

- **灵活的目标指定**：支持 IP 地址、IP 范围 (e.g., `192.168.1.1-10`)、CIDR 网络 (e.g., `192.168.1.0/24`) 以及从文件批量导入目标 IP 列表。
- **全面的端口扫描**：支持自定义扫描端口，预置 `top100`、`top1000` 常用端口，以及 `all` 全端口扫描。允许从文件导入自定义端口列表。
- **多种扫描模式**：
  - **主机发现 (Host Discovery)**：支持 `icmp`, `arp`, `ndp`, `tcp`, `tcpsyn`, `ping`, `oxid`, `netbios` 多种主机发现类型，支持内网场景下快速识别存活主机。
  - **端口扫描 (Port Scan)**：支持 `tcpconn`, `tcphalf`, `udp` 三种端口扫描类型，满足不同网络环境的需求。
  - **异步扫描 (Async Scan)**：支持异步扫描模式，显著提升扫描效率。
- **服务指纹识别 (Service Fingerprint)**：内置服务指纹探测功能，能够识别常见服务的版本信息，并支持用户自定义指纹规则目录。
- **服务指纹自定义**：支持yaml标准格式指纹插件，专业人员可开发插件扩展服务指纹探测能力。
- **被动 API 探测 (Passive API Scan)**：集成被动 API 探测资产功能，结合多种在线服务接口，辅助发现开放服务。
- **性能优化**：
  - **扫描速度预设**：提供 `normal`, `fast`, `slow` 三种扫描速度预设，方便用户根据需求调整扫描速度。
  - **QPS 限制**：支持精细化的 QPS (Queries Per Second) 限制，防止扫描行为对目标系统造成过大压力。
  - **超时和重试机制**：可配置超时时间和重试次数，增强扫描的稳定性和准确性。
- **结果输出**：支持多种输出格式 `CSV `、`JSON`,`YAML`,`TEXT`，方便后续分析和处理。提供详细输出模式，展示更全面的扫描信息。
- **日志记录**：提供多种日志级别 (`debug`, `info`, `warning`, `error`, `success`, `disable`)，方便问题排查和监控。

## 使用方法

ServiceScan 通过命令行进行操作，提供了丰富的选项来定制扫描行为。

### 命令行选项

```plaintext
NAME:
   servicescan - servicescan tool by ezreal team

USAGE:
   servicescan [options]

GLOBAL OPTIONS:
   --targets value, -t value           目标IP地址，可以使用单个IP、IP范围 (例如：192.168.1.1,192.168.1.1-10,192.168.1.0/24)
   --ports value, -p value             要扫描的端口，例如：80,443,80-90,all,top100,top1000,top100u,top1000u (默认值: "top100")
   --targets-file value, --tf value    包含目标IP地址列表的文件，每行一个IP地址
   --ports-file value, --pf value      包含端口列表的文件，每行一个端口或逗号分隔的端口列表
   --output value, -o value            输出结果摘要文件名，用于保存扫描结果。设置为 "false" 则禁用输出 (默认值: "servicescan_result.csv")
   --outputx, --ox                     启用详细输出模式 (默认值: false)
   --svc-finger, --sv                  启用服务指纹识别。 设置为 `--svc-finger=false` 禁用服务指纹识别 (默认值: true)
   --passive-api, --ps                 启用被动API扫描 (默认值: false)
   --only-discovery, --od              仅执行主机发现 (默认值: false)
   --no-discovery, --nd                跳过主机发现 (默认值: false)
   --speed value, --sp value           扫描速度预设 (normal, fast, slow) (默认值: normal)
   --async, --ac                       启用异步扫描模式 (默认值: false)
   --progress, --pg                    显示同步扫描模式进度条 (默认值: true)
   --discovery-type value, --dt value  主机发现扫描类型 (icmp, arp, ndp, tcp, tcpsyn, ping, oxid, netbios) (默认值: icmp)
   --discovery-tcp-port value          TCP扫描模式的默认端口 (默认值: 80)
   --portscan-type value, --pt value   端口扫描类型 (tcpconn, tcphalf, udp) (默认值: tcphalf)
   --portscan-tcp-flag value           TCP半连接扫描标志 (syn, ack, fin, null, xmas, window) (默认值: syn)
   --svc-fullprobe, --fp               向目标发送所有探测包 (默认值: false)
   --svc-ruler value                   ServiceProter 用户自定义规则目录 (默认值: "rulers")
   --svc-response-size value           ServiceProter 响应大小限制 (默认值: 10240)
   --passive-proxy value               被动API代理地址 (例如：http://127.0.0.1:7879)
   --passive-config value              被动API配置文件 (默认值: "passive-config.yaml")
   --passive-ua value                  被动API User-Agent
   --discovery-qps value               主机发现QPS (每秒查询数) 限制 (默认值: 500)
   --portscan-qps value                端口扫描QPS (每秒查询数) 限制 (默认值: 500)
   --svc-qps value                     ServiceProter QPS (每秒查询数) 限制 (默认值: 150)
   --passive-qps value                 被动API QPS (每秒查询数) 限制 (默认值: 20)
   --discovery-retry value             主机发现重试次数 (默认值: 3)
   --portscan-retry value              端口扫描重试次数 (默认值: 3)
   --discovery-timeout value           主机发现超时时长 (默认值: 2s)
   --portscan-timeout value            端口扫描超时时长 (默认值: 2s)
   --svc-read-timeout value            ServiceProter 读取超时时长 (默认值: 3s)
   --svc-dial-timeout value            ServiceProter 连接超时时长 (默认值: 3s)
   --log-level value                   日志级别 (debug, info, warning, error, success, disable) (默认值: "error")
   --lic value                         ez license 文件 (默认值: "ez.lic")
   --help, -h                          显示帮助信息
```

### 使用示例

1. **基本扫描**: 扫描 `192.168.1.1` 的 `top100` 常用端口，默认使用TCP SYN扫描，并将结果保存到默认文件 `servicescan_result.csv`。

   ```bash
   servicescan -t 192.168.1.1
   ```

2. **扫描 IP 范围**: 扫描 `192.168.1.1` 到 `192.168.1.10` 的 `80,443,8080` 端口。

   ```bash
   servicescan -t 192.168.1.1-10 -p 80,443,8080
   ```

3. **扫描 CIDR 网络**: 扫描 `192.168.1.0/24` 网段的 `top1000` 端口，并将结果保存到 `my_scan_result.csv`。

   ```bash
   servicescan -t 192.168.1.0/24 -p top1000 -o my_scan_result.csv
   ```

4. **使用 TCP Conn扫描**:  对 `192.168.2.100` 进行 TCP Conn 扫描，端口为 `1-1000`。

   ```bash
   servicescan -t 192.168.2.100 -p 1-1000 -pt tcpconn
   ```

5. **仅进行主机发现**:  使用 ARP 方式发现 `192.168.3.0/24` 网段的存活主机。

   ```bash
   servicescan -t 192.168.3.0/24 -dt arp -od
   ```

6. **禁用服务指纹识别**:  扫描 ``172.16.0.1`` 的 `top100` 端口，不进行服务识别

   ```bash
   servicescan -t 192.168.1.1 -sv=false
   ```

7. **使用被动 API 扫描**: 扫描 `172.16.0.1` 的 `80,443` 端口，并启用被动 API 扫描

   ```bash
   servicescan -t 172.16.0.1 -p 80,443 --passive-api
   ```

8. **调整扫描速度为快速**: 扫描 `10.10.10.1` 的 `top100` 端口，并设置为快速扫描模式。

   ```bash
   servicescan -t 10.10.10.1 -p top100 -sp fast
   ```

9. **从文件读取目标和端口**: 从 `targets.txt` 文件读取目标 IP 列表，从 `ports.txt` 文件读取端口列表进行扫描。

   ```bash
   servicescan -tf targets.txt -pf ports.txt -o batch_scan_result.csv
   ```

10. **详细输出模式**: 扫描 `192.168.4.1` 的 `80,443` 端口，并启用详细输出模式。

    ```bash
    servicescan -t 192.168.4.1 -p 80,443 -ox -o detail_result.csv
    ```

11. **异步扫描模式**: 扫描 `192.168.4.1` 的 `top100` 端口，启用异步扫描,提高扫描速度

    ```cmd
     servicescan -t 192.168.4.1 -async
    ```

12. **自定义输出格式**：扫描 `192.168.4.1` 的 `top100` 端口，输出`json`格式内容

    ```cmd
     servicescan -t 192.168.4.1 -o result.json
    ```

13. **取消输出文件**：

    ```cmd
     servicescan -t 192.168.4.1 -o false
    ```

## 注意事项

- 运行 ServiceScan 可能需要 root 或管理员权限，尤其是在使用某些主机发现和端口扫描类型时 (例如 `icmp`, `arp`, `tcphalf`)。
- windows 环境若使用TCP SYN(默认)扫描，请先下载 [npcap](https://npcap.com/)。
- linux 环境若使用TCP SYN(默认)扫描，请先安装 [libpcap-dev]（apt-get install libpcap-dev）。
- Linux/Mac 下的扫描速度会比 Windows 更快，更好的体验请使用 Linux/Mac。
- `passive-config.yaml` 文件用于配置被动 API 扫描的相关参数，例如 API 密钥等。首次运行可能会自动生成该文件。
- 合理配置扫描速度 (`--speed`) 和 QPS 限制 (`--discovery-qps`, `--portscan-qps`, `--svc-qps`, `--passive-qps`)，避免对目标系统造成拒绝服务 (DoS) 风险。
- 使用被动 API 扫描时，可能需要配置代理 (`--passive-proxy`) 以提高访问速度和成功率。
- 详细输出模式 (`--outputx`) 会产生更详细的扫描结果，文件体积也会相对较大。

## 下个版本计划功能

- 更多被动 API 接口集成。
- 优化服务指纹识别引擎，开放自定义服务指纹规则，提升识别准确率。
- 优化端口扫描速度和准确性。
- 完善 IPv6 扫描。
