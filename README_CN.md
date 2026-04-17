<div align="center">

**🌐 Select Language / 选择语言**

[English](README.md) | **中文** | [Español](README_ES.md) | [Русский](README_RU.md) | [Bahasa Indonesia](README_ID.md) | [Türkçe](README_TR.md) | [Tiếng Việt](README_VN.md) | [ภาษาไทย](README_TH.md) | [हिन्दी](README_HI.md) | [اردو](README_UR.md) | [Filipino](README_PH.md)

</div>

---

# binance&bybit&okx P2P auto_payment_bot：全自动按单付款 · 查账核验 · 自动放币机器人

> 面向 Binance、Bybit、OKX P2P 商户的专业级自动化交易引擎 —— **按单自动付款**、**智能查账核验**、**秒级自动放币**，支持多设备多支付 App 并行处理。

# [P2P 自动付款放币机器人 (Binance & Bybit & OKX) 官方网站](https://apip2p.top)

## 🎬 视频演示

[![观看演示](https://img.shields.io/badge/YouTube-观看演示视频-red?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=INewAPHXa5c)

> 📺 观看完整演示视频，了解 `binance&bybit&okx P2P auto_payment_bot` 的实际运行效果 —— 从订单检测到自动付款、查账核验、秒级放币全流程展示。

专为 P2P 商户（广告主）设计的终极自动化订单结算方案。告别人工盯单、手动转账、手动核验的低效模式，通过 `binance&bybit&okx P2P auto_payment_bot` 实现 **全天候全自动** P2P 按单付款、到账核验与秒级放币。最大化订单成交效率、保障资金安全、解放商户双手。

---

## 📝 概述与架构
<img width="1360" height="881" alt="2" src="https://github.com/user-attachments/assets/b2c18af8-6b0f-4062-a947-bb10e99aeed7" />


`binance&bybit&okx P2P auto_payment_bot` 是一款专为在 **Binance**、**Bybit** 和 **OKX** 平台从事 P2P 交易的商户（广告主）量身打造的 **本地桌面自动化客户端**。

在 P2P 交易中，商户面临大量重复操作：

- ❌ **手动付款**：每来一笔买单，商户需要手动打开支付 App、输入金额和收款人、确认转账 —— 大量订单下极其疲劳
- ❌ **手动查账**：每来一笔卖单，需要打开支付 App 核验买家是否真的付款、金额是否正确
- ❌ **手动放币**：核验通过后需要回到交易所手动点击放币按钮
- ❌ **夜间空档**：凌晨无人值守时，大量订单无法及时处理，错失交易机会

`binance&bybit&okx P2P auto_payment_bot` 通过 **智能设备自动化 + 交易所 API** 联动，一站式解决以上全部痛点：

- ✅ **🔥 按单自动付款（核心功能）**：交易所收到买单 → 自动在您的支付 App 中完成转账操作 → 无需人工干预
- ✅ **智能查账核验**：卖单中买家标记已付款 → 自动打开支付 App 核验到账金额、付款人信息
- ✅ **秒级自动放币**：核验通过后立即调用交易所 API 完成放币，毫秒级响应
- ✅ **多设备并行**：同时连接多部手机，不同设备处理不同订单，并发能力倍增
- ✅ **多交易所统一管理**：Binance、Bybit、OKX 三大平台在同一界面操作
- ✅ **全本地运行**：零外部服务器依赖，API 密钥永不离开本机

**软件定位**：`binance&bybit&okx P2P auto_payment_bot` 负责 **按单付款 + 查账核验 + 自动放币**（订单结算）；配合 [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot)（自动改价竞价）使用，可实现 P2P 商户业务的全链路智能自动化。

---

## 🚀 核心功能（算法与工作流程）

### 1. 🔥 按单自动付款（自动转账引擎）

> **这是 `binance&bybit&okx P2P auto_payment_bot` 最核心、最具差异化的功能。**

当交易所收到一笔 P2P 买单（买家要购买您的加密货币），软件能 **全自动完成付款操作**：

- **订单实时感知**：5 秒轮询周期自动检测新到 P2P 订单，零延迟响应
- **智能设备调度**：根据订单的支付方式（如 Maya、GCash、银行转账等），自动匹配空闲手机设备
- **自动化付款执行**：自动在手机支付 App 中完成完整转账流程 —— 导航到转账页面、输入金额、输入收款信息、确认交易，全程无需人工操作
- **操作流程学习**：首次操作时录制操作教程，后续相同场景自动回放，越用越快
- **多支付方式支持**：同一设备可配置多种支付方式，灵活应对不同订单需求
- **转账截图存档**：每笔转账完成后自动截取凭证，支持事后查验

#### 💡 实现概念（自动转账调度引擎）

```python
# [伪代码] 订单感知 → 设备匹配 → 自动转账的完整调度流程
import asyncio
from dataclasses import dataclass
from typing import Optional, List

@dataclass
class TransferTask:
    order_id:       str
    amount:         float
    currency:       str
    payee_name:     str
    payee_account:  str
    payment_method: str    # e.g. "maya", "gcash", "bank_transfer"
    status:         str    # pending → running → success / failed

class TransferScheduler:
    POLL_INTERVAL = 5  # 秒

    def __init__(self, exchange_clients: dict, device_pool: 'DevicePool'):
        self.exchanges   = exchange_clients
        self.device_pool = device_pool

    async def run(self):
        """主循环：轮询订单 → 匹配设备 → 派发转账"""
        while True:
            for platform, client in self.exchanges.items():
                orders = await client.get_pending_orders()
                for order in orders:
                    if self._is_new_order(order):
                        await self._dispatch(order)
            await asyncio.sleep(self.POLL_INTERVAL)

    async def _dispatch(self, order) -> None:
        """匹配空闲设备，创建并执行转账任务"""
        device = self.device_pool.find_idle_device(order.payment_method)
        if not device:
            return  # 暂无空闲设备，下一轮重试
        task = TransferTask(
            order_id=order.id, amount=order.fiat_amount,
            payee_name=order.counterparty, payee_account=order.account,
            payment_method=order.payment_method, status="pending",
        )
        await device.execute_transfer(task)
```

```javascript
// [伪代码] Node.js 风格 — 订单轮询与设备调度
class TransferScheduler {
    constructor(exchangeClients, devicePool) {
        this.exchanges  = exchangeClients;
        this.devicePool = devicePool;
        this.pollInterval = 5000; // 5s
    }

    async start() {
        while (true) {
            for (const [platform, client] of Object.entries(this.exchanges)) {
                const orders = await client.getPendingOrders();
                for (const order of orders) {
                    if (this.isNewOrder(order)) {
                        await this.dispatch(order);
                    }
                }
            }
            await this.sleep(this.pollInterval);
        }
    }

    async dispatch(order) {
        const device = this.devicePool.findIdleDevice(order.paymentMethod);
        if (!device) return; // 暂无空闲设备
        await device.executeTransfer({
            orderId: order.id,
            amount: order.fiatAmount,
            payeeName: order.counterparty,
            paymentMethod: order.paymentMethod,
        });
    }
}
```

---

### 2. 智能查账核验（到账验证引擎）

当您挂出 P2P 卖单，买家声称已付款后，软件自动打开支付 App 核验到账情况：

- **账单自动导航**：自动打开指定支付 App，导航到交易记录/账单页面
- **金额精准匹配**：在账单列表中定位金额匹配的最近交易，支持微小容差（防银行手续费影响）
- **付款人核验**：交叉比对付款人姓名与订单买家信息
- **交易唯一 ID 提取**：读取 Reference Number / UTR / 交易流水号，作为全局唯一标识
- **防重放保护**：每笔交易 ID 记录到本地数据库，同一笔到账绝不触发两次放币

#### 💡 实现概念（到账验证引擎）

```go
// [伪代码] Go 风格 — 到账核验与防重放保护
package verifier

type BillVerifyResult struct {
    Matched     bool
    Amount      float64
    SenderName  string
    UniqueID    string   // 银行 Reference Number
}

type BillVerifier struct {
    archive    *BillArchive   // 已处理交易存档（防重放）
    tolerance  float64        // 金额容差
}

func (v *BillVerifier) Verify(order P2POrder) (*BillVerifyResult, error) {
    // 1. 自动导航到支付 App 的账单页面
    bills, err := v.navigateToBillList(order.PaymentApp)
    if err != nil {
        return nil, err
    }

    // 2. 在账单列表中查找金额匹配的最近交易
    for _, bill := range bills {
        if math.Abs(bill.Amount - order.FiatAmount) <= v.tolerance {
            // 3. 进入详情页，提取完整信息
            detail := v.openBillDetail(bill)

            // 4. 防重放检查：该交易是否已被使用
            if v.archive.Exists(detail.UniqueID) {
                continue  // 已使用，跳过
            }

            return &BillVerifyResult{
                Matched:    true,
                Amount:     detail.Amount,
                SenderName: detail.SenderName,
                UniqueID:   detail.UniqueID,
            }, nil
        }
    }
    return &BillVerifyResult{Matched: false}, nil
}
```

```typescript
// [伪代码] TypeScript 风格 — 账单核验服务
interface BillDetail {
    amount:     number;
    senderName: string;
    uniqueId:   string;   // 交易唯一标识
    timestamp:  Date;
}

class BillVerifyService {
    private archive: BillArchive;
    private tolerance = 0.02;  // 金额容差

    async verify(order: P2POrder): Promise<VerifyResult> {
        // 导航到支付 App 账单页面
        const bills = await this.navigateToBillList(order.paymentApp);

        // 查找金额匹配的最近交易
        const matched = bills.find(bill =>
            Math.abs(bill.amount - order.fiatAmount) <= this.tolerance
            && !this.archive.exists(bill.uniqueId)
        );

        if (!matched) return { success: false };

        // 记录到存档，防止重复放币
        this.archive.record(matched.uniqueId, order.orderId);

        return {
            success:    true,
            amount:     matched.amount,
            senderName: matched.senderName,
            uniqueId:   matched.uniqueId,
        };
    }
}
```

---

### 3. 自动放币执行（秒级 API 放行）

- **即时放币**：查账核验通过后，立即调用交易所 P2P API 完成放币操作，全程零人工干预
- **放币结果验证**：调用放币接口后，主动轮询订单状态确认已变为"已完成"，防止接口静默失败
- **失败重试机制**：放币请求失败时，按指数退避策略自动重试（最多 3 次），超出上限推送告警
- **三平台统一适配**：Binance C2C SAPI / Bybit V5 P2P / OKX V5 P2P，统一接口，无缝切换

#### 💡 实现概念（跨平台自动放币执行器）

```python
# [伪代码] 跨平台放币执行器 — 适配 Binance / Bybit / OKX
import asyncio, logging

logger = logging.getLogger("release_executor")

class PlatformAdapterFactory:
    """工厂模式：根据交易所创建对应的 API 适配器"""
    @staticmethod
    def create(platform: str, credentials: dict):
        if platform == "binance":
            return BinanceP2PAdapter(credentials)
        elif platform == "bybit":
            return BybitP2PAdapter(credentials)
        elif platform == "okx":
            return OkxP2PAdapter(credentials)
        raise ValueError(f"不支持的平台: {platform}")

class ReleaseExecutor:
    MAX_RETRIES  = 3
    RETRY_BASE_S = 2   # 指数退避基数（秒）

    async def release(self, platform: str, order_id: str, verify_result: dict) -> bool:
        adapter = PlatformAdapterFactory.create(platform, self.credentials)
        for attempt in range(self.MAX_RETRIES):
            try:
                resp = await adapter.release_order(order_id)
                if resp.success:
                    confirmed = await self._poll_completion(adapter, order_id)
                    if confirmed:
                        logger.info(f"[放币成功] {platform} 订单 {order_id}")
                        self._record_to_archive(order_id, verify_result)
                        return True
            except Exception as e:
                wait = self.RETRY_BASE_S ** (attempt + 1)
                logger.warning(f"[重试 {attempt+1}] {order_id}: {e}，{wait}s 后重试")
                await asyncio.sleep(wait)
        return False
```

```rust
// [伪代码] Rust 风格 — 放币执行与重试
use std::time::Duration;

struct ReleaseExecutor {
    max_retries: u32,
    retry_base: Duration,
}

impl ReleaseExecutor {
    async fn release(&self, platform: &str, order_id: &str) -> Result<bool, Error> {
        let adapter = create_platform_adapter(platform)?;

        for attempt in 0..self.max_retries {
            match adapter.release_order(order_id).await {
                Ok(resp) if resp.success => {
                    if self.poll_completion(&adapter, order_id).await? {
                        log::info!("[放币成功] {} 订单 {}", platform, order_id);
                        return Ok(true);
                    }
                }
                Err(e) => {
                    let wait = self.retry_base * 2u32.pow(attempt);
                    log::warn!("[重试 {}] {}: {} — {}s 后重试",
                        attempt + 1, order_id, e, wait.as_secs());
                    tokio::time::sleep(wait).await;
                }
                _ => {}
            }
        }
        Ok(false)
    }
}
```

---

### 4. 多设备并行处理 & 智能调度

- **多设备连接**：同时连接多部 Android 手机（USB / WiFi），每台设备独立运行
- **实时投屏**：高性能画面同传（30-60fps / 30-70ms 延迟），在 PC 端直观监控设备状态
- **智能任务分配**：根据订单支付方式，自动匹配已安装对应 App 的空闲设备
- **独立任务队列**：每台设备维护独立的 FIFO 任务队列，互不阻塞
- **设备状态监控**：实时显示每台设备的连接状态、运行状态、当前任务、账户余额
- **操作教程隔离**：不同设备、不同 App、不同支付方式的操作流程分别隔离

#### 💡 实现概念（多设备并行调度器）

```python
# [伪代码] 多设备任务调度器 — 并行处理 + 智能分配
from dataclasses import dataclass, field
from typing import Dict, List
from collections import deque

@dataclass
class Device:
    serial:          str
    alias:           str
    status:          str           # idle / busy / offline
    supported_apps:  List[str]     # ["maya", "gcash", "alipay"]
    task_queue:      deque = field(default_factory=deque)

class DeviceScheduler:
    def __init__(self):
        self.devices: Dict[str, Device] = {}

    def find_idle_device(self, payment_method: str) -> Device | None:
        """查找支持该支付方式的空闲设备"""
        for device in self.devices.values():
            if (device.status == "idle"
                    and payment_method in device.supported_apps):
                return device
        return None

    def enqueue_task(self, task: 'TransferTask') -> bool:
        """将任务加入最佳设备的队列"""
        device = self.find_idle_device(task.payment_method)
        if device:
            device.task_queue.append(task)
            return True
        return False  # 暂无可用设备
```

```javascript
// [伪代码] JavaScript 风格 — 设备管理器
class DeviceScheduler {
    constructor() {
        this.devices = new Map(); // serial → Device
    }

    findIdleDevice(paymentMethod) {
        for (const device of this.devices.values()) {
            if (device.status === 'idle'
                && device.supportedApps.includes(paymentMethod)) {
                return device;
            }
        }
        return null;
    }

    enqueueTask(task) {
        const device = this.findIdleDevice(task.paymentMethod);
        if (device) {
            device.taskQueue.push(task);
            device.status = 'busy';
            return true;
        }
        return false;
    }
}
```

---

### 5. 操作流程学习系统（教程录制与回放）

- **一次录制、无限回放**：首次使用时手动录制一次支付 App 操作流程，系统自动学习
- **无需编程**：通过可视化投屏界面，点哪里录哪里，零代码完成教程配置
- **多维度索引**：教程按 设备 × App × 支付方式 × 分辨率 多维索引，精准匹配
- **跨设备共享**：同分辨率的不同设备可共享教程，无需重复录制
- **自动凭证填充**：密码、PIN 等敏感信息由本地配置自动注入，不存储在教程中
- **双模式教程**：转账教程（完成付款）和查账教程（核验到账）分别独立录制

---

### 6. 风控与反欺诈保护

- **交易唯一 ID 去重**：每笔银行流水的 Reference Number / UTR 记录到本地数据库，同一笔交易不会触发两次放币
- **金额精准匹配**：到账金额与订单金额的偏差超过容差阈值时，拒绝自动放币
- **超时保护**：超过预设时限仍未查到到账记录，不放币，可自动触发申诉
- **余额监控**：实时监控支付 App 余额，余额不足时自动暂停接单
- **全本地存档**：所有交易记录存储在本地数据库（180 天保留期，自动清理过期数据）

---

### 7. 状态持久化与会话恢复

- **配置自动保存**：所有账户配置、API 密钥、设备信息、教程数据在每次变更时实时写入本地文件
- **崩溃恢复**：软件重启后自动加载上次配置，无需重新输入任何参数
- **未处理订单恢复**：重启时扫描交易所仍处于"待处理"状态的订单，避免订单遗漏

---

## 🏗️ 系统架构

```
┌───────────────────────────────────────────────────────────────────────┐
│                          桌面 GUI 主窗口                                │
│  ┌─────────────┐ ┌──────────────┐ ┌──────────────┐ ┌─────────────┐  │
│  │ 交易所管理    │ │  设备管理     │ │  实时投屏     │ │  日志 & 审计 │  │
│  │Binance/Bybit│ │ (多手机列表)  │ │ (画面同传)    │ │  (实时流)   │  │
│  │  /OKX Tab   │ │              │ │              │ │             │  │
│  └──────┬──────┘ └──────┬───────┘ └──────┬───────┘ └─────────────┘  │
│         │               │               │                            │
│  ┌──────▼───────────────▼───────────────▼────────────────────────┐   │
│  │                    Signal / Slot 事件总线                        │   │
│  └──────┬───────────────┬───────────────┬────────────────────────┘   │
│         │               │               │                            │
│  ┌──────▼──────┐ ┌──────▼───────┐ ┌─────▼───────┐ ┌──────────────┐ │
│  │ 订单轮询器   │ │  转账执行器   │ │  查账核验器  │ │  状态持久化   │ │
│  │ (5s 周期)   │ │ (按单付款)   │ │ (到账验证)  │ │  (JSON/DB)  │ │
│  └──────┬──────┘ └──────┬───────┘ └─────┬───────┘ └──────────────┘ │
│         │               │               │                            │
│  ┌──────▼───────────────▼───────────────▼────────────────────────┐   │
│  │                    防重放 & 风控保护层                            │   │
│  │  ┌─────────────┐  ┌──────────────┐  ┌─────────────────────┐   │   │
│  │  │ 金额容差匹配 │  │ 交易ID去重   │  │ 余额监控 & 超时保护  │   │   │
│  │  └─────────────┘  └──────────────┘  └─────────────────────┘   │   │
│  └───────────────────────────────────────────────────────────────┘   │
└──────────────────────────────┬────────────────────────────────────────┘
                               │
                 ┌─────────────▼─────────────────┐
                 │       交易所 API 适配器层        │
                 │  ┌────────┐ ┌───────┐ ┌─────┐  │
                 │  │Binance │ │ Bybit │ │ OKX │  │
                 │  │C2C SAPI│ │V5 P2P │ │V5 P2P│  │
                 │  └───┬────┘ └───┬───┘ └──┬──┘  │
                 └──────┼─────────┼────────┼──────┘
                        │         │        │
                ┌───────▼───┐ ┌───▼────┐ ┌─▼──────┐
                │Binance API│ │Bybit   │ │OKX     │
                │  Server   │ │Server  │ │Server  │
                └───────────┘ └────────┘ └────────┘
```

---

## ⚙️ 技术规格与底层机制

| 组件 | 参数 | 说明 |
|------|------|------|
| 订单轮询周期 | 5,000 ms | 三交易所独立轮询，实时感知新订单 |
| 金额匹配容差 | 0.01–0.10 | 可配置，吸收银行手续费导致的微小差异 |
| 放币重试间隔 | 指数退避 2/4/8s | 最多 3 次，超上限推送告警 |
| 交易去重保留 | 180 天 | 本地数据库自动清理过期记录 |
| 设备投屏帧率 | 30–60 fps | 低延迟实时画面同传（30-70ms） |
| 多设备并发 | 无上限 | USB + WiFi 混合连接，每设备独立线程 |
| 教程存储 | JSON 文件 | 按设备 × App × 支付方式 × 分辨率索引 |
| UI 主题 | 暗色 / 亮色 | 深色护眼与浅色清晰双模式 |
| 界面语言 | 中文 / English | 内置完整双语支持 |

### API 认证与请求签名

软件支持三大交易所的 API 认证签名方案：

```python
# [伪代码] HMAC-SHA256 请求签名 — 适配多平台
import hashlib, hmac, time, base64

class MultiPlatformAuth:
    """统一的多交易所认证签名"""

    def sign_binance(self, params: dict, secret: str) -> str:
        """Binance: 查询串排序 + HMAC-SHA256"""
        query = "&".join(f"{k}={v}" for k, v in sorted(params.items()))
        return hmac.new(secret.encode(), query.encode(), hashlib.sha256).hexdigest()

    def sign_bybit(self, timestamp: str, api_key: str, payload: str, secret: str) -> str:
        """Bybit: timestamp + api_key + recv_window + payload"""
        sign_str = f"{timestamp}{api_key}5000{payload}"
        return hmac.new(secret.encode(), sign_str.encode(), hashlib.sha256).hexdigest()

    def sign_okx(self, timestamp: str, method: str, path: str, body: str, secret: str) -> str:
        """OKX: ISO8601 时间戳 + method + path + body → HMAC-SHA256 → Base64"""
        sign_str = f"{timestamp}{method}{path}{body}"
        signature = hmac.new(secret.encode(), sign_str.encode(), hashlib.sha256).digest()
        return base64.b64encode(signature).decode()
```

```go
// [伪代码] Go 风格 — 多平台 API 签名
package auth

import (
    "crypto/hmac"
    "crypto/sha256"
    "encoding/base64"
    "encoding/hex"
    "fmt"
    "sort"
    "strings"
)

func SignBinance(params map[string]string, secret string) string {
    // 查询串排序 + HMAC-SHA256 → hex
    keys := make([]string, 0)
    for k := range params { keys = append(keys, k) }
    sort.Strings(keys)
    var parts []string
    for _, k := range keys { parts = append(parts, fmt.Sprintf("%s=%s", k, params[k])) }
    query := strings.Join(parts, "&")
    mac := hmac.New(sha256.New, []byte(secret))
    mac.Write([]byte(query))
    return hex.EncodeToString(mac.Sum(nil))
}

func SignOKX(timestamp, method, path, body, secret string) string {
    // ISO8601 + method + path + body → HMAC-SHA256 → Base64
    signStr := timestamp + method + path + body
    mac := hmac.New(sha256.New, []byte(secret))
    mac.Write([]byte(signStr))
    return base64.StdEncoding.EncodeToString(mac.Sum(nil))
}
```

---

## 🌍 区域适配：全球多市场支持

`binance&bybit&okx P2P auto_payment_bot` 通过教程录制系统，理论上支持 **全球任意 Android 支付 App**。以下为已深度优化的市场：

### 支持的支付渠道（不完全列表）

| 地区 | 支付 App / 渠道 | 对接方式 |
|------|-----------------|----------|
| 🇵🇭 菲律宾 | Maya, GCash, UnionBank, BDO, BPI | 智能设备自动化 |
| 🇨🇳 中国大陆 | 支付宝 (Alipay), 微信支付 (WeChat Pay), 银行 App | 智能设备自动化 |
| 🇮🇳 印度 | PhonePe, Google Pay, Paytm, BHIM UPI | 智能设备自动化 |
| 🇮🇩 印度尼西亚 | Dana, OVO, GoPay, ShopeePay | 智能设备自动化 |
| 🇻🇳 越南 | MoMo, ZaloPay, Vietcombank | 智能设备自动化 |
| 🇹🇭 泰国 | PromptPay, TrueMoney, K PLUS | 智能设备自动化 |
| 🇰🇷 韩国 | Toss, KakaoPay | 智能设备自动化 |
| 🇭🇰 中国香港 | FPS (转数快), PayMe | 智能设备自动化 |
| 🇹🇼 中国台湾 | 银行 App, LINE Pay | 智能设备自动化 |
| 🇹🇷 土耳其 | Papara, İş Bankası | 智能设备自动化 |
| 🌐 更多市场 | 通过教程录制适配任意支付 App | 可扩展 |

### 典型应用场景

- 📈 **高频大单商户**：每日处理数十至数百笔 P2P 订单，手动付款已成瓶颈
- 🌙 **夜间无人值守**：凌晨自动处理到账订单，不错过任何交易机会
- 📱 **多设备并行**：多部手机同时运行不同支付 App，并发处理能力成倍提升
- 🔗 **完整自动化闭环**：配合 `binance_p2p_bot` 实现 挂单→竞价→成交→付款→核验→放币 全流程自动化

---

## 🌐 行业标准与生态关键词

`binance&bybit&okx P2P auto_payment_bot`, `P2P auto payment`, `Binance P2P 自动付款`, `Binance P2P 自动放币`, `Bybit P2P 自动确认收款`, `OKX P2P bot`, `P2P payment automation`, `crypto P2P release bot`, `USDT 自动放币`, `P2P order release automation`, `自动收款机器人`, `P2P payment confirmation bot`, `P2P 按单付款`, `自动转账机器人`, `币安 P2P 自动化`, `P2P merchant automation`, `crypto OTC automation`, `自动化做市`, `P2P 商户工具`, `Binance C2C automation`, `Bybit C2C bot`, `OKX C2C automation`, `P2P auto transfer bot`, `加密货币自动交易`, `Maya auto payment`, `GCash auto transfer`, `UPI auto payment`, `P2P 查账机器人`, `binance p2p bot`, `bybit p2p bot`.

---

## 💡 常见问题（Q&A）

**问：`binance&bybit&okx P2P auto_payment_bot` 是什么？解决什么问题？**
答：`binance&bybit&okx P2P auto_payment_bot` 是一款专为 Binance、Bybit、OKX P2P 商户打造的本地桌面自动化软件。它的核心能力是 **按单自动付款** —— 交易所收到买单后，自动在您的支付 App 中完成转账操作；同时支持自动查账核验与秒级放币。彻底解放商户双手，实现 7×24 小时无人值守交易。

**问：「按单自动付款」和传统的「自动放币」有什么区别？**
答：传统方案只做放币（买家声称已付款 → 核验 → 放币）。`binance&bybit&okx P2P auto_payment_bot` 在此基础上新增了 **按单自动付款**：当您收到买单时，软件自动在手机支付 App 中完成转账操作，无需人工打开 App 手动转账。这是目前市面上极少数能实现完整「付款+核验+放币」三位一体自动化的工具。

**问：软件如何防止放错币（风险控制）？**
答：多层风控保护：(1) 金额精准匹配（含可配置容差）；(2) 交易唯一 ID 全局去重（180 天保留）；(3) 超时保护（规定时间内未查到到账不放币）；(4) 余额监控（余额不足自动暂停）。所有逻辑均在本地运行，零第三方依赖。

**问：支持哪些支付 App / 渠道？**
答：理论上支持所有 Android 支付 App —— 通过教程录制系统，您可以自行适配任意 App（Maya、GCash、支付宝、微信支付、PhonePe、Dana 等）。无需编程，通过可视化界面录制操作步骤即可。详情请访问 [apip2p.top](https://apip2p.top)。

**问：软件需要在服务器上运行吗？安全吗？**
答：`binance&bybit&okx P2P auto_payment_bot` 完全在本地机器运行，零外部服务器依赖。所有 API 密钥、账户凭证均存储在本机，不经过任何第三方中转。支付 App 的密码/PIN 由本地配置自动注入，不存储在云端。**请勿为 API 密钥授予"提现"权限。**

**问：`binance&bybit&okx P2P auto_payment_bot` 和 `binance_p2p_bot` 是什么关系？**
答：两者互为补充，共同构成 P2P 商户的完整自动化工具链：
- [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot)：负责自动改价、抢排名、批量上下架广告（**盘前/盘中管理**）
- `binance&bybit&okx P2P auto_payment_bot`：负责按单付款、查账核验、自动放币（**订单结算**）

联合使用可实现从挂单→竞价→成交→付款→核验→放币的全流程自动化。

**问：重启电脑会丢失配置吗？**
答：不会。所有配置（API 密钥、设备信息、教程数据、交易记录）均实时写入本地文件，重启后自动恢复。

**问：软件使用什么技术栈？**
答：Python 构建原生桌面应用；多线程架构实现并发设备控制与订单处理；HMAC-SHA256 保障三平台 API 通信安全；本地数据库实现交易去重。无需任何外部服务器、云数据库或第三方服务。

**问：支持波动性资产（BTC、ETH）交易吗？**
答：支持。付款与核验逻辑基于法币金额运作，与底层加密资产无关。对于 BTC/ETH 等波动资产的 P2P 广告，建议配合 [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot) 的现货同步功能保持广告价格实时更新，`binance&bybit&okx P2P auto_payment_bot` 负责结算侧。

---

## 🔗 官方生态与联系方式

* **视频演示：** [🎬 在 YouTube 观看](https://www.youtube.com/watch?v=INewAPHXa5c)
* **官方主页与文档：** [apip2p.top](https://apip2p.top/) *(最新更新、P2P 自动化策略与算法交易解决方案)*
* **P2P 竞价机器人（配套工具）：** [ApiP2P-top/binance-p2p-bot](https://github.com/ApiP2P-top/binance-p2p-bot)
* **开发者 Telegram：** [@eason01993](https://t.me/eason01993)
* **社区频道：** [t.me/eason01993_p2p](https://t.me/eason01993_p2p)
* **YouTube 教程：** [Eason 的 YouTube 频道](https://www.youtube.com/@eason01993)

---

## ⚠️ 免责声明

`binance&bybit&okx P2P auto_payment_bot` 仅供教育和结构参考之用。加密货币 P2P 交易具有高度风险。请妥善保管您的 API 密钥与支付 App 凭证，切勿为自动化工具授予"提现"权限。软件不对因使用不当、网络异常或交易所 API 变更导致的任何资金损失承担责任。使用风险完全由您自行承担。
