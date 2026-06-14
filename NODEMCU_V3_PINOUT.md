# NodeMCU v3 (ESP32) - Połączenia Pinów dla Rolet

## 📌 Rekomendacja - Optymalne Połączenia

### ✅ Schemat Podłączenia (BEZPIECZNE PINY)

```
┌──────────────────────────────────────────────────────────┐
│                    NodeMCU v3 (ESP32)                    │
│                                                          │
│  D5 (GPIO14) ──────────→ ULN2003 IN1 (pin_a)           │
│  D6 (GPIO12) ──────────→ ULN2003 IN2 (pin_b)           │
│  D7 (GPIO13) ──────────→ ULN2003 IN3 (pin_c)           │
│  D8 (GPIO27) ──────────→ ULN2003 IN4 (pin_d)           │
│  GND ──────────────────→ ULN2003 GND                   │
│                                                          │
└──────────────────────────────────────────────────────────┘
         │
         │
    ┌────┴────┐
    │ ULN2003  │
    │ Sterownik│
    └────┬────┘
         │
    ┌────┴────────────────────────┐
    │   Silnik 28BYJ-48            │
    │   (Cewki A, B, C, D + GND)   │
    └─────────────────────────────┘
```

## 🔌 Tabela Połączeń

| Funkcja | NodeMCU Pin | GPIO | → | ULN2003 Pin |
|---------|-------------|------|---|-------------|
| Cewka A | D5 | GPIO14 | → | IN1 |
| Cewka B | D6 | GPIO12 | → | IN2 |
| Cewka C | D7 | GPIO13 | → | IN3 |
| Cewka D | D8 | GPIO27 | → | IN4 |
| **Masa** | **GND** | **GND** | → | **GND** |

## ⚡ Zasilanie

```
┌─────────────────────────────────────────┐
│   Zasilacz 5V-12V (Min 2A)              │
├─────────────────────────────────────────┤
│   + 5V/12V ──→ ULN2003 VCC             │
│   GND ───────→ ULN2003 GND             │
│               └→ Silnik GND (wspólne)   │
│               └→ NodeMCU GND (wspólne)  │
└─────────────────────────────────────────┘
```

## 🖥️ Konfiguracja ESPHome

```yaml
esphome:
  name: roleta-nodemcu-v3
  friendly_name: Roleta Salon

esp32:
  board: esp32dev

# Konfiguracja pinów dla ULN2003 - NodeMCU v3
stepper:
  - platform: uln2003
    id: my_stepper
    pin_a: GPIO14      # D5
    pin_b: GPIO12      # D6
    pin_c: GPIO13      # D7
    pin_d: GPIO27      # D8
    max_speed: 500
    acceleration: 200
    sleep_when_done: true

# Konfiguracja urządzenia typu "Roleta" (Cover)
cover:
  - platform: stepper
    name: "Roleta Okno"
    id: my_cover
    stepper_id: my_stepper
    full_steps: 20000
    # direction_inverted: true   # Odkomentuj jeśli kierunek jest odwrotny

logger:
  level: INFO

api:
  encryption:
    key: !secret api_key

ota:
  password: !secret ota_password
```

## 📊 Pełna Mapa Pinów NodeMCU v3

| NodeMCU | GPIO | Status | Opis | Nasza Użytkowość |
|---------|------|--------|------|------------------|
| **D5** | **GPIO14** | ✅ | SPI Clock | **pin_a (IN1)** |
| **D6** | **GPIO12** | ✅ | SPI MISO | **pin_b (IN2)** |
| **D7** | **GPIO13** | ✅ | SPI MOSI | **pin_c (IN3)** |
| **D8** | **GPIO27** | ✅ | Available | **pin_d (IN4)** |
| D0 | GPIO16 | ⚠️ RTC | Nie zalecane | - |
| D1 | GPIO5 | ⚠️ | I2C SCL | UNIKAJ |
| D2 | GPIO4 | ⚠️ | I2C SDA | UNIKAJ |
| D3 | GPIO0 | 🔴 | Boot Pin | UNIKAJ |
| D4 | GPIO2 | 🔴 | LED/UART | UNIKAJ |
| D9 | GPIO26 | ⚠️ | DAC1 (Audio) | UNIKAJ |
| D10 | GPIO25 | ⚠️ | DAC2 (Audio) | UNIKAJ |
| RX | GPIO3 | 🔴 | UART RX | UNIKAJ |
| TX | GPIO1 | 🔴 | UART TX | UNIKAJ |

