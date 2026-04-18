<div align="center">

**🌐 Select Language / 选择语言**

[English](README.md) | [中文](README_CN.md) | [Español](README_ES.md) | [Русский](README_RU.md) | **Bahasa Indonesia** | [Türkçe](README_TR.md) | [Tiếng Việt](README_VN.md) | [ภาษาไทย](README_TH.md) | [हिन्दी](README_HI.md) | [اردو](README_UR.md) | [Filipino](README_PH.md)

</div>

---

# binance&bybit&okx P2P auto_payment_bot: Bot Pembayaran Otomatis per Pesanan · Verifikasi Tagihan · Pelepasan Crypto

> Mesin otomatisasi profesional untuk merchant P2P Binance, Bybit & OKX — **pembayaran otomatis per pesanan**, **verifikasi tagihan cerdas**, **pelepasan crypto dalam milidetik**, dengan pemrosesan paralel multi-perangkat.

# [Bot Pembayaran & Pelepasan Otomatis P2P (Binance & Bybit & OKX) Situs Resmi](https://apip2p.top)

## 🎬 Demo Video


https://github.com/user-attachments/assets/262a41b2-cdcc-4abd-8c8e-4bdb1b566a03


[![Tonton Demo](https://img.shields.io/badge/YouTube-Tonton_Demo-red?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=INewAPHXa5c)

> 📺 Tonton demo lengkap `binance&bybit&okx P2P auto_payment_bot` beraksi — dari deteksi pesanan hingga pembayaran otomatis, verifikasi tagihan, dan pelepasan crypto.

Solusi penyelesaian pesanan otomatis terbaik untuk merchant P2P (pengiklan). Hilangkan siklus transfer manual, verifikasi manual, dan pelepasan crypto manual. Dengan `binance&bybit&okx P2P auto_payment_bot`, capai **otomatisasi penuh 24/7** — pembayaran per pesanan, verifikasi deposit, dan pelepasan crypto instan.

---

## 📝 Tinjauan & Arsitektur
<img width="1360" height="881" alt="2" src="https://github.com/user-attachments/assets/b2c18af8-6b0f-4062-a947-bb10e99aeed7" />


`binance&bybit&okx P2P auto_payment_bot` adalah **klien desktop otomatisasi lokal** profesional yang dibuat khusus untuk Merchant P2P (Pengiklan) di **Binance**, **Bybit**, dan **OKX**.

Dalam perdagangan P2P, merchant menghadapi operasi repetitif yang masif:

- ❌ **Pembayaran Manual**: Untuk setiap pesanan beli, merchant harus membuka aplikasi pembayaran, memasukkan jumlah dan penerima
- ❌ **Verifikasi Manual**: Untuk setiap pesanan jual, membuka aplikasi untuk memverifikasi pembayaran
- ❌ **Pelepasan Manual**: Setelah verifikasi, kembali ke exchange untuk mengklik tombol lepas
- ❌ **Jeda Malam**: Saat jam tidak aktif, pesanan menumpuk tanpa diproses

`binance&bybit&okx P2P auto_payment_bot` menyelesaikan semua ini melalui **otomatisasi perangkat cerdas + integrasi API exchange**:

- ✅ **🔥 Pembayaran Otomatis per Pesanan (Fitur Inti)**: Exchange menerima pesanan beli → otomatis menyelesaikan transfer di aplikasi pembayaran Anda → tanpa campur tangan manusia
- ✅ **Verifikasi Tagihan Cerdas**: Pembeli menandai pembayaran → otomatis membuka aplikasi untuk memverifikasi jumlah dan info pengirim
- ✅ **Pelepasan Crypto Milidetik**: Setelah verifikasi berhasil, langsung memanggil API exchange
- ✅ **Pemrosesan Paralel Multi-Perangkat**: Hubungkan beberapa ponsel secara bersamaan
- ✅ **Manajemen Terpadu Multi-Exchange**: Binance, Bybit, OKX — tiga platform dalam satu antarmuka
- ✅ **Operasi Sepenuhnya Lokal**: Tanpa dependensi server eksternal

**Posisi**: `binance&bybit&okx P2P auto_payment_bot` menangani **pembayaran per pesanan + verifikasi tagihan + pelepasan otomatis**. Dikombinasikan dengan [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot) untuk **otomatisasi cerdas end-to-end**.

---

## 🚀 Fitur Inti (Algoritma & Alur Kerja)

### 1. 🔥 Pembayaran Otomatis per Pesanan (Mesin Transfer Otomatis)

> **Ini adalah kemampuan paling kritis dan pembeda dari `binance&bybit&okx P2P auto_payment_bot`.**

- **Deteksi Pesanan Real-time**: Polling 5 detik mendeteksi pesanan baru
- **Dispatch Perangkat Cerdas**: Otomatis mencocokkan perangkat idle berdasarkan metode pembayaran
- **Eksekusi Pembayaran Otomatis**: Menyelesaikan seluruh alur transfer tanpa campur tangan manusia
- **Mesin Pembayaran Adaptif**: Mesin protokol cerdas beradaptasi secara otonom dengan antarmuka setiap aplikasi pembayaran
- **Dukungan Multi-Metode Pembayaran**: Satu perangkat dapat dikonfigurasi dengan beberapa metode
- **Arsip Screenshot Transfer**: Otomatis menangkap bukti setelah setiap transfer

#### 💡 Konsep Implementasi (Mesin Penjadwalan Transfer)

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

### 2. Verifikasi Tagihan Cerdas (Mesin Verifikasi Deposit)

- **Navigasi Tagihan Otomatis**: Membuka aplikasi dan menuju ke halaman riwayat transaksi
- **Pencocokan Jumlah Presisi**: Dengan toleransi yang dapat dikonfigurasi
- **Verifikasi Pengirim**: Validasi silang nama pengirim
- **Ekstraksi ID Transaksi Unik**: Membaca Reference Number sebagai pengenal unik
- **Perlindungan Anti-Replay**: Setiap ID dicatat — deposit yang sama tidak bisa memicu dua pelepasan

#### 💡 Konsep Implementasi (Mesin Verifikasi Deposit)

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

### 3. Eksekusi Pelepasan Crypto Otomatis

- **Pelepasan Instan**: Langsung memanggil API P2P exchange
- **Verifikasi Hasil**: Polling status pesanan untuk konfirmasi
- **Retry dengan Exponential Backoff**: Hingga 3 kali (2/4/8 detik)
- **Adapter Terpadu Tiga Platform**: Binance C2C SAPI / Bybit V5 P2P / OKX V5 P2P

#### 💡 Konsep Implementasi (Eksekutor Pelepasan Multi-Platform)

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

### 4. Pemrosesan Paralel Multi-Perangkat

- **Koneksi Multi-Perangkat**: USB / WiFi untuk beberapa ponsel Android
- **Mirroring Layar Real-time**: 30-60fps / 30-70ms latensi
- **Penugasan Tugas Cerdas**: Pencocokan perangkat otomatis
- **Antrian Tugas Independen**: Setiap perangkat memiliki antrian FIFO sendiri
- **Pemantauan Status**: Koneksi, tugas saat ini, saldo
- **Isolasi Alur Kerja**: Alur operasi terpisah untuk perangkat × aplikasi × metode pembayaran berbeda

#### 💡 Konsep Implementasi (Penjadwal Paralel Multi-Perangkat)

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

### 5. Mesin Adaptasi Pembayaran Cerdas

- **Dukungan Pembayaran Tanpa Konfigurasi**: Mesin secara otomatis mengenali dan beradaptasi dengan antarmuka aplikasi pembayaran apa pun melalui algoritma pengenalan visual
- **Tanpa Pemrograman**: Cukup hubungkan perangkat dan konfigurasikan metode pembayaran
- **Optimasi Multi-Dimensi**: Jalur eksekusi dioptimalkan per Perangkat × Aplikasi × Metode pembayaran
- **Kompatibilitas Antar Perangkat**: Profil pembayaran portabel antar perangkat dengan konfigurasi serupa
- **Injeksi Kredensial Otomatis**: Password dan PIN diinjeksi dari konfigurasi terenkripsi lokal saat runtime
- **Eksekusi Dual-Mode**: Eksekusi pembayaran (transfer keluar) dan verifikasi (cek deposit masuk) menggunakan pipeline independen

---

### 6. Kontrol Risiko & Perlindungan Anti-Penipuan

- **Deduplikasi ID Transaksi**: Retensi 180 hari
- **Pencocokan Jumlah Presisi**: Tolak jika melebihi toleransi
- **Perlindungan Timeout**: Tidak ada pelepasan tanpa deposit yang dikonfirmasi
- **Pemantauan Saldo**: Jeda otomatis saat saldo tidak cukup
- **Arsip Sepenuhnya Lokal**: Semua catatan transaksi disimpan lokal (retensi 180 hari dengan pembersihan otomatis)

---

### 7. Persistensi Status & Pemulihan Sesi

- **Penyimpanan Otomatis**: Semua konfigurasi disimpan real-time
- **Pemulihan Crash**: Muat konfigurasi terakhir otomatis
- **Pemulihan Pesanan Pending**: Scan pesanan yang belum selesai saat startup

---

## 🏗️ Arsitektur Sistem

```
┌─────────────────────────────────────────────────────────────────────┐
│                     Jendela Utama GUI Desktop                       │
│  ┌───────────┐ ┌────────────┐ ┌────────────┐ ┌───────────┐  │
│  │ Manajemen │ │ Manajemen  │ │  Mirroring  │ │ Log &     │  │
│  │ Exchange  │ │ Perangkat  │ │  Layar     │ │ Audit     │  │
│  └─────┬─────┘ └─────┬──────┘ └─────┬──────┘ └───────────┘  │
│        │             │             │                             │
│  ┌────▼───────────▼───────────▼──────────────────┐    │
│  │                Signal / Slot Event Bus                   │    │
│  └────┬───────────┬───────────┬──────────────────┘    │
│       │              │              │                              │
│  ┌────▼────┐ ┌─────▼─────┐ ┌────▼─────┐ ┌────────────┐   │
│  │  Poller  │ │ Eksekutor │ │Verifikator│ │ Persistensi│   │
│  │ Pesanan  │ │ Transfer  │ │ Tagihan  │ │  Status    │   │
│  │(5s siklus)│ │(bayar per │ │(verifikasi│ │ (JSON/DB)  │   │
│  │          │ │ pesanan)  │ │ deposit) │ │            │   │
│  └──────────┘ └───────────┘ └──────────┘ └────────────┘   │
│       │              │              │                              │
│  ┌────▼───────────▼───────────▼──────────────────┐    │
│  │        Anti-Replay & Kontrol Risiko Layer              │    │
│  │  ┌───────────┐ ┌───────────┐ ┌────────────────┐   │    │
│  │  │Toleransi  │ │Deduplikasi│ │Monitor Saldo     │   │    │
│  │  │Jumlah     │ │ID Trans.  │ │& Guard Timeout   │   │    │
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

## ⚙️ Spesifikasi Teknis

| Komponen | Parameter | Deskripsi |
|----------|-----------|----------|
| Siklus Polling | 5.000 ms | Polling independen per exchange |
| Toleransi Jumlah | 0.01–0.10 | Dapat dikonfigurasi |
| Interval Retry | Exp. backoff 2/4/8s | Maks. 3 kali |
| Retensi Dedup | 180 hari | Pembersihan otomatis |
| FPS Mirroring | 30–60 fps | Latensi rendah (30-70ms) |
| Konkurensi | Tidak terbatas | USB + WiFi |
| Profil Pembayaran | Penyimpanan lokal terenkripsi | Optimasi per Perangkat × App × Metode |
| Tema UI | Gelap / Terang | Dua mode |
| Bahasa UI | 中文 / English | Dukungan dwibahasa |

### Autentikasi API & Penandatanganan Request

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

## 🌍 Adaptasi Regional

`binance&bybit&okx P2P auto_payment_bot` secara teoritis mendukung **aplikasi pembayaran Android apa pun di seluruh dunia**.

| Wilayah | Aplikasi Pembayaran | Integrasi |
|---------|-------------------|-----------|
| 🇵🇭 Filipina | Maya, GCash, UnionBank, BDO, BPI | Otomatisasi cerdas |
| 🇨🇳 Tiongkok | Alipay, WeChat Pay, Aplikasi Bank | Otomatisasi cerdas |
| 🇮🇳 India | PhonePe, Google Pay, Paytm, BHIM UPI | Otomatisasi cerdas |
| 🇮🇩 Indonesia | Dana, OVO, GoPay, ShopeePay | Otomatisasi cerdas |
| 🇻🇳 Vietnam | MoMo, ZaloPay, Vietcombank | Otomatisasi cerdas |
| 🇹🇭 Thailand | PromptPay, TrueMoney, K PLUS | Otomatisasi cerdas |
| 🇹🇷 Turki | Papara, İş Bankası | Otomatisasi cerdas |
| �🇷 Korea Selatan | Toss, KakaoPay | Otomatisasi cerdas |
| 🇭🇰 Hong Kong | FPS, PayMe | Otomatisasi cerdas |
| 🇹🇼 Taiwan | Aplikasi Bank, LINE Pay | Otomatisasi cerdas |
| 🌐 Pasar Lainnya | Dapat diadaptasi melalui otomatisasi perangkat cerdas | Dapat diperluas |

### Kasus Penggunaan Tipikal

- 📈 **Merchant Volume Tinggi**: Memproses puluhan-ratusan pesanan P2P per hari
- 🌙 **Operasi Malam Tanpa Pengawasan**: Memproses pesanan otomatis di luar jam kerja
- 📱 **Pemrosesan Paralel**: Beberapa ponsel menjalankan aplikasi berbeda secara bersamaan
- 🔗 **Siklus Otomatisasi Lengkap**: Dengan `binance_p2p_bot` — dari listing hingga pelepasan crypto

---

## 🌐 Kata Kunci Ekosistem

`binance&bybit&okx P2P auto_payment_bot`, `P2P auto payment`, `Binance P2P auto release`, `Bybit P2P bot`, `OKX P2P bot`, `P2P payment automation`, `crypto P2P release bot`, `USDT auto release`, `P2P payment-by-order`, `Binance C2C automation`, `P2P merchant automation`, `Dana auto payment`, `OVO auto transfer`, `GoPay auto payment`, `binance p2p bot`, `bybit p2p bot`.

---

## 💡 FAQ

**T: Apa itu `binance&bybit&okx P2P auto_payment_bot`?**
J: Klien desktop otomatisasi lokal profesional untuk merchant P2P Binance, Bybit, dan OKX. Kemampuan inti adalah **pembayaran otomatis per pesanan**. Memungkinkan perdagangan 24/7 hands-free.

**T: Apakah aman? Perlu server?**
J: Berjalan sepenuhnya lokal. Kunci API disimpan di komputer Anda. **Jangan pernah berikan izin "Penarikan" ke kunci API Anda.**

**T: Hubungannya dengan `binance_p2p_bot`?**
J: Saling melengkapi:
- [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot): Auto-pricing, ranking (**manajemen pra-pesanan**)
- `binance&bybit&okx P2P auto_payment_bot`: Pembayaran, verifikasi, pelepasan otomatis (**penyelesaian pesanan**)

**T: Apa bedanya "Pembayaran Otomatis per Pesanan" dengan "Auto Release" tradisional?**
J: Solusi tradisional hanya menangani tahap pelepasan. `binance&bybit&okx P2P auto_payment_bot` melangkah lebih jauh — **otomatis melakukan transfer** di aplikasi pembayaran. Ini adalah triad lengkap: **pembayaran + verifikasi + pelepasan**.

**T: Bagaimana cara mencegah pelepasan yang salah?**
J: Perlindungan multi-lapis: (1) Pencocokan jumlah presisi; (2) Deduplikasi ID global (180 hari); (3) Perlindungan timeout; (4) Pemantauan saldo.

**T: Aplikasi pembayaran apa yang didukung?**
J: Secara teoritis semua aplikasi Android — melalui mesin adaptasi cerdas. Konfigurasi metode pembayaran saja, mesin menangani sisanya. Detail di [apip2p.top](https://apip2p.top).

**T: Apakah konfigurasi hilang jika restart?**
J: Tidak. Semua disimpan lokal secara real-time. Bot otomatis memulihkan konfigurasi saat startup.

**T: Stack teknologi apa yang digunakan?**
J: Python untuk pengalaman desktop native. Arsitektur multi-thread. HMAC-SHA256 untuk API. Database lokal.

**T: Mendukung aset volatil (BTC, ETH)?**
J: Ya. Logika pembayaran beroperasi pada jumlah fiat. Untuk BTC/ETH, rekomendasikan Spot Market Sync dari [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot).

---

## 🔗 Ekosistem Resmi & Kontak

* **Demo Video:** [🎬 Tonton di YouTube](https://www.youtube.com/watch?v=INewAPHXa5c)
* **Situs Resmi:** [apip2p.top](https://apip2p.top/)
* **Bot P2P Pendamping:** [ApiP2P-top/binance-p2p-bot](https://github.com/ApiP2P-top/binance-p2p-bot)
* **Telegram Developer:** [@eason01993](https://t.me/eason01993)
* **Channel Komunitas:** [t.me/eason01993_p2p](https://t.me/eason01993_p2p)
* **YouTube:** [Eason's YouTube Channel](https://www.youtube.com/@eason01993)

---

## ⚠️ Penafian

`binance&bybit&okx P2P auto_payment_bot` disediakan untuk tujuan pendidikan dan referensi. Perdagangan P2P crypto membawa risiko signifikan. Simpan kunci API Anda dengan aman. Jangan berikan izin "Penarikan" ke alat otomatis. Software tidak bertanggung jawab atas kerugian finansial. Gunakan sepenuhnya atas risiko Anda sendiri.
