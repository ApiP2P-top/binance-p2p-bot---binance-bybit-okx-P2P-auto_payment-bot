<div align="center">

**🌐 Select Language / 选择语言**

[English](README.md) | [中文](README_CN.md) | [Español](README_ES.md) | [Русский](README_RU.md) | [Bahasa Indonesia](README_ID.md) | **Türkçe** | [Tiếng Việt](README_VN.md) | [ภาษาไทย](README_TH.md) | [हिन्दी](README_HI.md) | [اردو](README_UR.md) | [Filipino](README_PH.md)

</div>

---

# binance&bybit&okx P2P auto_payment_bot: Sipariş Bazlı Tam Otomatik Ödeme · Fatura Doğrulama · Kripto Serbest Bırakma Botu

> Binance, Bybit ve OKX P2P tüccarları için profesyonel otomasyon motoru — **sipariş bazlı otomatik ödeme**, **akıllı fatura doğrulama**, **milisaniye kripto serbest bırakma**, çoklu cihaz paralel işleme.

# [P2P Otomatik Ödeme ve Serbest Bırakma Botu (Binance & Bybit & OKX) Resmi Site](https://apip2p.top)

## 🎬 Video Demo


https://github.com/user-attachments/assets/262a41b2-cdcc-4abd-8c8e-4bdb1b566a03


