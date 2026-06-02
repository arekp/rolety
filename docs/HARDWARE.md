# Hardware - Dokumentacja Sprzętu

## Wymagane Komponenty

### Elektronika

| Lp. | Komponent | Model | Ilość | Napięcie | Notatka |
|-----|-----------|-------|-------|----------|---------|
| 1 | Mikrokontroler | ESP32 Dev Kit | 1 | 3.3V/5V | Obsługuje WiFi i Bluetooth |
| 2 | Silnik Krokowy | 28BYJ-48 | 1 | 5V | Unipolarny, 4 fazy |
| 3 | Sterownik | ULN2003 Module | 1 | 5V logic | Driver dla silnika |
| 4 | Zasilacz | PSU 5V | 1 | 5V | Min. 2A, stabilny |
| 5 | Kondensat. | 1000µF / 16V | 1 | - | Do stabilizacji zasilania |
| 6 | Przełącznik | Limit switch | 2 | 5V | (Opcjonalnie) |
| 7 | Dioda | 1N4007 | 4 | - | (Opcjonalnie) Ochrona zwrotna |

### Połączenia

| Lp. | Typ | Ilość | Notatka |
|-----|-----|-------|---------|
| 1 | Przewód Dupont M-M | 10 szt | Połączenia logiczne (3.3V) |
| 2 | Przewód Dupont M-F | 5 szt | Do przycisków/czujników |
| 3 | Przewód zasilający | 2 m | Mały przekrój (0.5-1mm²) |
| 4 | Konektor JST | - | Do modułu silnika (opcjonalnie) |

---

## Schemat Podłączenia

### Układ Główny

```
┌──────────────────────────────────────────────────────────────────────┐
│                            ESP32 Dev Kit                             │
│                                                                       │
│  GPIO13 ────┐                                                        │
│  GPIO12 ────┤                                                        │
│  GPIO14 ────┼──────────────────┐                                    │
│  GPIO27 ────┤                  │                                    │
│  GND  ──────┼────┐         ┌───▼────────────┐                       │
│             │    │         │   ULN2003      │                       │
│             │    │    ┌────┤ Sterownik      │                       │
│             └────┼────┤    │   Silnika      │                       │
│                  │    │    └────┬───────────┘                        │
│                  │    │         │                                    │
│  3.3V ──────────┐│    │         │ Motor Pins                         │
│  5V ───────────┐││    │    ┌────▼────────────┐                      │
│                └┼┼────┘    │  28BYJ-48       │                      │
│                 ││         │  Stepper Motor  │                      │
│                 └┼─────────│                 │                      │
│                  │         └─────────────────┘                      │
└──────────────────┼─────────────────────────────────────────────────┘
                   │
            ┌──────▼──────┐
            │  PSU 5V     │
            │  ≥2A        │
            │             │
            └─────────────┘
```

### Pinout Szczegółowy

#### ESP32 GPIO Pins
```
┌─────────────────────────────────────────────┐
│           ESP32 Dev Kit Pinout              │
├─────────────────────────────────────────────┤
│ Pin 13 (GPIO13) ──► ULN2003 IN1             │
│ Pin 12 (GPIO12) ──► ULN2003 IN2             │
│ Pin 14 (GPIO14) ──► ULN2003 IN3             │
│ Pin 27 (GPIO27) ──► ULN2003 IN4             │
│ Pin 32 (GPIO32) ──► Limit Switch (Close)   │
│ Pin 33 (GPIO33) ──► Limit Switch (Open)    │
│ GND        ──► GND (wspólna masa)           │
└─────────────────────────────────────────────┘
```

#### ULN2003 Pinout
```
┌──────────────────────────────────────────────┐
│        ULN2003 Driver Module (DIP16)         │
├──────────────────────────────────────────────┤
│ IN1 (Pin 1)   ◄── ESP32 GPIO13              │
│ IN2 (Pin 2)   ◄── ESP32 GPIO12              │
│ IN3 (Pin 3)   ◄── ESP32 GPIO14              │
│ IN4 (Pin 4)   ◄── ESP32 GPIO27              │
│                                              │
│ OUT1 (Pin 16) ──► Motor Pin A               │
│ OUT2 (Pin 15) ──► Motor Pin B               │
│ OUT3 (Pin 14) ──► Motor Pin C               │
│ OUT4 (Pin 13) ──► Motor Pin D               │
│                                              │
│ VCC (Pin 8)   ◄── +5V Power Supply          │
│ GND (Pin 9)   ◄── GND (wspólna masa)        │
└──────────────────────────────────────────────┘
```

