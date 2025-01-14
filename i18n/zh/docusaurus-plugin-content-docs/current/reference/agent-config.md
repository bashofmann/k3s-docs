---
title: K3s Agent 配置
weight: 2
---

在本节中，你将学习如何配置 K3s Agent。

- [Logging](#logging)
- [集群选项](#集群选项)
- [数据](#数据)
- [节点](#节点)
- [运行时](#运行时)
- [网络](#网络)
- [自定义标志](#自定义标志)
- [实验功能](#实验功能)
- [已弃用](#已弃用)
- [Agent 的节点标签和污点](#agent-的节点标签和污点)
- [K3s Agent CLI 帮助](#k3s-agent-cli-帮助)

### Logging

| 标志 | 默认 | 描述 |
| ----------------------- | ------- | -------------------------------------------------------------------- |
| `-v` value | 0 | 表示日志级别详细程度的数字 |
| `--vmodule` value | N/A | pattern=N 格式，用逗号分隔的列表，用于文件过滤日志 |
| `--log value, -l` value | N/A | 记录到文件 |
| `--alsologtostderr` | N/A | 记录到标准错误以及文件（如果设置） |

### 集群选项

| 标志 | 环境变量 | 描述 |
| -------------------------- | -------------------- | ------------------------------------ |
| `--token value, -t` value | `K3S_TOKEN` | 用于身份验证的令牌 |
| `--token-file` value | `K3S_TOKEN_FILE` | 用于身份验证的令牌文件 |
| `--server value, -s` value | `K3S_URL` | 要连接的 server |

### 数据

| 标志 | 默认 | 描述 |
| ---------------------------- | ---------------------- | -------------------- |
| `--data-dir value, -d` value | "/var/lib/rancher/k3s" | 保存状态的文件夹 |

### 节点

| 标志 | 环境变量 | 描述 |
| -------------------- | -------------------- | --------------------------------------------------- |
| `--node-name` value | `K3S_NODE_NAME` | 节点名称 |
| `--with-node-id` | N/A | 将 ID 尾附到节点名称 |
| `--node-label` value | N/A | 使用一组标签注册和启动 kubelet |
| `--node-taint` value | N/A | 使用一组污点注册 kubelet |

### 运行时

| 标志 | 默认 | 描述 |
| ------------------------------------ | ---------------------------------- | ------------------------------------------------------------------ |
| `--container-runtime-endpoint` value | N/A | 禁用嵌入式 containerd 并使用替代的 CRI 实现 |
| `--pause-image` value | "docker.io/rancher/pause:3.1" | 为 containerd 或 Docker 沙盒定制的 pause 镜像 |
| `--private-registry` value | "/etc/rancher/k3s/registries.yaml" | 私有镜像仓库配置文件 |

### 网络

| 标志 | 环境变量 | 描述 |
| --------------------------- | -------------------- | ----------------------------------------- |
| `--node-ip value, -i` value | N/A | 节点的 IP 地址 |
| `--node-external-ip` value | N/A | 节点的外部 IP 地址 |
| `--resolv-conf` value | `K3S_RESOLV_CONF` | Kubelet resolv.conf 文件 |
| `--flannel-iface` value | N/A | 覆盖默认的 Flannel interface |
| `--flannel-conf` value | N/A | 覆盖默认的 Flannel 配置文件 |
| `--flannel-cni-conf` value | N/A | 覆盖默认的 Flannel CNI 配置文件 |

### 自定义标志

| 标志 | 描述 |
| ------------------------ | -------------------------------------- |
| `--kubelet-arg` value | kubelet 进程的自定义标志 |
| `--kube-proxy-arg` value | kube-proxy 进程的自定义标志 |

### 实验功能

| 标志 | 描述 |
| ------------ | ------------------------------------- |
| `--rootless` | 无根运行 |
| `--docker` | 使用 cri-dockerd 而不是 containerd |

### 已弃用

| 标志 | 环境变量 | 描述 |
| ------------------------ | -------------------- | ---------------------------- |
| `--no-flannel` | N/A | 使用 `--flannel-backend=none` |
| `--cluster-secret` value | `K3S_CLUSTER_SECRET` | 使用 `--token` |

### Agent 的节点标签和污点

K3s Agent 可以通过 `--node-label` 和 `--node-taint` 选项来配置，它们会为 kubelet 添加标签和污点。这两个选项只能在注册时添加标签和/或污点，因此它们只能被添加一次，之后不能再通过运行 K3s 命令来改变。

下面是显示如何添加标签和污点的示例：

```bash
     --node-label foo=bar \
     --node-label hello=world \
     --node-taint key1=value1:NoExecute
```

如果你想在节点注册后更改节点标签和污点，你需要使用 `kubectl`。关于如何添加[污点](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)和[节点标签](https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes/#add-a-label-to-a-node)的详细信息，请参阅官方 Kubernetes 文档。

### K3s Agent CLI 帮助

> 如果某个选项出现在括号中（例如 `[$K3S_URL]`），该选项可以作为该名称的环境变量传入。

```bash
NAME:
   k3s agent - Run node agent

USAGE:
   k3s agent [OPTIONS]

OPTIONS:
   --config FILE, -c FILE                     (config) Load configuration from FILE (default: "/etc/rancher/k3s/config.yaml") [$K3S_CONFIG_FILE]
   --debug                                    (logging) Turn on debug logs [$K3S_DEBUG]
   -v value                                   (logging) Number for the log level verbosity (default: 0)
   --vmodule value                            (logging) Comma-separated list of pattern=N settings for file-filtered logging
   --log value, -l value                      (logging) Log to file
   --alsologtostderr                          (logging) Log to standard error as well as file (if set)
   --token value, -t value                    (cluster) Token to use for authentication [$K3S_TOKEN]
   --token-file value                         (cluster) Token file to use for authentication [$K3S_TOKEN_FILE]
   --server value, -s value                   (cluster) Server to connect to [$K3S_URL]
   --data-dir value, -d value                 (agent/data) Folder to hold state (default: "/var/lib/rancher/k3s")
   --node-name value                          (agent/node) Node name [$K3S_NODE_NAME]
   --with-node-id                             (agent/node) Append id to node name
   --node-label value                         (agent/node) Registering and starting kubelet with set of labels
   --node-taint value                         (agent/node) Registering kubelet with set of taints
   --image-credential-provider-bin-dir value  (agent/node) The path to the directory where credential provider plugin binaries are located (default: "/var/lib/rancher/credentialprovider/bin")
   --image-credential-provider-config value   (agent/node) The path to the credential provider plugin config file (default: "/var/lib/rancher/credentialprovider/config.yaml")
   --container-runtime-endpoint value         (agent/runtime) Disable embedded containerd and use alternative CRI implementation
   --pause-image value                        (agent/runtime) Customized pause image for containerd or docker sandbox (default: "rancher/mirrored-pause:3.6")
   --snapshotter value                        (agent/runtime) Override default containerd snapshotter (default: "overlayfs")
   --private-registry value                   (agent/runtime) Private registry configuration file (default: "/etc/rancher/k3s/registries.yaml")
   --node-ip value, -i value                  (agent/networking) IPv4/IPv6 addresses to advertise for node
   --node-external-ip value                   (agent/networking) IPv4/IPv6 external IP addresses to advertise for node
   --resolv-conf value                        (agent/networking) Kubelet resolv.conf file [$K3S_RESOLV_CONF]
   --flannel-iface value                      (agent/networking) Override default flannel interface
   --flannel-conf value                       (agent/networking) Override default flannel config file
   --flannel-cni-conf value                   (agent/networking) Override default flannel cni config file
   --kubelet-arg value                        (agent/flags) Customized flag for kubelet process
   --kube-proxy-arg value                     (agent/flags) Customized flag for kube-proxy process
   --protect-kernel-defaults                  (agent/node) Kernel tuning behavior. If set, error if kernel tunables are different than kubelet defaults.
   --rootless                                 (experimental) Run rootless
   --selinux                                  (agent/node) Enable SELinux in containerd [$K3S_SELINUX]
   --lb-server-port value                     (agent/node) Local port for supervisor client load-balancer. 如果 supervisor 和 apiserver 没有位于同一位置，则比该端口小 1 的端口也将用于 apiserver 客户端负载均衡器(default: 6444) [$K3S_LB_SERVER_PORT]
   --docker                                   (deprecated) Use docker instead of containerd
   --no-flannel                               (deprecated) use --flannel-backend=none
   --cluster-secret value                     (deprecated) use --token [$K3S_CLUSTER_SECRET]
```