[![Demoyu İzle](https://img.shields.io/badge/YouTube-Demoyu_İzle-red?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=INewAPHXa5c)

> 📺 `binance&bybit&okx P2P auto_payment_bot`'un tam demo videosunu izleyin — sipariş tespitinden otomatik ödemeye, fatura doğrulamaya ve kripto serbest bırakma adımlarına kadar.

P2P tüccarları (reklamverenler) için tasarlanmış nihai otomatik sipariş çözüm sistemi. Manuel transferlerin, manuel doğrulamanın ve manuel kripto serbest bırakmanın yorucu döngüsünü ortadan kaldırın. `binance&bybit&okx P2P auto_payment_bot` ile **7/24 tam otomatik** P2P sipariş bazlı ödeme, depozito doğrulama ve anlık kripto serbest bırakma elde edin.

---

## 📝 Genel Bakış ve Mimari

<img width="1360" height="881" alt="2" src="https://github.com/user-attachments/assets/b2c18af8-6b0f-4062-a947-bb10e99aeed7" />


`binance&bybit&okx P2P auto_payment_bot`, **Binance**, **Bybit** ve **OKX** platformlarında faaliyet gösteren P2P Tüccarları için özel olarak oluşturulmuş profesyonel bir **yerel masaüstü otomasyon istemcisidir**.

P2P ticaretinde tüccarlar büyük tekrarlayan operasyonlarla karşılaşır:

- ❌ **Manuel Ödemeler**: Her satın alma siparişi için ödeme uygulamasını açıp transfer yapmak
- ❌ **Manuel Doğrulama**: Alıcının gerçekten ödeme yapıp yapmadığını kontrol etmek
- ❌ **Manuel Serbest Bırakma**: Doğrulamadan sonra borsaya dönüp serbest bırakma düğmesine tıklamak
- ❌ **Gece Boşlukları**: Mesai dışı saatlerde siparişlerin birikmesi

`binance&bybit&okx P2P auto_payment_bot` tüm bunları **akıllı cihaz otomasyonu + borsa API entegrasyonu** ile çözer:

- ✅ **🔥 Sipariş Bazlı Otomatik Ödeme (Ana Özellik)**: Borsa sipariş alır → ödeme uygulamanızda transferi otomatik tamamlar → sıfır insan müdahalesi
- ✅ **Akıllı Fatura Doğrulama**: Alıcı ödediğini işaretler → uygulamanızı otomatik açarak tutarı ve gönderen bilgisini doğrular
- ✅ **Milisaniye Kripto Serbest Bırakma**: Doğrulama sonrası anında borsa API'sini çağırır
- ✅ **Çoklu Cihaz Paralel İşleme**: Birden fazla telefonu aynı anda bağlayın
- ✅ **Çoklu Borsa Birleşik Yönetim**: Binance, Bybit, OKX — tek arayüz
- ✅ **Tamamen Yerel Çalışma**: Harici sunucu bağımlılığı yok

**Konumlandırma**: `binance&bybit&okx P2P auto_payment_bot` **sipariş bazlı ödeme + fatura doğrulama + otomatik serbest bırakma** işlemlerini yönetir. [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot) ile birlikte **uçtan uca tam otomasyon** sağlar.

---

## 🚀 Temel Özellikler

### 1. 🔥 Sipariş Bazlı Otomatik Ödeme

> **`binance&bybit&okx P2P auto_payment_bot`'un en kritik ve ayırt edici özelliğidir.**

- **Gerçek Zamanlı Sipariş Tespiti**: 5 saniye yoklama döngüsü
- **Akıllı Cihaz Dağıtımı**: Ödeme yöntemine göre boşta olan cihazı eşleştirir
- **Otomatik Ödeme Yürütme**: Telefondaki ödeme uygulamasında tam transfer akışını tamamlar
- **Uyarlanabilir Ödeme Motoru**: Akıllı protokol motoru her ödeme uygulamasının arayüzüne otonom olarak uyum sağlar
- **Çoklu Ödeme Yöntemi Desteği**: Tek cihaz, birden fazla yöntem
- **Transfer Ekran Görüntüsü Arşivi**: Her transferden sonra kanıt yakalar

#### 💡 Uygulama Konsepti (Transfer Planlama Motoru)

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

### 2. Akıllı Fatura Doğrulama

- **Otomatik Fatura Navigasyonu**: Ödeme uygulamasını açar ve işlem geçmişine gider
- **Hassas Tutar Eşleştirme**: Yapılandırılabilir tolerans ile
- **Gönderen Doğrulama**: Gönderen adını çapraz doğrular
- **Benzersiz İşlem ID Çıkarma**: Reference Number'ı okur
- **Anti-Replay Koruması**: Her ID kaydedilir — aynı depozito iki serbest bırakma tetikleyemez

#### 💡 Uygulama Konsepti (Depozit Doğrulama Motoru)

```go
// [Pseudocode] Go style — Deposit verification with replay-attack protection
package verifier

type BillVerifyResult struct {
    Matched bool; Amount float64; SenderName string; UniqueID string
}

type BillVerifier struct {
    archive *BillArchive; tolerance float64
}

func (v *BillVerifier) Verify(order P2POrder) (*BillVerifyResult, error) {
    bills, err := v.navigateToBillList(order.PaymentApp)
    if err != nil { return nil, err }
    for _, bill := range bills {
        if math.Abs(bill.Amount - order.FiatAmount) <= v.tolerance {
            detail := v.openBillDetail(bill)
            if v.archive.Exists(detail.UniqueID) { continue }
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

### 3. Otomatik Kripto Serbest Bırakma

- **Anında Serbest Bırakma**: Doğrulama sonrası API'yi çağırır
- **Sonuç Doğrulama**: Sipariş durumunu sorgular
- **Üstel Geri Çekilme ile Yeniden Deneme**: 3 denemeye kadar
- **Üç Platform Birleşik Adaptör**: Binance / Bybit / OKX

#### 💡 Uygulama Konsepti (Çapraz Platform Serbest Bırakma Yürütücüsü)

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

### 4. Çoklu Cihaz Paralel İşleme

- **Çoklu Bağlantı**: USB / WiFi ile birden fazla Android telefon
- **Gerçek Zamanlı Ekran Yansıtma**: 30-60fps / 30-70ms gecikme
- **Akıllı Görev Atama**: Otomatik cihaz eşleştirme
- **Bağımsız Görev Kuyrukları**: Her cihaz kendi FIFO kuyruğu
- **Durum İzleme**: Bağlantı durumu, mevcut görev, bakiye
- **İş Akışı İzolasyonu**: Farklı cihazlar × uygulamalar × ödeme yöntemleri için ayrı operasyon akışları

#### 💡 Uygulama Konsepti (Paralel Planlayıcı)

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

### 5. Akıllı Ödeme Uyum Motoru

- **Yapılandırma Gerektirmez**: Motor, herhangi bir ödeme uygulamasının arayüzünü otomatik olarak tanır ve uyum sağlar
- **Programlama Gerektirmez**: Cihazınızı bağlayın ve ödeme yöntemini yapılandırın — gerisini motor halleder
- **Çok Boyutlu Optimizasyon**: Cihaz × Uygulama × Ödeme Yöntemi başına optimize edilmiş yürütme yolları
- **Otomatik Kimlik Bilgisi Enjeksiyonu**: Şifreler ve PIN'ler çalışma zamanında yerel şifreli yapılandırmadan enjekte edilir
- **Çift Mod Yürütme**: Ödeme yürütme (giden transferler) ve doğrulama (gelen depozit kontrolleri) bağımsız pipeline'lar kullanır

---

### 6. Risk Kontrolü ve Dolandırıcılık Koruması

- **İşlem ID Çoğaltma Önleme**: 180 gün saklama
- **Hassas Tutar Eşleştirme**: Tolerans aşılırsa reddet
- **Zaman Aşımı Koruması**: Depozito bulunamazsa serbest bırakma yok
- **Bakiye İzleme**: Yetersiz bakiyede otomatik duraklama
- **Tamamen Yerel Arşiv**: Tüm işlem kayıtları yerel olarak saklanır (180 gün saklama, otomatik temizleme)

---

### 7. Durum Kalıcılığı ve Oturum Kurtarma

- **Otomatik Kaydetme**: Tüm yapılandırmalar gerçek zamanlı kaydedilir
- **Çökme Kurtarma**: Yeniden başlatma sonrası otomatik geri yükleme
- **Bekleyen Sipariş Kurtarma**: Başlangıçta tamamlanmamış siparişleri tarar

---

## �️ Sistem Mimarisi

```
┌─────────────────────────────────────────────────────────────────┐
│                     Masaüstü GUI Ana Penceresi                       │
│  ┌───────────┐ ┌────────────┐ ┌────────────┐ ┌───────────┐  │
│  │ Borsa     │ │ Cihaz        │ │  Yansıtma   │ │ Log &     │  │
│  │ Yönetimi  │ │ Yönetimi     │ │  (Ekran)   │ │ Denetim   │  │
│  └─────┬─────┘ └─────┬──────┘ └─────┬──────┘ └───────────┘  │
│        │             │             │                             │
│  ┌────▼───────────▼───────────▼──────────────────┐    │
│  │                Signal / Slot Event Bus                   │    │
│  └────┬───────────┬───────────┬──────────────────┘    │
│       │              │              │                              │
│  ┌────▼────┐ ┌─────▼─────┐ ┌────▼─────┐ ┌────────────┐   │
│  │  Sipariş  │ │ Transfer    │ │Fatura      │ │ Durum       │   │
│  │  Poller   │ │ Yürütücü   │ │Doğrulama   │ │ Kalıcılık   │   │
│  │(5s döngü) │ │(ödeme per  │ │(depozit    │ │ (JSON/DB)   │   │
│  │          │ │ sipariş)   │ │ doğrulama)│ │             │   │
│  └──────────┘ └───────────┘ └──────────┘ └────────────┘   │
│       │              │              │                              │
│  ┌────▼───────────▼───────────▼──────────────────┐    │
│  │        Anti-Replay & Risk Kontrol Katmanı              │    │
│  │  ┌───────────┐ ┌───────────┐ ┌────────────────┐   │    │
│  │  │Tutar       │ │İşlem ID   │ │Bakiye İzleme    │   │    │
│  │  │Toleransı   │ │Deduplikasy│ │& Zaman Aşımı   │   │    │
│  │  └───────────┘ └───────────┘ └────────────────┘   │    │
│  └──────────────────────────────────────────────────┘    │
└───────────────────────────┬────────────────────────────────────┘
                            │
              ┌───────────▼──────────────┐
              │  Exchange API Adaptör Katmanı  │
              │ ┌───────┐┌──────┐┌──────┐ │
              │ │Binance││Bybit ││ OKX  │ │
              │ └───────┘└──────┘└──────┘ │
              └──────────────────────────┘
```

---

## ⚙️ Teknik Özellikler

| Bileşen | Parametre | Açıklama |
|---------|-----------|----------|
| Sipariş Yoklama Döngüsü | 5.000 ms | Exchange başına bağımsız yoklama |
| Tutar Eşleşme Toleransı | 0.01–0.10 | Yapılandırılabilir |
| Serbest Bırakma Yeniden Deneme | Üssel geri çekilme 2/4/8s | Maks. 3 deneme |
| Deduplikasyon Saklama | 180 gün | Otomatik temizleme |
| Ekran Yansıtma FPS | 30–60 fps | Düşük gecikme (30-70ms) |
| Çoklu Cihaz Eşzamanlılık | Sınırsız | USB + WiFi |
| İş Akışı Profilleri | Şifreli yerel depolama | Cihaz × Uygulama × Yöntem başına optimizasyon |
| UI Temaları | Koyu / Açık | İki mod |
| UI Dilleri | 中文 / English | İki dil desteği |

### API Kimlik Doğrulama ve İstek İmzalama

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

## �🌍 Bölgesel Uyum

`binance&bybit&okx P2P auto_payment_bot` teorik olarak **dünya çapında herhangi bir Android ödeme uygulamasını** destekler.

| Bölge | Ödeme Uygulaması | Entegrasyon |
|-------|-----------------|-------------|
| 🇵🇭 Filipinler | Maya, GCash, UnionBank | Akıllı otomasyon |
| 🇨🇳 Çin | Alipay, WeChat Pay | Akıllı otomasyon |
| 🇮🇳 Hindistan | PhonePe, Google Pay, Paytm | Akıllı otomasyon |
| 🇮🇩 Endonezya | Dana, OVO, GoPay | Akıllı otomasyon |
| 🇻🇳 Vietnam | MoMo, ZaloPay | Akıllı otomasyon |
| 🇹🇭 Tayland | PromptPay, TrueMoney | Akıllı otomasyon |
| 🇹🇷 Türkiye | Papara, İş Bankası, Ziraat | Akıllı otomasyon |
| �🇷 Güney Kore | Toss, KakaoPay | Akıllı otomasyon |
| 🇭🇰 Hong Kong | FPS, PayMe | Akıllı otomasyon |
| 🇹🇼 Tayvan | Banka Uygulamaları, LINE Pay | Akıllı otomasyon |
| 🌐 Diğer Pazarlar | Akıllı cihaz otomasyonu ile uyarlanabilir | Genişletilebilir |

### Tipik Kullanım Senaryoları

- 📈 **Yüksek Hacimli Tüccarlar**: Günde onlarca-yüzlerce P2P siparişi işleyen tüccarlar için
- 🌙 **Gece Gözetimsiz Çalışma**: Mesai dışı saatlerde otomatik sipariş işleme
- 📱 **Çoklu Cihaz Paralel İşleme**: Birden fazla telefon farklı ödeme uygulamalarını aynı anda çalıştırır
- 🔗 **Tam Otomasyon Döngüsü**: `binance_p2p_bot` ile birlikte — listeleme → teklif → sipariş → ödeme → doğrulama → serbest bırakma

---

## 🌐 Ekosistem Anahtar Kelimeler

`binance&bybit&okx P2P auto_payment_bot`, `P2P auto payment`, `Binance P2P otomatik ödeme`, `Bybit P2P bot`, `OKX P2P bot`, `P2P ödeme otomasyonu`, `kripto P2P serbest bırakma`, `USDT otomatik serbest bırakma`, `Papara otomatik ödeme`, `Binance C2C otomasyon`, `P2P tüccar otomasyonu`, `binance p2p bot`, `bybit p2p bot`.

---

## 💡 Sıkça Sorulan Sorular

**S: `binance&bybit&okx P2P auto_payment_bot` nedir?**
C: Binance, Bybit ve OKX P2P tüccarları için profesyonel yerel masaüstü otomasyon istemcisidir. **Sipariş bazlı otomatik ödeme** ana özelliğidir. 7/24 eller serbest ticaret sağlar.

**S: Güvenli mi? Sunucu gerekli mi?**
C: Tamamen yerel çalışır. API anahtarları yalnızca bilgisayarınızda saklanır. **API anahtarlarınıza asla "Çekim" izni vermeyin.**

**S: `binance_p2p_bot` ile ilişkisi nedir?**
C: Birbirini tamamlarlar:
- [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot): Otomatik fiyatlandırma, sıralama (**sipariş öncesi yönetim**)
- `binance&bybit&okx P2P auto_payment_bot`: Ödeme, doğrulama, serbest bırakma (**sipariş çözümü**)

**S: "Sipariş Bazlı Otomatik Ödeme" ile geleneksel "Otomatik Serbest Bırakma" arasındaki fark nedir?**
C: Geleneksel çözümler yalnızca serbest bırakma adımını ele alır. `binance&bybit&okx P2P auto_payment_bot` daha ileri gider — **otomatik transfer** gerçekleştirir. Tam üçlü: **ödeme + doğrulama + serbest bırakma**.

**S: Yazılım yanlış serbest bırakmaları nasıl önler (risk kontrolü)?**
C: Çok katmanlı koruma: (1) Hassas tutar eşleştirme; (2) İşlem ID global deduplikasyonu (180 gün); (3) Zaman aşımı koruması; (4) Bakiye izleme.

**S: Hangi ödeme uygulamaları / kanalları destekleniyor?**
C: Teorik olarak herhangi bir Android ödeme uygulaması — akıllı uyum motoru sayesinde. Ödeme yönteminizi yapılandırın, motor gerisini halleder. Detaylar için [apip2p.top](https://apip2p.top).

**S: Bilgisayarı yeniden başlatırsam yapılandırmam kaybolur mu?**
C: Hayır. Tüm yapılandırmalar gerçek zamanlı olarak yerel dosyalara kaydedilir. Bot başlangıçta otomatik olarak geri yükler.

**S: `binance&bybit&okx P2P auto_payment_bot` hangi teknoloji yığınını kullanıyor?**
C: Python ile yerel masaüstü deneyimi için oluşturulmuştur. Çok iş parçacıklı mimari. HMAC-SHA256 ile API iletişimi. Yerel veritabanı. Harici sunucu gerekmez.

**S: Bot dalgalı varlıkları (BTC, ETH) destekliyor mu?**
C: Evet. Ödeme ve doğrulama mantığı fiat tutarları üzerinden çalışır. BTC/ETH için [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot)'un Spot Piyasa Senkronizasyonu özelliklerini kullanmanızı öneririz.
---

## 🔗 Resmi Ekosistem ve İletişim
* **Video Demo:** [🎬 YouTube'da İzle](https://www.youtube.com/watch?v=INewAPHXa5c)
* **Resmi Site:** [apip2p.top](https://apip2p.top/)
* **P2P Eşlik Botu:** [ApiP2P-top/binance-p2p-bot](https://github.com/ApiP2P-top/binance-p2p-bot)
* **Geliştirici Telegram:** [@eason01993](https://t.me/eason01993)
* **Topluluk Kanalı:** [t.me/eason01993_p2p](https://t.me/eason01993_p2p)
* **YouTube:** [Eason'ın YouTube Kanalı](https://www.youtube.com/@eason01993)

---

## ⚠️ Sorumluluk Reddi

`binance&bybit&okx P2P auto_payment_bot` eğitim ve yapısal referans amaçlıdır. Kripto P2P ticareti önemli risk taşır. API anahtarlarınızı güvenle saklayın. Otomatik araçlara asla "Çekim" izni vermeyin. Yazılım mali kayıplardan sorumlu değildir. Tamamen kendi riskinizle kullanın.
