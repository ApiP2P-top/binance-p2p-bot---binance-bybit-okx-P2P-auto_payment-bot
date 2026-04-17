<div align="center">

**🌐 Select Language / 选择语言**

[English](README.md) | [中文](README_CN.md) | **Español** | [Русский](README_RU.md) | [Bahasa Indonesia](README_ID.md) | [Türkçe](README_TR.md) | [Tiếng Việt](README_VN.md) | [ภาษาไทย](README_TH.md) | [हिन्दी](README_HI.md) | [اردو](README_UR.md) | [Filipino](README_PH.md)

</div>

---

# binance&bybit&okx P2P auto_payment_bot: Bot de Pago Automático por Orden · Verificación de Facturas · Liberación de Criptomonedas

> Motor de automatización de grado profesional para comerciantes P2P de Binance, Bybit y OKX — **pago automático por orden**, **verificación inteligente de facturas**, **liberación de criptomonedas en milisegundos**, con procesamiento paralelo multi-dispositivo y multi-aplicación.

# [Bot de Pago Automático P2P (Binance & Bybit & OKX) Sitio Oficial](https://apip2p.top)

## 🎬 Demostración en Video


https://github.com/user-attachments/assets/262a41b2-cdcc-4abd-8c8e-4bdb1b566a03


[![Ver la Demo](https://img.shields.io/badge/YouTube-Ver_Demo-red?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=INewAPHXa5c)

> 📺 Mira la demostración completa de `binance&bybit&okx P2P auto_payment_bot` en acción — desde la detección de órdenes hasta el pago automático, la verificación de facturas y la liberación de criptomonedas.

La solución definitiva de liquidación automática de órdenes diseñada para comerciantes P2P (anunciantes). Elimine el ciclo agotador de transferencias manuales, verificación manual y liberación manual de criptomonedas. Con `binance&bybit&okx P2P auto_payment_bot`, logre **automatización completa 24/7** de pagos P2P por orden, verificación de depósitos y liberación instantánea de criptomonedas.

---

## 📝 Descripción General y Arquitectura
<img width="1360" height="881" alt="2" src="https://github.com/user-attachments/assets/b2c18af8-6b0f-4062-a947-bb10e99aeed7" />


`binance&bybit&okx P2P auto_payment_bot` es un **cliente de escritorio de automatización local** de grado profesional construido específicamente para Comerciantes P2P (Anunciantes) que operan en **Binance**, **Bybit** y **OKX**.

En el comercio P2P, los comerciantes enfrentan operaciones repetitivas masivas:

- ❌ **Pagos Manuales**: Por cada orden de compra, el comerciante debe abrir manualmente una aplicación de pago, ingresar el monto y el destinatario, confirmar la transferencia
- ❌ **Verificación Manual**: Por cada orden de venta, abrir manualmente la aplicación de pago para verificar si el comprador realmente pagó
- ❌ **Liberación Manual**: Después de la verificación, volver al exchange para hacer clic manualmente en el botón de liberación
- ❌ **Brechas Nocturnas**: Durante las horas sin servicio, las órdenes se acumulan sin procesar

`binance&bybit&okx P2P auto_payment_bot` resuelve todo esto mediante **automatización inteligente de dispositivos + integración API del exchange**:

- ✅ **🔥 Pago Automático por Orden (Función Principal)**: El exchange recibe una orden de compra → completa automáticamente la transferencia en su aplicación de pago → sin intervención humana
- ✅ **Verificación Inteligente de Facturas**: El comprador marca el pago como enviado → abre automáticamente su aplicación de pago para verificar el monto y la información del remitente
- ✅ **Liberación de Criptomonedas en Milisegundos**: Después de una verificación exitosa, llama instantáneamente a la API del exchange para liberar criptomonedas
- ✅ **Procesamiento Paralelo Multi-Dispositivo**: Conecte múltiples teléfonos simultáneamente, diferentes dispositivos manejan diferentes órdenes
- ✅ **Gestión Unificada Multi-Exchange**: Binance, Bybit, OKX — las tres plataformas en una sola interfaz
- ✅ **Operación Completamente Local**: Sin dependencias de servidores externos; sus claves API nunca salen de su máquina

**Posicionamiento**: `binance&bybit&okx P2P auto_payment_bot` maneja **pago por orden + verificación de facturas + liberación automática** (liquidación de órdenes). Combinado con [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot) (auto-pricing y ranking), logre **automatización inteligente completa de extremo a extremo** de su negocio de comerciante P2P.

---

## 🚀 Características Principales (Algoritmo y Flujo de Trabajo)

### 1. 🔥 Pago Automático por Orden (Motor de Transferencia Automática)

> **Esta es la capacidad más crítica y diferenciadora de `binance&bybit&okx P2P auto_payment_bot`.**

Cuando el exchange recibe una orden de compra P2P, el software **automatiza completamente la ejecución del pago**:

- **Detección de Órdenes en Tiempo Real**: Ciclo de polling de 5 segundos detecta nuevas órdenes P2P sin retraso
- **Despacho Inteligente de Dispositivos**: Empareja automáticamente dispositivos inactivos según el método de pago de la orden
- **Ejecución Automatizada de Pagos**: Completa todo el flujo de transferencia en la aplicación de pago del teléfono — sin intervención humana
- **Motor de Pago Adaptativo**: El motor de protocolo inteligente se adapta autónomamente a la interfaz de cada app de pago — optimiza continuamente las rutas de ejecución
- **Soporte Multi-Método de Pago**: Un solo dispositivo puede configurarse con múltiples métodos de pago
- **Archivo de Capturas de Transferencia**: Captura automáticamente capturas de pantalla de prueba después de cada transferencia

#### 💡 Concepto de Implementación (Motor de Programación de Transferencias)

```python
# [Pseudocódigo] Detección de órdenes → emparejamiento de dispositivos → pipeline de transferencia
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
    POLL_INTERVAL = 5  # seconds

    def __init__(self, exchange_clients: dict, device_pool: 'DevicePool'):
        self.exchanges   = exchange_clients
        self.device_pool = device_pool

    async def run(self):
        """Main loop: poll orders → match devices → dispatch transfers"""
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

---

### 2. Verificación Inteligente de Facturas (Motor de Verificación de Depósitos)

Cuando publica un anuncio de venta P2P y un comprador dice que ha pagado, el software abre automáticamente su aplicación de pago para verificar el depósito:

- **Navegación Automática de Facturas**: Abre automáticamente la aplicación de pago designada y navega al historial de transacciones
- **Coincidencia Precisa de Montos**: Localiza la transacción más reciente que coincide con el monto de la orden
- **Verificación del Remitente**: Valida cruzadamente el nombre del remitente con la información del comprador
- **Extracción de ID de Transacción Única**: Lee el número de referencia como identificador único global
- **Protección Anti-Replay**: Cada ID de transacción se registra en una base de datos local — el mismo depósito nunca puede activar dos liberaciones

---

### 3. Ejecución Automatizada de Liberación de Criptomonedas (Activación API en Milisegundos)

- **Liberación Instantánea**: Después de una verificación exitosa, llama inmediatamente a la API P2P del exchange
- **Verificación del Resultado de Liberación**: Después de llamar a la API, consulta activamente el estado de la orden
- **Reintento con Retroceso Exponencial**: Si falla, reintenta automáticamente hasta 3 veces con retroceso exponencial
- **Adaptador Unificado Tri-Plataforma**: Binance C2C SAPI / Bybit V5 P2P / OKX V5 P2P — interfaz unificada

---

### 4. Procesamiento Paralelo Multi-Dispositivo y Despacho Inteligente

- **Conexión Multi-Dispositivo**: Conecte múltiples teléfonos Android simultáneamente (USB / WiFi)
- **Duplicación de Pantalla en Tiempo Real**: Transmisión de pantalla de alto rendimiento (30-60fps / 30-70ms de latencia)
- **Asignación Inteligente de Tareas**: Empareja automáticamente dispositivos inactivos con la aplicación requerida
- **Colas de Tareas Independientes**: Cada dispositivo mantiene su propia cola FIFO
- **Monitoreo de Estado del Dispositivo**: Muestra en tiempo real el estado de conexión, tarea actual y saldo

---

### 5. Motor Inteligente de Adaptación de Pagos

- **Soporte de Pago Sin Configuración**: El motor reconoce y se adapta automáticamente a cualquier interfaz de app de pago mediante algoritmos de reconocimiento visual propietarios
- **Sin Programación Requerida**: Simplemente conecte su dispositivo y configure el método de pago — el motor maneja toda la lógica de interacción
- **Optimización Multi-Dimensional**: Rutas de ejecución optimizadas por Dispositivo × App × Método de Pago para máxima velocidad
- **Compatibilidad Entre Dispositivos**: Los perfiles de pago son portátiles entre dispositivos con configuraciones similares

---

### 6. Control de Riesgos y Protección Anti-Fraude

- **Deduplicación de ID de Transacción**: Cada número de referencia se registra — la misma transacción nunca activa dos liberaciones
- **Coincidencia Precisa de Montos**: Rechaza la liberación automática cuando la desviación excede la tolerancia
- **Protección por Tiempo Límite**: Sin liberación si no se encuentra depósito dentro del tiempo configurado
- **Monitoreo de Saldo**: Pausa automática cuando el saldo es insuficiente
- **Archivo Completamente Local**: Registros con retención de 180 días y limpieza automática

---

### 7. Persistencia de Estado y Recuperación de Sesión

- **Auto-Guardado de Configuración**: Todas las configuraciones se escriben en archivos locales en tiempo real
- **Recuperación ante Caídas**: Al reiniciar, restaura automáticamente la última configuración
- **Recuperación de Órdenes Pendientes**: Al iniciar, escanea órdenes aún en estado "Pendiente"

---

## 🏗️ Arquitectura del Sistema

```
┌───────────────────────────────────────────────────────────────────────┐
│                     Ventana Principal GUI de Escritorio                 │
│  ┌─────────────┐ ┌──────────────┐ ┌──────────────┐ ┌─────────────┐  │
│  │  Gestión de  │ │  Gestión de  │ │  Duplicación │ │  Registro   │  │
│  │  Exchanges  │ │ Dispositivos │ │  de Pantalla │ │  y Auditoría│  │
│  │ Binance/    │ │(Multi-teléf.)│ │  (Tiempo     │ │  (Stream)   │  │
│  │ Bybit/OKX  │ │              │ │   Real)      │ │             │  │
│  └──────┬──────┘ └──────┬───────┘ └──────┬───────┘ └─────────────┘  │
│         │               │               │                            │
│  ┌──────▼───────────────▼───────────────▼────────────────────────┐   │
│  │                    Bus de Eventos Signal / Slot                  │   │
│  └──────┬───────────────┬───────────────┬────────────────────────┘   │
│         │               │               │                            │
│  ┌──────▼──────┐ ┌──────▼───────┐ ┌─────▼───────┐ ┌──────────────┐ │
│  │  Poller de  │ │  Ejecutor de │ │ Verificador │ │  Persistencia│ │
│  │  Órdenes    │ │Transferencias│ │ de Facturas │ │  de Estado   │ │
│  │ (5s ciclo)  │ │(pago por     │ │(verificación│ │  (JSON/DB)   │ │
│  │             │ │  orden)      │ │ depósito)   │ │              │ │
│  └──────┬──────┘ └──────┬───────┘ └─────┬───────┘ └──────────────┘ │
│         │               │               │                            │
│  ┌──────▼───────────────▼───────────────▼────────────────────────┐   │
│  │              Capa Anti-Replay y Control de Riesgos              │   │
│  └───────────────────────────────────────────────────────────────┘   │
└──────────────────────────────┬────────────────────────────────────────┘
                               │
                 ┌─────────────▼─────────────────┐
                 │   Capa de Adaptadores API       │
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

## ⚙️ Especificaciones Técnicas

| Componente | Parámetro | Descripción |
|------------|-----------|-------------|
| Ciclo de Polling | 5,000 ms | Polling independiente por exchange |
| Tolerancia de Coincidencia | 0.01–0.10 | Configurable; absorbe micro-discrepancias |
| Intervalo de Reintento | Retroceso exponencial 2/4/8s | Máx. 3 reintentos |
| Retención de Dedup | 180 días | Limpieza automática de registros expirados |
| FPS de Duplicación | 30–60 fps | Latencia baja (30-70ms) |
| Concurrencia Multi-Dispositivo | Ilimitada | Conexiones USB + WiFi híbridas |
| Temas de UI | Oscuro / Claro | Modo oscuro y modo claro |
| Idiomas de UI | 中文 / English | Soporte bilingüe completo |

---

## 🌍 Adaptación Regional: Soporte Multi-Mercado Global

A través de su motor de adaptación inteligente de pagos, `binance&bybit&okx P2P auto_payment_bot` teóricamente soporta **cualquier aplicación de pago Android en el mundo**.

### Canales de Pago Soportados

| Región | Aplicación / Canal | Integración |
|--------|-------------------|-------------|
| 🇵🇭 Filipinas | Maya, GCash, UnionBank, BDO, BPI | Automatización inteligente |
| 🇨🇳 China Continental | Alipay, WeChat Pay, Apps Bancarias | Automatización inteligente |
| 🇮🇳 India | PhonePe, Google Pay, Paytm, BHIM UPI | Automatización inteligente |
| 🇮🇩 Indonesia | Dana, OVO, GoPay, ShopeePay | Automatización inteligente |
| 🇻🇳 Vietnam | MoMo, ZaloPay, Vietcombank | Automatización inteligente |
| 🇹🇭 Tailandia | PromptPay, TrueMoney, K PLUS | Automatización inteligente |
| 🇰🇷 Corea del Sur | Toss, KakaoPay | Automatización inteligente |
| 🇭🇰 Hong Kong | FPS, PayMe | Automatización inteligente |
| 🇹🇼 Taiwán | Apps Bancarias, LINE Pay | Automatización inteligente |
| 🇹🇷 Turquía | Papara, İş Bankası | Automatización inteligente |
| 🌐 Más Mercados | Adaptable a cualquier app mediante automatización inteligente | Extensible |

---

## 🌐 Palabras Clave del Ecosistema

`binance&bybit&okx P2P auto_payment_bot`, `P2P auto payment`, `Binance P2P auto payment`, `Binance P2P auto release`, `Bybit P2P payment confirmation`, `OKX P2P bot`, `P2P payment automation`, `crypto P2P release bot`, `USDT auto release`, `P2P payment-by-order`, `auto transfer bot`, `Binance C2C automation`, `Bybit C2C bot`, `OKX C2C automation`, `P2P merchant automation`, `crypto OTC automation`, `Maya auto payment`, `GCash auto transfer`, `UPI auto payment`, `binance p2p bot`, `bybit p2p bot`.

---

## 💡 Preguntas Frecuentes (FAQ)

**P: ¿Qué es `binance&bybit&okx P2P auto_payment_bot`?**
R: Es un cliente de escritorio de automatización local de grado profesional para comerciantes P2P de Binance, Bybit y OKX. Su capacidad principal es el **pago automático por orden** — cuando el exchange recibe una orden de compra, el software completa automáticamente la transferencia. Permite comercio P2P sin intervención 24/7.

**P: ¿El software necesita un servidor? ¿Es seguro?**
R: `binance&bybit&okx P2P auto_payment_bot` funciona completamente en su máquina local sin dependencias externas. Las claves API se almacenan localmente. **Nunca otorgue permisos de "Retiro" a sus claves API.**

**P: ¿Cuál es la relación con `binance_p2p_bot`?**
R: Son complementarios:
- [`binance_p2p_bot`](https://github.com/ApiP2P-top/binance-p2p-bot): Auto-pricing, ranking, gestión de anuncios (**gestión pre-orden**)
- `binance&bybit&okx P2P auto_payment_bot`: Pago por orden, verificación, liberación automática (**liquidación de órdenes**)

---

## 🔗 Ecosistema Oficial y Contacto

* **Demostración en Video:** [🎬 Ver en YouTube](https://www.youtube.com/watch?v=INewAPHXa5c)
* **Página Oficial:** [apip2p.top](https://apip2p.top/)
* **Bot de Licitación P2P:** [ApiP2P-top/binance-p2p-bot](https://github.com/ApiP2P-top/binance-p2p-bot)
* **Telegram del Desarrollador:** [@eason01993](https://t.me/eason01993)
* **Canal de la Comunidad:** [t.me/eason01993_p2p](https://t.me/eason01993_p2p)
* **YouTube:** [Canal de YouTube de Eason](https://www.youtube.com/@eason01993)

---

## ⚠️ Descargo de Responsabilidad

`binance&bybit&okx P2P auto_payment_bot` se proporciona con fines educativos y de referencia estructural. El comercio P2P de criptomonedas conlleva un riesgo significativo. Almacene de forma segura sus claves API y credenciales. Nunca asigne permisos de "Retiro" a herramientas automatizadas. El software no asume ninguna responsabilidad por pérdidas financieras. Úselo bajo su propio riesgo.
