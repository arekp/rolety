# Rolety - Stepper Motor Roller Blind Control with ESPHome

Projekt sterowania roletami za pomocą silnika krokowego **28BYJ-48** i sterownika **ULN2003** zintegrowanego z **Home Assistant** poprzez **ESPHome**.

## 📋 Spis Treści

1. [Opis Projektu](#opis-projektu)
2. [Wymagane Komponenty](#wymagane-komponenty)
3. [Schemat Podłączenia](#schemat-podłączenia)
4. [Instalacja](#instalacja)
5. [Konfiguracja](#konfiguracja)
6. [Kalibracja](#kalibracja)
7. [Troubleshooting](#troubleshooting)

## 🎯 Opis Projektu

Projekt umożliwia integrację elektrycznych rolet sterowanych silnikami krokowymi z Home Assistant. System oferuje:

- ✅ Pełną kontrolę otwierania/zamykania rolet
- ✅ Ustawianie pozycji rolet na procent (0-100%)
- ✅ Automatyzacje w Home Assistant
- ✅ Efektywne zarządzanie energią (sleep when done)
- ✅ Wsparcie wielu rolet niezależnie

## 🔧 Wymagane Komponenty

### Hardware
| Komponent | Ilość | Notatka |
|-----------|-------|---------|
| ESP32 Dev Kit | 1 | lub inny mikrokontroler obsługiwany przez ESPHome |
| Stepper Motor 28BYJ-48 | n | Silnik krokowy 5V |
| ULN2003 Driver Module | n | Sterownik dla silnika krokowego |
| Power Supply 5V-12V | 1 | Min. 2A dla stabilności |
| Przewody dupont | - | Połączenia pomiędzy komponentami |
| Przełączniki/Przyciski | 2 | Opcjonalnie: otwarty/zamknięty |

### Software
- ESPHome 2024.1.0+ (najnowsza wersja)
- Home Assistant 2024.1.0+ (najnowsza wersja)
- YAML (do konfiguracji)

## 🔌 Schemat Podłączenia

```
┌─────────────┐
│   ESP32     │
│ Dev Kit     │
└──────┬──────┘
       │
   ┌───┴────────────────────────────────┐
   │                                    │
   │  GPIO13 → ULN2003 IN1  ────┐      │
   │  GPIO12 → ULN2003 IN2  ────┤      │
   │  GPIO14 → ULN2003 IN3  ────┼─ VCC → Power Supply (5V-12V)
   │  GPIO27 → ULN2003 IN4  ────┤      │
   │          GND ←────────────┐ │      │
   │                           │ │      │
   │                           ▼ │      │
   └───────────────────────────┬─┴──────┘
                               │
                         ┌─────▼──────┐
                         │  ULN2003   │
                         │  Sterownik │
                         └─────┬──────┘
                               │
                    ┌──────────┴──────────┐
                    │                     │
                 Motor Coils 28BYJ-48  GND
                    │                     │
                    └──────────┬──────────┘
                               │
                        Power Supply GND
```

## 💻 Instalacja

### Krok 1: Przygotowanie ESPHome

1. Zainstaluj ESPHome:
```bash
pip install esphome
```

2. Klonuj/pobierz pliki projektu:
```bash
git clone https://github.com/arekp/rolety.git
cd rolety
```

### Krok 2: Konfiguracja Urządzenia

1. Otwórz plik `esphome/rolety-config.yaml`
2. Dostosuj parametry do Twojej konfiguracji (WiFi, piny GPIO, itp.)

### Krok 3: Wgranie Firmware

```bash
esphome run esphome/rolety-config.yaml
```

lub przez Home Assistant Web UI:
1. Wejdź w Settings → Devices & Services → ESPHome
2. Kliknij "Create New Device"
3. Załaduj plik konfiguracji z repo

## ⚙️ Konfiguracja

### Plik Konfiguracji ESPHome

Główny plik: `esphome/rolety-config.yaml`

**Kluczowe sekcje:**

#### 1. Definicja Silnika
```yaml
stepper:
  - platform: uln2003
    id: my_stepper
    pin_a: GPIO13
    pin_b: GPIO12
    pin_c: GPIO14
    pin_d: GPIO27
```

#### 2. Parametry Ruchu
```yaml
    max_speed: 500 steps/s        # Maksymalna prędkość
    acceleration: 200 steps/s^2   # Przyspieszenie
    sleep_when_done: true         # WAŻNE: wyłącza cewki na postoju
```

#### 3. Definicja Rołety (Cover)
```yaml
cover:
  - platform: stepper
    name: "Roleta Okno"
    id: my_cover
    stepper_id: my_stepper
    full_steps: 20000             # Liczba kroków na pełny ruch
```

## 📐 Kalibracja

### Procedura Kalibracji

1. **Wstępna Konfiguracja**
   - Ustaw `full_steps: 20000` (wartość początkowa)
   - Wgraj firmware

2. **Test w Home Assistant**
   - Otwórz panel kontrolny rołety
   - Przesuń suwak z 0% na 100%
   - Zaobserwuj ruch rolety

3. **Pomiar i Dostosowanie**
   - Zmierz, jak daleko odjechała roleta
   - Jeśli nie dojechała całkowicie:
     - Zwiększ `full_steps` (np. na 25000)
   - Jeśli zrobiła więcej obrotu niż potrzeba:
     - Zmniejsz `full_steps` (np. na 15000)

4. **Powtórz**
   - Powtarzaj krok 2-3 aż osiągniesz idealną pozycję

### Wzór na Obliczenie

Dla silnika 28BYJ-48:
- **1 obrót = ~4096 kroków** (bez redukcji)
- **Ze względu na redukcję wewnętrzną: ~1/64 obrotu = 4096 kroków**

Jeśli roleta wymaga n obrotów:
```
full_steps ≈ n × 4096
```

### Odwrócenie Kierunku

Jeśli kierunek jest odwrotny:

**Opcja 1: Konfiguracja YAML**
```yaml
cover:
  - platform: stepper
    direction_inverted: true
```

**Opcja 2: Zamiana Pinów**
```yaml
stepper:
  - platform: uln2003
    pin_a: GPIO27      # było pin_d
    pin_b: GPIO14      # było pin_c
    pin_c: GPIO12      # było pin_b
    pin_d: GPIO13      # było pin_a
```

## ⚡ Ważne Informacje

### Zasilanie

⚠️ **KRYTYCZNE**: Silnik 28BYJ-48 MUSI być zasilany z **oddzielnego zasilacza**

```
┌─────────────┐     ┌──────────────┐
│   ESP32     │────→│  GND ESP32   │
│ (3.3V/5V)   │     │ + GND PSU    │
└─────────────┘     └──────────────┘
                           ▲
                           │
                    ┌──────┴─────┐
                    │  ZASILACZ  │
                    │  5V-12V    │
                    │  Min. 2A   │
                    └────────────┘
                           │
                    ┌──────┴──────────┐
                    │   ULN2003 VCC   │
                    │   Motor GND     │
                    └─────────────────┘
```

### Grzanie Silnika

Problem: Silnik się grzeje podczas postoju
**Rozwiązanie**: Upewnij się, że `sleep_when_done: true` jest ustawione

### Reset ESP32 przy Starcie

Problem: ESP32 resetuje się przy uruchomieniu silnika
**Rozwiązanie**: 
- Użyj mocniejszego zasilacza (minimum 2A)
- Dodaj kondensator 1000µF parallel do zasilania
- Sprawdź stabilność połączeń GND

### Silnik Piszczy ale Się Nie Kręci

Problem: Słychać pisk, ale silnik się nie rusza
**Rozwiązanie**:
- Zmniejsz `max_speed` do 200-300 steps/s
- Sprawdź połączenia ULN2003
- Zmień kolejność pinów

## 🐛 Troubleshooting

| Problem | Przyczyna | Rozwiązanie |
|---------|-----------|------------|
| Brak połączenia z ESPHome | WiFi | Sprawdź SSID/hasło w konfiguracji |
| Silnik się nie rusza | Złe piny | Sprawdź konfigurację GPIO |
| Silnik piszczy | Za wysoka prędkość | Zmniejsz `max_speed` |
| ESP32 się resetuje | Niewystarczające zasilanie | Podłącz wzmocniony zasilacz |
| Roleta się grzeje | Brak sleep when done | Ustaw `sleep_when_done: true` |
| Zły kierunek | Konfiguracja | Ustaw `direction_inverted: true` |

## 📚 Dodatkowe Zasoby

- [Dokumentacja ESPHome - Stepper](https://esphome.io/components/stepper/index.html)
- [Dokumentacja ESPHome - Cover](https://esphome.io/components/cover/stepper.html)
- [Dokumentacja Home Assistant - Cover](https://www.home-assistant.io/integrations/cover/)
- [Datasheet 28BYJ-48](https://datasheetspdf.com/pdf-file/1310626/ContentDam/datasheetspdf.com/28byj-48.pdf)

## 📝 Licencja

MIT License - patrz plik LICENSE

## 👤 Autor

**arekp** - [GitHub Profile](https://github.com/arekp)

---

**Ostatnia aktualizacja**: 2026-06-02
