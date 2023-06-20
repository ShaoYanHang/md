# 简述 ETCD 及其特点？
etcd 是 CoreOS 团队发起的开源项目，是一个==管理配置信息和服务发现==（service discovery）的项目，
它的目标是构建一个高可用的分布式键值（key-value）数据库，基于 Go 语言实现。
特点：
简单：支持 REST 风格的 HTTP+JSON API
安全：支持 HTTPS 方式的访问
快速：支持并发 1k/s 的写操作
可靠：支持分布式结构，基于 Raft 的一致性算法，Raft 是一套通过选举主节点来实现分布式系统
一致性的算法。

# 简述什么是Kubernetes？

Kubernetes是一个全新的基于容器技术的分布式系统支撑平台。是Google开源的容器集群管理系统

（谷歌内部:Borg）。在Docker技术的基础上，为容器化的应用提供部署运行、资源调度、服务发现和

动态伸缩等一系列完整功能，提高了大规模容器集群管理的便捷性。并且具有完备的集群管理能力，多

层次的安全防护和准入机制、多租户应用支撑能力、透明的服务注册和发现机制、內建智能负载均衡

器、强大的故障发现和自我修复能力、服务滚动升级和在线扩容能力、可扩展的资源自动调度机制以及

多粒度的资源配额管理能力。

# 简述Kubernetes和Docker的关系

Docker 提供容器的生命周期管理和，Docker 镜像构建运行时容器。它的主要优点是将将软件/应用程序
运行所需的设置和依赖项打包到一个容器中，从而实现了可移植性等优点。
Kubernetes 用于关联和编排在多个主机上运行的容器

# 简述Kubernetes常见的部署方式？

kind

二进制

kubeadm

minikube

# 简述Kubernetes如何实现集群管理？

在集群管理方面，Kubernetes将集群中的机器划分为一个Master节点和一群工作节点Node。其中，在Master节点运行着集群管理相关的一组进程kube-apiserver、kube-controller-manager和kube-scheduler，这些进程实现了整个集群的资源管理、Pod调度、弹性伸缩、安全控制、系统监控和纠错等管理能力，并且都是全自动完成的。

# 简述Kubernetes相关基础概念？

## master

k8s集群的管理节点，负责管理集群，提供集群的资源数据访问入口。拥有Etcd存储服务（可
选），运行Api Server进程，Controller Manager服务进程及Scheduler服务进程。

## node（worker）

Node（worker）是Kubernetes集群架构中运行Pod的服务节点，是Kubernetes集 群操作的单元，用来承载被分配Pod的运行，是Pod运行的宿主机。运行docker eninge服务，守护进程 kunelet及负载均衡器kube-proxy。

## pod

运行于Node节点上，若干相关容器的组合(Kubernetes 之 Pod 实现原理)。Pod内包含的容器运 行在同一宿主机上，使用相同的网络命名空间、IP地址和端口，能够通过localhost进行通信。Pod是 Kurbernetes进行创建、调度和管理的最小单位，它提供了比容器更高层次的抽象，使得部署和管理更 加灵活。一个Pod可以包含一个容器或者多个相关容器。

## label

Kubernetes中的Label实质是一系列的Key/Value键值对，其中key与value可自定义。Label可以 附加到各种资源对象上，如Node、Pod、Service、RC等。一个资源对象可以定义任意数量的Label，同 一个Label也可以被添加到任意数量的资源对象上去。Kubernetes通过Label Selector（标签选择器）查 询和筛选资源对象。

## Replication Controller

Replication Controller用来管理Pod的副本，保证集群中存在指定数量的 Pod副本。集群中副本的数量大于指定数量，则会停止指定数量之外的多余容器数量。反之，则会启动 少于指定数量个数的容器，保证数量不变。Replication Controller是实现弹性伸缩、动态扩容和滚动升 级的核心。

## Deployment

Deployment在内部使用了RS来实现目的,Deployment相当于RC的一次升级，其最大 的特色为可以随时获知当前Pod的部署进度。

## HPA（Horizontal Pod Autoscaler）

Pod的横向自动扩容，也是Kubernetes的一种资源，通过追踪 分析RC控制的所有Pod目标的负载变化情况，来确定是否需要针对性的调整Pod副本数量。

## Service

