# 功能总览

多云编排的功能列表如下：

- 统一管理面：多云编排拥有统一的管理面，由该管理面进行多个多云实例的管理，统一请求入口（多云编排实例的 LCM），而所有其他关于多云编排的请求可以部署在 global 集群。
- 多实例：支持创建多个多云实例，实例之间隔离工作，互不感知、互不影响。
- 集群一键接入：支持从现有纳管集群中一键接入集群到多云实例中，并实时同步接入的集群最新信息（删除的同时一并删除）。
- 原生 API 支持：支持所有 Kubernetes 原生 API。
- 多云应用分发：丰富的多云应用分发策略、差异化策略等。
- 应用故障转移：内置提供应用多云故障转移（failover）能力。
- 应用一键转换：实现 DCE4 到 DCE5 应用的一键转换。
- 跨集群弹性伸缩：根据应用负载的需求，在不同的集群之间动态地调整资源。
- 可观测性：丰富的审计、指标度量，提高可观测性的能力。
- 对接全局管理权限：以工作空间管理用户访问范围，对用户和实例执行鉴权操作。

## 多云实例管理

- 添加多云实例：支持添加实例时不加入任何集群，以创建空实例，即不包含任何集群的实例。

- 查看多云实例：查看实例列表并支持根据实例名称进行检索

    支持查看当前租户环境内全部的多云实例列表和实例的基础信息、实例内已接入集群的 CPU、内存的使用率情况、实例内已接入集群的状态情况等，支持查看版本信息、实例创建时间和实例状态。

- 移除多云实例：移除多云实例时自动进行校验；为保证数据安全，仅允许当前实例中不包含任何集群时，才能够进行移除多云实例。

## 实例内集群管理

- 添加集群

    支持动态（热插拔）将新集群加入到当前多云实例中，添加了选取容器管理的集群界面，默认展示当前有权限接入的全部待接入集群，

- 查看集群

    支持查看已接入当前实例的所有集群，以及这些集群的细节信息，包含集群名称、状态、平台、地域、可用区、Kubernetes 版本等。

- 移除集群

    支持动态（热插拔）将集群从当前多云实例中移除。

    移除校验：支持对集群的资源进行校验，告知用户需要事先移除集群内的存在的多个工作负载、命名空间、存储声明、配置与密钥等，提示移除的风险。

- 使用 Kubectl 管理实例资源

    支持获取 kubeconfig 链接信息，用户可在本地终端管理多云实例。

    支持通过网页终端 cloudshell 进行多云实例的管理。

## 多云工作负载

- 创建多云无状态负载

    镜像创建：界面化创建无状态工作负载，添加差异化配置、分发策略等差异化配置。

    差异化设置：支持在创建负载时，进行差异化配置，支持按集群进行差异化配置集群部署 Pod 数/CPU/内存/升级策略，差异化配置集群镜像、部署策略、容器脚本、容器存储、容器日志、调度策略、标签和注解等。

    YAML 创建：用户通过 YAML 进行创建工作负载。

    YAML 语法校验：支持对用户输入的 YAML 进行语法校验，对错误语法给予提示。

- 多云工作负载详情

    基本信息：支持查看无状态负载的部署详情信息 Pod 数量情况，活跃集群信息。资源负载提供单 Pod 监控查看能力，可跳转到 Insight Pod 日志信息，提供单 Pod 日志查看能力。

    部署信息：查看无状态负载的部署详情信息，并支持多云工作负载的重启/暂停/恢复/释放等操作。

    实例列表：支持按照集群的方式展示工作负载在多个 Kubernetes 集群的 Pod 信息，支持一键快速跳转到对应集群的工作负载详情界面，查看对应 Pod 的监控、日志信息。

    服务列表：支持按照集群的方式切分集群内的工作负载服务信息，支持一键快速跳转到对应集群的工作负载详情界面，查看对应服务的监控、日志信息。

- 更新多云工作负载

    页面编辑：支持通过页面的方式编辑集群

    YAML 编辑：支持通过 YAML 的的方式变更多云工作负载的配置信息

- 删除多云工作负载

    页面删除：支持通过页面的方式进行多云工作负载的删除，同时删除在多集群内的部署

    支持 cloudshell 及终端进行删除：支持通过终端进行删除

    删除提示：提醒在删除时，需要进行二次确认

## 资源管理

- 多云命名空间

    多云命名空间管理：支持查看多云命名空间资源列表

    查看多云命名空间列表：提供列表可以查看多集群的命名空间信息

    创建多云命名空间：提供通过界面化创建多云命名空间的能力

    删除多云命名空间：支持对空闲的多云命名空间进行删除

- 多云密钥

    查看多云存储声明列表：支持查看创建的多云 PVC 资源列表

    创建多云存储声明：提供通过界面化和 YAML 多种方式进行存储声明的创建

    删除多云存储声明：支持对空闲存储声明进行删除操作

- 多云配置项

    多云配置项管理：支持查看多云 ConfigMap 资源列表

    多云密钥管理：支持查看多云 Secret 资源列表

    创建多云配置或多云密钥：支持通过界面化或 YAML 完成对多云配置或多云密钥的创建，并统一显示在用户界面

    删除多云配置或多云密钥：支持对空闲的多云配置或多云秘钥进行删除操作，对于在使用中的配置或秘钥不允许删除

- 多云服务和路由

    创建服务和路由：支持通过界面化或 YAML 完成对 Service 创建，并统一显示在用户界面

    删除服务和路由：支持对 Service/Ingress 进行删除操作，对于在使用中的给出对应的删除提示

## 策略管理

- 部署策略

    部署策略列表：支持在界面上查看当前实例的部署策略列表及其关联的多云资源

    创建部署策略：支持以 YAML 的方式维护创建和编辑部署策略信息

    删除部署策略：仅对空闲的部署策略提供删除按钮

- 差异化策略

    差异化策略列表：支持在界面上查看当前实例的部署策略列表及其关联的多云资源

    创建差异化策略：支持以 YAML 的方式维护创建和编辑差异化策略信息

    删除差异化策略：支持在界面上查看当前实例的差异化策略列表以及当前差异化策略关联的多云资源

## 系统设置

- 集群健康检查调度
    
    配置集群健康状态标记成功/失败的时长

- 故障转移

    配置参数实现当应用部署在某个集群内发生故障时，将会自动将故障集群 Pod 副本迁移到其他可用集群，以保证服务稳定性的能力

- 定时重新调度

    定期检查集群内的 Pod 状态，如果某个 Pod 在指定时间内一直保持不可调度状态，则会自动将其驱逐以避免资源浪费

- 弹性伸缩

    在实例控制面集群安装 karmada-metrics-adapter 以提供 metrics API。默认关闭状态。
