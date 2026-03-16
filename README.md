# Prism（Prism / 三棱镜）

**Slogan：** Refraction for Resilience — 极轻量、高性能的分布式流量调度与自愈中间件

---

## 1. 核心定位（Positioning）

Prism 是一款专为**高并发 API 服务**设计的 **Spring Boot 生态中间件**。它通过对流量的精准调度（Load Balancing）与主动自愈（Resilience），用于解决微服务架构中常见的：

- 多通道/多节点选择与动态路由
- 异常抖动与故障蔓延
- 熔断、降级、重试与恢复策略落地
- 监控指标驱动的实时权重调整
- 配置热更新与治理透明接入

---

## 2. 核心架构（The Prism Structure）

Prism 的设计遵循“三位一体”的哲学：

### 2.1 Refractor（折射器 - 调度层）
- 基于**加权随机（Weighted Random）**与**平滑轮询（Smooth Round Robin）**等算法
- 实现流量的动态分发与策略切换

### 2.2 Sensor（传感器 - 监控层）
- 实时捕获异常频率、响应延迟等指标
- 为调度层提供决策依据（数据驱动）

### 2.3 Buffer（缓冲器 - 容错层）
- 集成**断路器（Circuit Breaker）**与**指数退避重试（Exponential Backoff）**
- 防止故障扩散，并支持逐步恢复

---

## 3. 五大核心特性（Key Features）

| 特性 | 技术实现描述 |
|------|--------------|
| 智能加权调度 | 采用 `ThreadLocalRandom` 实现高性能加权随机，支持根据通道实时健康度动态调整权重。 |
| 自愈式熔断降级 | 记录异常时间窗口，自动触发熔断（Break）或中断（Interrupt），并支持定时尝试恢复。 |
| 透明化接入 | 提供 Spring Boot Starter，支持声明式注解 `@PrismClient`，实现零侵入式流量治理。 |
| 上下文全链路追踪 | 兼容 TTL（Transmittable Thread Local），确保在异步与虚拟线程环境下日志 TraceId 不丢失。 |
| 多源配置感知 | 支持接入 Redis、Nacos 或本地配置，实现通道信息与负载权重的秒级热更新。 |

---

## 4. 核心组件设计（Component Design）

### 4.1 算法引擎（`prism-algorithm`）

高度抽象的选择器接口，预置了加权避让等策略能力。

```java
public interface PrismSelector {
    // 基于权重和历史上下文选出最佳节点
    PrismNode select(List<PrismNode> nodes, SelectionContext context);
}
```
## 5. 应用场景（Use Cases）
- 多通道支付 / 短链 / 短信网关： 根据成本、成功率动态切换通道
- 灰度发布与流量切分： 通过权重控制新旧版本的流量占比
- 高可用异地多活： 当某地域节点响应变慢时，Prism 自动折射，将流量切换至备用机房
## 6. 为什么叫 Prism？
- Spectrum（光谱）： 后端每一个 API 通道都是一道光谱，Prism 保证每一道光都能在最合适的时间亮起。
- Precision（精准）： 不仅仅是随机，而是基于权重的科学分发。
- Transparency（透明）： 对业务代码透明；它静静地躺在底层，像玻璃一样清澈，却能掌控所有光线的去向。