Service(Kubernetes 之服务发现)定义了Pod的逻辑集合和访问该集合的策略，是真实服务的 抽象。Service提供了一个统一的服务访问入口以及服务代理和发现机制，关联多个相同Label的Pod， 用户不需要了解后台Pod是如何运行。

## Volume

Volume是Pod中能够被多个容器访问的共享目录，Kubernetes中的Volume是定义在Pod 上，可以被一个或多个Pod中的容器挂载到某个目录下

## Namespace

Namespace用于实现多租户的资源隔离，可将集群内部的资源对象分配到不同的 Namespace中，形成逻辑上的不同项目、小组或用户组，便于不同的Namespace在共享使用整个集群 的资源的同时还能被分别管理。



# 简述 Kubernetes 集群相关组件？
## Kubernetes Master 控制组件，调度管理整个系统（集群），包含如下组件:

### Kubernetes API Server 

作为 Kubernetes 系统的入口，其封装了核心对象的增删改查操作，以 RESTful API 接口方式提供给外部客户和内部组件调用，集群内各个功能模块之间数据交互和通信的中心枢纽。

### Kubernetes Scheduler 

为新建立的 Pod 进行节点(node)选择(即分配机器)，负责集群的资源调度。

### Kubernetes Controller 

负责执行各种控制器，目前已经提供了很多控制器来保证Kubernetes 的正常运行。
### Replication Controller 

管理维护 Replication Controller，关联 Replication Controller 和 - Pod，保证 Replication Controller 定义的副本数量与实际运行 Pod 数量一致。

### Node Controller 

管理维护 Node，定期检查 Node 的健康状态，标识出(失效|未失效)的Node 节点。
### Namespace Controller 

管理维护 Namespace，定期清理无效的 Namespace，包括Namesapce 下的 API 对象，比如 Pod、Service 等。
### Service Controller 

管理维护 Service，提供负载以及服务代理。
### EndPoints Controller 

管理维护 Endpoints，关联 Service 和 Pod，创建 Endpoints 为Service 的后端，当 Pod 发生变化时，实时更新 Endpoints。
### Service Account Controller 

管理维护 Service Account，为每个 Namespace 创建默认的Service Account，同时为 Service Account 创建 Service Account Secret。
### Persistent Volume Controller 

管理维护 Persistent Volume 和 Persistent Volume Claim，为新的 Persistent Volume Claim 分配 Persistent Volume 进行绑定，为释放的 PersistentVolume 执行清理回收。

### Daemon Set Controller 

管理维护 Daemon Set，负责创建 Daemon Pod，保证指定的 Node上正常的运行 Daemon Pod。
### Deployment Controller 

管理维护 Deployment，关联 Deployment 和 Replication Controller，保证运行指定数量的 Pod。当 Deployment 更新时，控制实现 Replication Controller 和 Pod 的更新。
### Job Controller 管理维护 Job，为 Jod 创建一次性任务 Pod，保证完成 Job 指定完成的任务数
目

### Pod Autoscaler Controller 

实现 Pod 的自动伸缩，定时获取监控数据，进行策略匹配，当满足条件时执行 Pod 的伸缩动作。

# 简述 Kubernetes RC 的机制？

Replication Controller 用来管理 Pod 的副本，保证集群中存在指定数量的 Pod 副本。当定义了 RC 并
提交至 Kubernetes 集群中之后，Master 节点上的 Controller Manager 组件获悉，并同时巡检系统中
当前存活的目标 Pod，并确保目标 Pod 实例的数量刚好等于此 RC 的期望值，若存在过多的 Pod 副本在
运行，系统会停止一些 Pod，反之则自动创建一些 Pod。

# 简述 Kubernetes Replica Set 和 Replication Controller 之间有什么区别？

Replica Set 和 Replication Controller 类似，都是确保在任何给定时间运行指定数量的 Pod 副本。不同
之处在于 RS 使用基于集合的选择器，而 Replication Controller 使用基于权限的选择器。

# 简述 kube-proxy 作用？

kube-proxy 运行在所有节点上，它监听 apiserver 中 service 和 endpoint 的变化情况，创建路由规则
以提供服务 IP 和负载均衡功能。简单理解此进程是 Service 的透明代理兼负载均衡器，其核心功能是将
到某个 Service 的访问请求转发到后端的多个 Pod 实例上。

# 简述 Kubernetes 中 Pod 可能位于的状态？