#### 28BYJ-48 Motor Wiring
```
┌────────────────────────────────────────────┐
│    28BYJ-48 Stepper Motor Coil Pins        │
├────────────────────────────────────────────┤
│ Pin 1 (Red)    - Common (VCC)              │
│ Pin 2 (Brown)  - Coil A ──► ULN2003 OUT1  │
│ Pin 3 (Yellow) - Coil B ──► ULN2003 OUT2  │
│ Pin 4 (Orange) - Coil C ──► ULN2003 OUT3  │
│ Pin 5 (Pink)   - Coil D ──► ULN2003 OUT4  │
│                                             │
│ Red pin connects to +5V (Power Supply)     │
└────────────────────────────────────────────┘
```

---

## Diagram Połączeń Zasilania

```
                 ┌────────────────┐
                 │  PSU 5V / 12V  │
                 │     ≥ 2A       │
                 └────┬────────┬──┘
                      │        │
                    +5V       GND
                      │        │
         ┌────────────┼────────┼────────────────┐
         │            │        │                │
         │    ┌───────┴───┐    │                │
         │    │   1000µF  │    │                │
         │    │  Kondensa-│    │                │
         │    │   tor     │    │                │
         │    └───────┬───┘    │                │
         │            │        │                │
         ▼            ▼        ▼                │
      ┌─────┐      ┌──────┐ ┌─────┐           │
      │ESP32│      │ULN20 │ │Motor│           │
      │     │      │  03  │ │ 28  │           │
      │ VCC ├─────►│ VCC  │ │BYJ48│           │
      │ GND ├──────┤ GND  ├─┤GND  │           │
      └─────┘      └──────┘ └─────┘           │
                                               │
         ┌─────────────────────────────────────┘
         │
         └──────────────► GND (wspólna masa)
```

### Ważne: Wspólna Masa (Common GND)
```
┌─────────────────────────────────────────┐
│         WSPÓLNA MASA - KRYTYCZNE!       │
├─────────────────────────────────────────┤
│                                         │
│  PSU GND ────┐                         │
│              ├─► WSPÓLNA MASA (0V)    │
│  ESP32 GND ──┤      │                  │
│              ├──────┘                  │
│  ULN2003 GND │                         │
│                                         │
│  NIGDY nie rozdzielaj mas!             │
│  Wszystkie urządzenia muszą mieć       │
│  wspólny punkt GND!                    │
└─────────────────────────────────────────┘
```

---

## Montaż Mechaniczny

### Materiały Montażowe

| Komponent | Ilość | Opis |
|-----------|-------|------|
| Profil aluminiowy | 2 m | Do montażu silnika i rolatki |
| Wałek aluminiowy | 1 szt | Do napędu rolety |
| Sprzęgło elastyczne | 1 szt | Połączenie silnika z wałkiem |
| Łożyska | 2 szt | Do oparcia wałka |
| Śruby M4 | 10 szt | Mocowanie komponentów |
| Muf PVC | 1 szt | Ochrona przewodów |

### Układ Montażu

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  Górna część okna (sufit)                                  │
│  ┌───────────────────────────────────────────────────────┐ │
│  │                                                       │ │
│  │  ┌──────────┐      ┌──────────┐      ┌──────────┐   │ │
│  │  │Łożysko L │      │Wałek alu │      │Łożysko P │   │ │
│  │  └──────┬───┘      └────┬─────┘      └──────┬───┘   │ │
│  │         │               │                    │       │ │
│  │  ┌──────▼───────────────▼────────────────────▼────┐  │ │
│  │  │           Roleta zwijana                       │  │ │
│  │  │    (materiał + mechanizm zwijania)             │  │ │
│  │  └──────────────────────────────────────────────┬─┘  │ │
│  │                                                 │     │ │
│  │  ┌──────────────┐                           ┌───────┐│ │
│  │  │ Sterownik +  │   Elastyczne             │Silnik ││ │
│  │  │ Motor 28BYJ  │◄──sprzęgło◄──────────────│28BYJ ││ │
│  │  └──────────────┘                           └───────┘│ │
│  │        (Na ścianie poniżej okna)                      │ │
│  │                                                       │ │
│  └───────────────────────────────────────────────────────┘ │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Testy Sprzętu