## 🎯 Dlaczego Wybrana Konfiguracja?

✅ **GPIO14, GPIO12, GPIO13, GPIO27** są optymalne ponieważ:
- Nie kolidują z WiFi (GPIO17, GPIO15)
- Nie są rezerwowane do boot/UART
- Mają w pełni funkcjonalne wyjścia cyfrowe
- Kompatybilne z ESPHome i Home Assistant
- Nie są używane jako DAC (audio)

## 📋 Procedura Montażu Krok po Kroku

### 1. Przygotowanie Przewodów
- Użyj przewodów Dupont lub skręconych przewodów z tulejkami
- Długość: 10-20cm dla każdego połączenia

### 2. Połączenia GPIO NodeMCU → ULN2003
```
NodeMCU D5  (GPIO14) ──[przewód]──→ ULN2003 IN1
NodeMCU D6  (GPIO12) ──[przewód]──→ ULN2003 IN2
NodeMCU D7  (GPIO13) ──[przewód]──→ ULN2003 IN3
NodeMCU D8  (GPIO27) ──[przewód]──→ ULN2003 IN4
NodeMCU GND ──[przewód]──→ ULN2003 GND
```

### 3. Połączenia ULN2003 → Silnik 28BYJ-48
```
ULN2003 OUT1 ──→ Silnik A (pin 1)
ULN2003 OUT2 ──→ Silnik B (pin 2)
ULN2003 OUT3 ──→ Silnik C (pin 3)
ULN2003 OUT4 ──→ Silnik D (pin 4)
ULN2003 GND ──→ Silnik GND (pin 5)
```

### 4. Zasilanie
```
PSU 5V ──[przewód]──→ ULN2003 VCC
PSU GND ──[przewód]──→ ULN2003 GND
                   └──→ NodeMCU GND (wspólne!)
                   └──→ Silnik GND (wspólne!)
```

## ⚠️ Ważne Uwagi

1. **GND musi być wspólny** dla wszystkich komponentów
2. **Kondensator 1000µF** parallel do zasilania stabilizuje napięcie
3. **Min. zasilacz 2A** dla stabilnego działania
4. **Nie podłączaj silnika do 5V NodeMCU** - musi być osobny zasilacz
5. **D9 i D10 to DAC piny** - lepiej ich unikać dla GPIO

## 🔧 Testowanie Połączeń

```bash
# 1. Wgraj firmware
esphome run esphome/nodemcu_v3_roller_config.yaml

# 2. Sprawdź logi
esphome logs esphome/nodemcu_v3_roller_config.yaml

# 3. W Home Assistant:
# - Settings → Devices & Services → ESPHome
# - Powinno się pojawić urządzenie "Roleta Salon"
```

## 🐛 Troubleshooting

| Problem | Przyczyna | Rozwiązanie |
|---------|-----------|------------|
| Brak połączenia WiFi | ESP32 resetuje się | Wzmocnij zasilacz lub dodaj kondensator |
| Silnik się nie rusza | Złe połączenia GPIO | Sprawdź czy D5,D6,D7,D8 są podłączone |
| Silnik piszczy | Za wysoka prędkość | Zmniejsz `max_speed` do 300-400 |
| Kierunek odwrotny | Kolejność pinów | Ustaw `direction_inverted: true` |
| Roleta się grzeje | Brak sleep_when_done | Upewnij się że `sleep_when_done: true` |

---

**Ostatnia aktualizacja**: 2026-06-14  
**Urządzenie**: NodeMCU v3 (ESP32)  
**Silnik**: 28BYJ-48 + ULN2003  
**Status**: ✅ Bezpieczna konfiguracja