Pending ：API Server 已经创建该 Pod，且 Pod 内还有一个或多个容器的镜像没有创建，包括正
在下载镜像的过程。
Running ：Pod 内所有容器均已创建，且至少有一个容器处于运行状态、正在启动状态或正在重
启状态。
Succeeded ：Pod 内所有容器均成功执行退出，且不会重启。
Failed ：Pod 内所有容器均已退出，但至少有一个容器退出为失败状态。
Unknown ：由于某种原因无法获取该 Pod 状态，可能由于网络通信不畅导致。



# 简述 Kubernetes 创建一个 Pod 的主要流程？

Kubernetes 中创建一个 Pod 涉及多个组件之间联动，主要流程如下：

1. 客户端提交 Pod 的配置信息（可以是 yaml 文件定义的信息）到 kube-apiserver。
2. Apiserver 收到指令后，通知给 controller-manager 创建一个资源对象。
3. Controller-manager 通过 api-server 将 pod 的配置信息存储到 ETCD 数据中心中。
4. Kube-scheduler 检测到 pod 信息会开始调度预选，会先过滤掉不符合 Pod 资源配置要求的节点，
  然后开始调度调优，主要是挑选出更适合运行 pod 的节点，然后将 pod 的资源配置单发送到
  node 节点上的 kubelet 组件上。
5. Kubelet 根据 scheduler 发来的资源配置单运行 pod，运行成功后，将 pod 的运行信息返回给
  scheduler，scheduler 将返回的 pod 运行状况的信息存储到 etcd 数据中心。

# 简述 Kubernetes 中 Pod 的健康检查方式？

对 Pod 的健康检查可以通过两类探针来检查：LivenessProbe 和 ReadinessProbe。
LivenessProbe 探针 ：用于判断容器是否存活（running 状态），如果 LivenessProbe 探针探测
到容器不健康，则 kubelet 将杀掉该容器，并根据容器的重启策略做相应处理。若一个容器不包含
LivenessProbe 探针，kubelet 认为该容器的 LivenessProbe 探针返回值用于是“Success”。
ReadineeProbe 探针 ：用于判断容器是否启动完成（ready 状态）。如果 ReadinessProbe 探针
探测到失败，则 Pod 的状态将被修改。Endpoint Controller 将从 Service 的 Endpoint 中删除包
含该容器所在 Pod 的 Eenpoint。
startupProbe 探针 ：启动检查机制，应用一些启动缓慢的业务，避免业务长时间启动而被上面
两类探针 kill 掉。

# 简述 Kubernetes 初始化容器（init container）？

init container 的运行方式与应用容器不同，它们必须先于应用容器执行完成，当设置了多个 init
container 时，将按顺序逐个运行，并且只有前一个 init container 运行成功后才能运行后一个 init
container。当所有 init container 都成功运行后，Kubernetes 才会初始化 Pod 的各种信息，并开始创
建和运行应用容器。

# 简述 Kubernetes deployment 升级过程？

初始创建 Deployment 时，系统创建了一个 ReplicaSet，并按用户的需求创建了对应数量的 Pod
副本。
当更新 Deployment 时，系统创建了一个新的 ReplicaSet，并将其副本数量扩展到 1，然后将旧
ReplicaSet 缩减为 2。
之后，系统继续按照相同的更新策略对新旧两个 ReplicaSet 进行逐个调整。
最后，新的 ReplicaSet 运行了对应个新版本 Pod 副本，旧的 ReplicaSet 副本数量则缩减为 0。

# 简述 Kubernetes deployment 升级策略？

在 Deployment 的定义中，可以通过 spec.strategy 指定 Pod 更新的策略，目前支持两种策略：
Recreate（重建）和 RollingUpdate（滚动更新），默认值为 RollingUpdate。
Recreate ：设置 spec.strategy.type=Recreate，表示 Deployment 在更新 Pod 时，会先杀掉所
有正在运行的 Pod，然后创建新的 Pod。
RollingUpdate ：设置 spec.strategy.type=RollingUpdate，表示 Deployment 会以滚动更新的
方式来逐个更新 Pod。同时，可以通过设置 spec.strategy.rollingUpdate 下的两个参数
（maxUnavailable 和 maxSurge）来控制滚动更新的过程。

# 简述 Kubernetes Service 类型？