### Checklist Przed Pierwszym Uruchomieniem

- [ ] Sprawdzić wszystkie połączenia pinów
- [ ] Sprawdzić polaryzację zasilania (+5V i GND)
- [ ] Upewnić się, że masa (GND) jest wspólna dla wszystkich urządzeń
- [ ] Wgotować firmware ESPHome
- [ ] Podłączyć zasilacz (bez podłączonego motora - test logiki)
- [ ] Sprawdzić komunikację WiFi
- [ ] Podłączyć motor i przetestować ruchy
- [ ] Skalibrować pozycje w Home Assistant

### Pomiary Diagnostyczne

#### Zasilanie
```
Pomiar multimetrem (DC):
- ESP32 VCC: 3.3V
- ULN2003 VCC: 5V
- PSU Output: 5V (bez obciążenia)
- Motor Red Pin: 5V (gdy cewka aktywna)
```

#### Sygnały GPIO
```
Logika (3.3V):
- GPIO13-27 "High": ~3.3V
- GPIO13-27 "Low": 0V
```

---

## Rozwiązywanie Problemów Sprzętowych

### Problem: Brak zasilania urządzenia

**Sprawdzenie:**
1. Podłącz PSU do gniazdka AC
2. Zmierz woltaż wyjściowy PSU (powinno być 5V)
3. Sprawdź przewód zasilający na przerwę
4. Sprawdzić polaryzację (czerwony = +, czarny = -)

### Problem: Motor się nie rusza

**Sprawdzenie:**
1. Czy cewki mają +5V gdy silnik powinien działać?
2. Czy ESP32 wysyła sygnały na GPIO13-27?
3. Czy przewody nie są przecięte?
4. Czy ULN2003 ma prawidłowe zasilanie?

### Problem: Motor piszczy bez ruchu

**Sprawdzenie:**
1. Zmniejsz `max_speed` w konfiguracji (300 steps/s)
2. Sprawdzić połączenia silnika (PIN1-5)
3. Zmierzyć napięcie na każdym wyjściu ULN2003 (OUT1-4)

### Problem: ESP32 resetuje się przy starcie silnika

**Sprawdzenie:**
1. Czy zasilacz dostarcza przynajmniej 2A?
2. Czy dodałeś kondensator 1000µF?
3. Czy wszystkie masy (GND) są połączone?

---

## Bezpieczeństwo

### ⚠️ Ostrzeżenia

1. **Zasilanie**: Silnik MUSI mieć oddzielny zasilacz od ESP32
2. **Masa**: Wspólna masa jest OBOWIĄZKOWA
3. **Prąd**: ULN2003 pobiera prąd na postoju - używaj `sleep_when_done: true`
4. **Grzanie**: Sprawdzaj temperaturę ULN2003 podczas działania
5. **Ochrona**: Zainstaluj bezpiecznik 3A na zasilaniu silnika

### 🔒 Normy i Standardy

- Projekt kompatybilny z CE (przy prawidłowym zasilaniu)
- Używaj zasilacza z certyfikatem UL/FCC
- Montaż powinien być niedostępny dla dzieci

---

## Źródła i Referencje

- [Datasheet 28BYJ-48](https://datasheetspdf.com/pdf-file/1310626/28byj-48.pdf)
- [Datasheet ULN2003](https://datasheetspdf.com/pdf-file/1320223/ULN2003.pdf)
- [ESP32 Pinout Reference](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)
- [ESPHome Stepper Documentation](https://esphome.io/components/stepper/index.html)

