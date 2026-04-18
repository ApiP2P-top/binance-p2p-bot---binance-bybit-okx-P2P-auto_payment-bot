<div align="center">

**🌐 Select Language / 选择语言**

[English](README.md) | [中文](README_CN.md) | [Español](README_ES.md) | [Русский](README_RU.md) | [Bahasa Indonesia](README_ID.md) | [Türkçe](README_TR.md) | [Tiếng Việt](README_VN.md) | [ภาษาไทย](README_TH.md) | [हिन्दी](README_HI.md) | [اردو](README_UR.md) | **Filipino**

</div>

---

# binance-p2p-bot：binance&bybit&okx P2P auto_payment_bot: Fully Automated Payment-by-Order · Bill Verification · Crypto Release Bot

> Propesyonal na automation tool para sa mga P2P merchant ng Binance, Bybit & OKX — **awtomatikong bayad ayon sa order**, **matalinong pag-verify ng bill**, **millisecond na pag-release ng crypto**, multi-device parallel processing

# [P2P Auto Payment & Release Bot (Binance & Bybit & OKX) Opisyal na Website](https://apip2p.top)

## 🎬 Video Demo


https://github.com/user-attachments/assets/262a41b2-cdcc-4abd-8c8e-4bdb1b566a03


[![Panoorin ang Demo](https://img.shields.io/badge/YouTube-Panoorin_ang_Demo-red?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=INewAPHXa5c)

> 📺 Panoorin ang buong video demo ng `binance&bybit&okx P2P auto_payment_bot` — mula sa pag-detect ng order hanggang awtomatikong bayad, pag-verify ng bill at pag-release ng crypto.

Solusyon sa awtomatikong pagbayad ng order na espesyal na dinisenyo para sa mga P2P merchant (advertiser). Alisin na ang paulit-ulit na manu-manong paglipat, manu-manong pag-verify at manu-manong pag-release ng crypto. Sa `binance&bybit&okx P2P auto_payment_bot`, makamit ang **buong 24/7 automation** — bayad ayon sa order, pag-verify ng deposito at instant na pag-release ng crypto.

---

## 📝 Pangkalahatang-tanaw at Arkitektura

<img width="1360" height="881" alt="2" src="https://github.com/user-attachments/assets/b2c18af8-6b0f-4062-a947-bb10e99aeed7" />


Ang `binance&bybit&okx P2P auto_payment_bot` ay isang **propesyonal na lokal na desktop automation application** na ginawa para sa mga P2P Merchant (Advertiser) sa **Binance**, **Bybit** at **OKX**.

Sa P2P trading, nahaharap ang mga merchant sa paulit-ulit na operasyon:

- ❌ **Manu-manong Pagbayad**: Buksan ang payment app, ilagay ang halaga, kumpirmahin ang transfer
- ❌ **Manu-manong Pag-verify**: Buksan ang app at tingnan kung nagbayad na ang buyer
- ❌ **Manu-manong Pag-release**: Bumalik sa exchange at pindutin ang release button
- ❌ **Gabi na Gap**: Nag-iipon ang mga order sa labas ng working hours

Nilulutas ng `binance&bybit&okx P2P auto_payment_bot` ang lahat sa pamamagitan ng **matalinong device automation + exchange API integration**:

- ✅ **🔥 Awtomatikong Pagbayad Ayon sa Order (Pangunahing Feature)**: Tumatanggap ang exchange ng buy order → awtomatikong transfer sa payment app → hindi kailangan ng intervention
- ✅ **Matalinong Pag-verify ng Bill**: Nag-mark ang buyer ng paid → awtomatikong buksan ang app para i-verify ang halaga at impormasyon ng sender
- ✅ **Millisecond na Pag-release ng Crypto**: Pagkatapos ng matagumpay na pag-verify, agad na tawagan ang exchange API
- ✅ **Multi-Device Parallel Processing**: Magkonekta ng maraming phone nang sabay-sabay
- ✅ **Multi-Exchange na Pinag-isang Pamamahala**: Binance, Bybit, OKX — isang interface
- ✅ **Buong Lokal na Operasyon**: Walang external server dependency

**Posisyon**: Ang `binance&bybit&okx P2P auto_payment_bot` ay nag-aasikaso ng **pagbayad ayon sa order + pag-verify ng bill + awtomatikong pag-release**. Pagsamahin sa [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot) para sa **end-to-end na matalinong automation**.

---

## 🚀 Mga Pangunahing Feature

### 1. 🔥 Awtomatikong Pagbayad Ayon sa Order

> **Ito ang pinakamahalagang at pinakakaibang kakayahan ng `binance&bybit&okx P2P auto_payment_bot`.**

- **Real-time na Pag-detect ng Order**: 5 segundo polling cycle
- **Matalinong Pagtatalaga ng Device**: Awtomatikong pag-match ng available na device ayon sa payment method
- **Awtomatikong Pagpapatupad ng Pagbayad**: Kumpletuhin ang buong proseso ng transfer
- **Adaptive na Payment Engine**: Awtomatikong nag-a-adapt ang matalinong protocol engine sa interface ng bawat payment app
- **Multi-Payment Method**: Isang device, maraming payment method
- **Pag-save ng Screenshot**: Awtomatikong kumuha ng ebidensya pagkatapos ng bawat transaksyon

#### 💡 Konsepto ng Implementasyon (Transfer Scheduling Engine)

```python
# [Pseudocode] Order detection → device matching → auto transfer scheduling pipeline
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
    payment_method: str
    status:         str

class TransferScheduler:
    POLL_INTERVAL = 5

    def __init__(self, exchange_clients: dict, device_pool: 'DevicePool'):
        self.exchanges   = exchange_clients
        self.device_pool = device_pool

    async def run(self):
        while True:
            for platform, client in self.exchanges.items():
                orders = await client.get_pending_orders()
                for order in orders:
                    if self._is_new_order(order):
                        await self._dispatch(order)
            await asyncio.sleep(self.POLL_INTERVAL)

    async def _dispatch(self, order) -> None:
        device = self.device_pool.find_idle_device(order.payment_method)
        if not device:
            return
        task = TransferTask(
            order_id=order.id, amount=order.fiat_amount,
            payee_name=order.counterparty, payee_account=order.account,
            payment_method=order.payment_method, status="pending",
        )
        await device.execute_transfer(task)
```

```javascript
// [Pseudocode] Node.js style — Order polling and device dispatch
class TransferScheduler {
    constructor(exchangeClients, devicePool) {
        this.exchanges  = exchangeClients;
        this.devicePool = devicePool;
        this.pollInterval = 5000;
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
        if (!device) return;
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

### 2. Matalinong Pag-verify ng Bill

- **Awtomatikong Bill Navigation**: Buksan ang app sa transaction history page
- **Tumpak na Pag-match ng Halaga**: May configurable na tolerance
- **Pag-verify ng Sender**: Cross-reference ng pangalan ng sender
- **Pagkuha ng Transaction ID**: Basahin ang Reference Number / transaction code
- **Anti-Replay na Proteksyon**: Bawat ID ay naka-record — isang bayad ay hindi puwedeng mag-trigger ng dalawang release

#### 💡 Konsepto ng Implementasyon (Deposit Verification Engine)

```go
// [Pseudocode] Go style — Deposit verification with replay-attack protection
package verifier

type BillVerifyResult struct {
    Matched     bool
    Amount      float64
    SenderName  string
    UniqueID    string
}

type BillVerifier struct {
    archive    *BillArchive
    tolerance  float64
}

func (v *BillVerifier) Verify(order P2POrder) (*BillVerifyResult, error) {
    bills, err := v.navigateToBillList(order.PaymentApp)
    if err != nil {
        return nil, err
    }
    for _, bill := range bills {
        if math.Abs(bill.Amount - order.FiatAmount) <= v.tolerance {
            detail := v.openBillDetail(bill)
            if v.archive.Exists(detail.UniqueID) {
                continue
            }
            return &BillVerifyResult{
                Matched: true, Amount: detail.Amount,
                SenderName: detail.SenderName, UniqueID: detail.UniqueID,
            }, nil
        }
    }
    return &BillVerifyResult{Matched: false}, nil
}
```

```typescript
// [Pseudocode] TypeScript style — Bill verification service
interface BillDetail {
    amount: number; senderName: string; uniqueId: string; timestamp: Date;
}

class BillVerifyService {
    private archive: BillArchive;
    private tolerance = 0.02;

    async verify(order: P2POrder): Promise<VerifyResult> {
        const bills = await this.navigateToBillList(order.paymentApp);
        const matched = bills.find(bill =>
            Math.abs(bill.amount - order.fiatAmount) <= this.tolerance
            && !this.archive.exists(bill.uniqueId)
        );
        if (!matched) return { success: false };
        this.archive.record(matched.uniqueId, order.orderId);
        return { success: true, amount: matched.amount,
            senderName: matched.senderName, uniqueId: matched.uniqueId };
    }
}
```

---

### 3. Awtomatikong Pag-release ng Crypto

- **Instant na Pag-release**: Agad na exchange API call pagkatapos ng pag-verify
- **Pag-verify ng Resulta**: Tingnan ang order status
- **Exponential na Pag-retry**: Maximum 3 ulit (2/4/8 segundo)
- **Three-Platform na Switcher**: Binance / Bybit / OKX

#### 💡 Konsepto ng Implementasyon (Cross-Platform Release Executor)

```python
# [Pseudocode] Cross-platform release executor
import asyncio, logging
logger = logging.getLogger("release_executor")

class PlatformAdapterFactory:
    @staticmethod
    def create(platform: str, credentials: dict):
        if platform == "binance": return BinanceP2PAdapter(credentials)
        elif platform == "bybit": return BybitP2PAdapter(credentials)
        elif platform == "okx": return OkxP2PAdapter(credentials)
        raise ValueError(f"Unsupported platform: {platform}")

class ReleaseExecutor:
    MAX_RETRIES = 3
    RETRY_BASE_S = 2

    async def release(self, platform: str, order_id: str, verify_result: dict) -> bool:
        adapter = PlatformAdapterFactory.create(platform, self.credentials)
        for attempt in range(self.MAX_RETRIES):
            try:
                resp = await adapter.release_order(order_id)
                if resp.success:
                    confirmed = await self._poll_completion(adapter, order_id)
                    if confirmed:
                        logger.info(f"[RELEASED] {platform} order {order_id}")
                        return True
            except Exception as e:
                wait = self.RETRY_BASE_S ** (attempt + 1)
                await asyncio.sleep(wait)
        return False
```

```rust
// [Pseudocode] Rust style — Release execution with retry
use std::time::Duration;

struct ReleaseExecutor { max_retries: u32, retry_base: Duration }

impl ReleaseExecutor {
    async fn release(&self, platform: &str, order_id: &str) -> Result<bool, Error> {
        let adapter = create_platform_adapter(platform)?;
        for attempt in 0..self.max_retries {
            match adapter.release_order(order_id).await {
                Ok(resp) if resp.success => {
                    if self.poll_completion(&adapter, order_id).await? {
                        return Ok(true);
                    }
                }
                Err(e) => {
                    let wait = self.retry_base * 2u32.pow(attempt);
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

### 4. Multi-Device Parallel Processing

- **Multi-Device na Koneksyon**: USB / WiFi para sa maraming Android phone
- **Screen Mirroring**: 30-60fps / 30-70ms latency
- **Matalinong Pagtatalaga**: Awtomatikong pag-match ng device ayon sa app
- **Independiyenteng Queue**: Bawat device ay may sariling FIFO queue
- **Status Monitoring**: Koneksyon, kasalukuyang gawain, balanse
- **Workflow Isolation**: Hiwalay na operation flow para sa iba't ibang device × app × paraan ng pagbabayad

#### 💡 Konsepto ng Implementasyon (Parallel Scheduler)

```python
# [Pseudocode] Multi-device task scheduler
from dataclasses import dataclass, field
from typing import Dict, List
from collections import deque

@dataclass
class Device:
    serial: str
    alias: str
    status: str
    supported_apps: List[str]
    task_queue: deque = field(default_factory=deque)

class DeviceScheduler:
    def __init__(self):
        self.devices: Dict[str, Device] = {}

    def find_idle_device(self, payment_method: str) -> Device | None:
        for device in self.devices.values():
            if device.status == "idle" and payment_method in device.supported_apps:
                return device
        return None

    def enqueue_task(self, task: 'TransferTask') -> bool:
        device = self.find_idle_device(task.payment_method)
        if device:
            device.task_queue.append(task)
            return True
        return False
```

```javascript
// [Pseudocode] JavaScript style — Device manager
class DeviceScheduler {
    constructor() { this.devices = new Map(); }

    findIdleDevice(paymentMethod) {
        for (const device of this.devices.values()) {
            if (device.status === 'idle' && device.supportedApps.includes(paymentMethod))
                return device;
        }
        return null;
    }

    enqueueTask(task) {
        const device = this.findIdleDevice(task.paymentMethod);
        if (device) { device.taskQueue.push(task); device.status = 'busy'; return true; }
        return false;
    }
}
```

---

### 5. Matalinong Payment Adaptation Engine

- **Zero-Config Payment Support**: Awtomatikong nakikilala at nag-a-adapt ang engine sa interface ng anumang payment app
- **Hindi Kailangan ng Coding**: Ikonekta lang ang device at i-configure ang payment method
- **Multi-Dimensional na Optimization**: Mga execution path na na-optimize per Device × App × Payment Method
- **Awtomatikong Credential Injection**: Mga password at PIN ay inilalagay mula sa lokal na naka-encrypt na config sa runtime
- **Dual-Mode na Execution**: Ang execution ng bayad (papalabas na transfer) at verification (papasok na deposit check) ay gumagamit ng independenteng pipeline

---

### 6. Kontrol sa Panganib at Pag-iwas sa Panloloko

- **Pag-filter ng Duplicate Transaction ID**: Nakapreserba ng 180 araw
- **Tumpak na Pag-match ng Halaga**: Tanggihan kung lampas sa tolerance
- **Proteksyon sa Timeout**: Hindi mag-release kung hindi makita ang deposito
- **Pagmonitor ng Balanse**: Awtomatikong huminto kung kulang ang balanse
- **Ganap na Lokal na Archive**: Lahat ng transaction record ay naka-store locally (180-araw na retention, awtomatikong cleanup)

---

### 7. Pag-save ng Estado at Pag-recover ng Session

- **Awtomatikong Pag-save**: Lahat ng configuration ay naka-save sa real-time
- **Pag-recover Pagkatapos ng Crash**: Awtomatikong i-load ang huling configuration
- **Pag-recover ng Pending Order**: I-scan ang hindi pa natapos na order sa startup

---

## �️ System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                     Desktop GUI Main Window                          │
│  ┌───────────┐ ┌────────────┐ ┌────────────┐ ┌───────────┐  │
│  │ Exchange  │ │  Device     │ │  Screen    │ │ Log &     │  │
│  │ Management│ │ Management │ │  Mirroring │ │ Audit     │  │
│  └─────┬─────┘ └─────┬──────┘ └─────┬──────┘ └───────────┘  │
│        │             │             │                             │
│  ┌────▼───────────▼───────────▼──────────────────┐    │
│  │                Signal / Slot Event Bus                   │    │
│  └────┬───────────┬───────────┬──────────────────┘    │
│       │              │              │                              │
│  ┌────▼────┐ ┌─────▼─────┐ ┌────▼─────┐ ┌────────────┐   │
│  │  Order  │ │ Transfer  │ │   Bill   │ │   State    │   │
│  │ Poller  │ │ Executor  │ │ Verifier │ │  Persist   │   │
│  │(5s cycle)│ │(payment)  │ │(deposit) │ │ (JSON/DB)  │   │
│  └──────────┘ └───────────┘ └──────────┘ └────────────┘   │
│       │              │              │                              │
│  ┌────▼───────────▼───────────▼──────────────────┐    │
│  │        Anti-Replay & Risk Control Layer              │    │
│  │  ┌───────────┐ ┌───────────┐ ┌────────────────┐   │    │
│  │  │Amount     │ │Transaction│ │Balance Monitor   │   │    │
│  │  │Tolerance  │ │ID Dedup   │ │& Timeout Guard   │   │    │
│  │  └───────────┘ └───────────┘ └────────────────┘   │    │
│  └──────────────────────────────────────────────────┘    │
└───────────────────────────┬────────────────────────────────────┘
                            │
              ┌───────────▼──────────────┐
              │  Exchange API Adapter Layer  │
              │ ┌───────┐┌──────┐┌──────┐ │
              │ │Binance││Bybit ││ OKX  │ │
              │ └───────┘└──────┘└──────┘ │
              └──────────────────────────┘
```

---

## ⚙️ Technical Specifications

| Component | Parameter | Description |
|-----------|-----------|-------------|
| Polling Cycle | 5,000 ms | Independenteng polling per exchange |
| Amount Tolerance | 0.01–0.10 | Configurable |
| Retry Interval | Exp. backoff 2/4/8s | Max 3 ulit |
| Dedup Retention | 180 araw | Awtomatikong cleanup |
| Mirroring FPS | 30–60 fps | Mababang latency (30-70ms) |
| Concurrency | Walang limitasyon | USB + WiFi |
| Workflow Profiles | Naka-encrypt na lokal na storage | Na-optimize per Device × App × Method |
| UI Themes | Dark / Light | Dalawang mode |
| UI Languages | 中文 / English | Bilingual support |

### API Authentication at Request Signing

```python
# [Pseudocode] HMAC-SHA256 Request Signing
import hashlib, hmac, base64

class MultiPlatformAuth:
    def sign_binance(self, params: dict, secret: str) -> str:
        query = "&".join(f"{k}={v}" for k, v in sorted(params.items()))
        return hmac.new(secret.encode(), query.encode(), hashlib.sha256).hexdigest()

    def sign_bybit(self, timestamp: str, api_key: str, payload: str, secret: str) -> str:
        sign_str = f"{timestamp}{api_key}5000{payload}"
        return hmac.new(secret.encode(), sign_str.encode(), hashlib.sha256).hexdigest()

    def sign_okx(self, timestamp: str, method: str, path: str, body: str, secret: str) -> str:
        sign_str = f"{timestamp}{method}{path}{body}"
        return base64.b64encode(hmac.new(secret.encode(), sign_str.encode(), hashlib.sha256).digest()).decode()
```

```go
// [Pseudocode] Go style — Multi-platform API signing
package auth
import ("crypto/hmac"; "crypto/sha256"; "encoding/base64"; "encoding/hex"; "fmt"; "sort"; "strings")

func SignBinance(params map[string]string, secret string) string {
    keys := make([]string, 0)
    for k := range params { keys = append(keys, k) }
    sort.Strings(keys)
    var parts []string
    for _, k := range keys { parts = append(parts, fmt.Sprintf("%s=%s", k, params[k])) }
    mac := hmac.New(sha256.New, []byte(secret))
    mac.Write([]byte(strings.Join(parts, "&")))
    return hex.EncodeToString(mac.Sum(nil))
}

func SignOKX(timestamp, method, path, body, secret string) string {
    mac := hmac.New(sha256.New, []byte(secret))
    mac.Write([]byte(timestamp + method + path + body))
    return base64.StdEncoding.EncodeToString(mac.Sum(nil))
}
```

---

## �🌍 Pag-angkop sa Rehiyon

Ang `binance&bybit&okx P2P auto_payment_bot` ay sumusuporta sa teorya sa **anumang Android payment app sa buong mundo**.

| Rehiyon | Payment App | Integration |
|---------|------------|-------------|
| 🇵🇭 Pilipinas | Maya, GCash, UnionBank | Matalinong Automation |
| 🇨🇳 China | Alipay, WeChat Pay | Matalinong Automation |
| 🇮🇳 India | PhonePe, Google Pay, Paytm | Matalinong Automation |
| 🇮🇩 Indonesia | Dana, OVO, GoPay | Matalinong Automation |
| 🇻🇳 Vietnam | MoMo, ZaloPay, Vietcombank | Matalinong Automation |
| 🇹🇭 Thailand | PromptPay, TrueMoney | Matalinong Automation |
| 🇹🇷 Turkey | Papara, İş Bankası | Matalinong Automation |
| �🇷 South Korea | Toss, KakaoPay | Matalinong automation |
| 🇭🇰 Hong Kong | FPS, PayMe | Matalinong automation |
| 🇹🇼 Taiwan | Bank Apps, LINE Pay | Matalinong automation |
| 🌐 Iba pang Market | Maaaring i-adapt sa pamamagitan ng matalinong device automation | Extensible |

### Mga Karaniwang Gamit

- 📈 **Merchant na Mataas ang Volume**: Nagpoproseso ng dose-dosenang hanggang daan-daang P2P order araw-araw
- 🌙 **Walang-Bantay na Gabi na Operasyon**: Awtomatikong nagpoproseso ng order sa labas ng working hours
- 📱 **Parallel na Pagpoproseso**: Maraming phone na nagpapatakbo ng iba't ibang app nang sabay-sabay
- 🔗 **Kumpletong Automation Loop**: Kasama ang `binance_p2p_bot` — mula listing hanggang crypto release

---

## 🌐 Mga Ecosystem Keyword

`binance&bybit&okx P2P auto_payment_bot`, `P2P auto payment`, `Binance P2P awtomatikong bayad`, `Bybit P2P bot`, `OKX P2P bot`, `awtomatikong bayad P2P`, `USDT awtomatikong release`, `GCash awtomatikong bayad`, `Maya auto payment`, `Binance C2C automation`, `P2P merchant automation`, `binance p2p bot`, `bybit p2p bot`

---

## 💡 Mga Madalas Itanong

**T: Ano ang `binance&bybit&okx P2P auto_payment_bot`?**
S: Propesyonal na lokal na desktop automation application para sa mga P2P merchant ng Binance, Bybit at OKX. Ang pangunahing kakayahan ay **awtomatikong pagbayad ayon sa order**. Nagbibigay-daan sa 24/7 trading nang walang intervention.

**T: Ligtas ba ito? Kailangan ba ng server?**
S: Buong lokal na operasyon. Ang API key ay naka-save lamang sa iyong kompyuter. **Huwag kailanman magbigay ng "Withdrawal" na permiso sa API key.**

**T: Ano ang relasyon sa `binance_p2p_bot`?**
S: Komplementaryo sa isa't isa:
- [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot): Awtomatikong pagpepresyo, ranking (**pre-order na pamamahala**)
- `binance&bybit&okx P2P auto_payment_bot`: Pagbayad, pag-verify, awtomatikong pag-release (**pagbayad ng order**)

**T: Ano ang pagkakaiba ng "Awtomatikong Pagbayad Ayon sa Order" at tradisyonal na "Auto Release"?**
S: Ang tradisyonal na solusyon ay ang release step lang ang inaasikaso. Ang `binance&bybit&okx P2P auto_payment_bot` ay mas malayo — **awtomatikong nagsasagawa ng transfer** sa payment app. Ito ang kumpletong trifecta: **pagbayad + pag-verify + pag-release**.

**T: Paano pinipigilan ng software ang maling pag-release (risk control)?**
S: Multi-layer na proteksyon: (1) Tumpak na pag-match ng halaga; (2) Global na deduplication ng Transaction ID (180 araw); (3) Proteksyon sa timeout; (4) Pagmonitor ng balanse.

**T: Anong mga payment app ang suportado?**
S: Sa teorya, lahat ng Android app — sa pamamagitan ng matalinong adaptation engine. I-configure lang ang payment method. Mga detalye sa [apip2p.top](https://apip2p.top).

**T: Mawawala ba ang configuration kapag nag-restart?**
S: Hindi. Lahat ay naka-save locally sa real-time. Awtomatikong bina-restore sa startup.

**T: Anong technology stack ang ginagamit?**
S: Python para sa native desktop experience. Multi-threaded architecture. HMAC-SHA256 para sa API. Lokal na database.

**T: Sumusuporta ba sa mga volatile asset (BTC, ETH)?**
S: Oo. Ang logic ng pagbayad ay gumagana sa fiat amount. Para sa BTC/ETH, inirerekomenda ang Spot Market Sync ng [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot).

---

## 🔗 Ecosystem at Kontak

* **Video Demo:** [🎬 Panoorin sa YouTube](https://www.youtube.com/watch?v=INewAPHXa5c)
* **Opisyal na Website:** [apip2p.top](https://apip2p.top/)
* **Kasamang P2P Bot:** [ApiP2P-top/binance-p2p-bot](https://github.com/ApiP2P-top/binance-p2p-bot)
* **Developer Telegram:** [@eason01993](https://t.me/eason01993)
* **Community Channel:** [t.me/eason01993_p2p](https://t.me/eason01993_p2p)
* **YouTube:** [YouTube Channel ni Eason](https://www.youtube.com/@eason01993)

---

## ⚠️ Disclaimer

Ang `binance&bybit&okx P2P auto_payment_bot` ay ibinibigay para sa educational at reference na layunin. Ang P2P crypto trading ay may malaking panganib. Panatilihing ligtas ang API key. Huwag kailanman magbigay ng "Withdrawal" na permiso sa mga automation tool. Ang software ay hindi responsable sa anumang pagkalugi sa pananalapi. Gamitin sa sarili mong peligro.