通过创建 Service，可以为一组具有相同功能的容器应用提供一个统一的入口地址，并且将请求负载分发
到后端的各个容器应用上。其主要类型有：
ClusterIP ：虚拟的服务 IP 地址，该地址用于 Kubernetes 集群内部的 Pod 访问，在 Node 上
kube-proxy 通过设置的 iptables 规则进行转发；
NodePort ：使用宿主机的端口，使能够访问各 Node 的外部客户端通过 Node 的 IP 地址和端口号
就能访问服务；
LoadBalancer ：使用外接负载均衡器完成到服务的负载分发，需要在 spec.status.loadBalancer
字段指定外部负载均衡器的 IP 地址，通常用于公有云。

# 简述 Kubernetes Service 分发后端的策略？
Service 负载分发的策略有：RoundRobin 和 SessionAffinity
RoundRobin：默认为轮询模式，即轮询将请求转发到后端的各个 Pod 上。
SessionAffinity：基于客户端 IP 地址进行会话保持的模式，即第 1 次将某个客户端发起的请求转
发到后端的某个 Pod 上，之后从相同的客户端发起的请求都将被转发到后端相同的 Pod 上。

# 简述 Kubernetes 外部如何访问集群内的服务？
对于 Kubernetes，集群外的客户端默认情况，无法通过 Pod 的 IP 地址或者 Service 的虚拟 IP 地址:虚
拟端口号进行访问。
通常可以通过以下方式进行访问 Kubernetes 集群内的服务：
映射 Pod 到物理机：将 Pod 端口号映射到宿主机，即在 Pod 中采用 hostPort 方式，以使客户端
应用能够通过物理机访问容器应用。
映射 Service 到物理机：将 Service 端口号映射到宿主机，即在 Service 中采用 nodePort 方式，
以使客户端应用能够通过物理机访问容器应用。
映射 Sercie 到 LoadBalancer：通过设置 LoadBalancer 映射到云服务商提供的 LoadBalancer 地
址。这种用法仅用于在公有云服务提供商的云平台上设置 Service 的场景。

# 简述 Kubernetes ingress？
Kubernetes 的 Ingress 资源对象，用于将不同 URL 的访问请求转发到后端不同的 Service，以实现
HTTP 层的业务路由机制。
Kubernetes 使用了 Ingress 策略和 Ingress Controller，两者结合并实现了一个完整的 Ingress 负载均
衡器。
使用 Ingress 进行负载分发时，Ingress Controller 基于 Ingress 规则将客户端请求直接转发到 Service
对应的后端 Endpoint（Pod）上，从而跳过 kube-proxy 的转发功能，kube-proxy 不再起作用，全过
程为：ingress controller + ingress 规则 —-> services。
同时当 Ingress Controller 提供的是对外服务，则实际上实现的是边缘路由器的功能。

# 简述 Kubernetes 镜像的下载策略？
K8s 的镜像下载策略有三种：Always、Never、IFNotPresent。
Always ：镜像标签为 latest 时，总是从指定的仓库中获取镜像。
Never ：禁止从仓库中下载镜像，也就是说只能使用本地镜像。
IfNotPresent ：仅当本地没有对应镜像时，才从目标仓库中下载。默认的镜像下载策略是：当镜
像标签是 latest 时，默认策略是 Always；当镜像标签是自定义时（也就是标签不是 latest），那
么默认策略是 IfNotPresent。

# 简述 Kubernetes 各模块如何与 API Server 通信？
Kubernetes API Server 作为集群的核心，负责集群各功能模块之间的通信。
集群内的各个功能模块通过 API Server 将信息存入 etcd，当需要获取和操作这些数据时，则通过 API
Server 提供的 REST 接口（用 GET、LIST 或 WATCH 方法）来实现，从而实现各模块之间的信息交互。
如 kubelet 进程与 API Server 的交互：每个 Node 上的 kubelet 每隔一个时间周期，就会调用一次 API
Server 的 REST 接口报告自身状态，API Server 在接收到这些信息后，会将节点状态信息更新到 etcd
中。
如 kube-controller-manager 进程与 API Server 的交互：kube-controller-manager 中的 Node
Controller 模块通过 API Server 提供的 Watch 接口实时监控 Node 的信息，并做相应处理。
如 kube-scheduler 进程与 API Server 的交互：Scheduler 通过 API Server 的 Watch 接口监听到新建
Pod 副本的信息后，会检索所有符合该 Pod 要求的 Node 列表，开始执行 Pod 调度逻辑，在调度成功
后将 Pod 绑定到目标节点上。
