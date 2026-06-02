# Instrukcja Instalacji i Konfiguracji

## 📦 Wymagania Wstępne

- Python 3.7+
- pip (Python Package Manager)
- Home Assistant 2024.1+ (opcjonalnie)
- ESPHome Web Dashboard (opcjonalnie)
- Mikrokontroler ESP32 z obsługą USB-UART

---

## 🚀 Szybki Start

### Krok 1: Klonowanie Repozytorium

```bash
git clone https://github.com/arekp/rolety.git
cd rolety
```

### Krok 2: Instalacja ESPHome

#### Na Linux/MacOS:
```bash
pip install esphome
```

#### Na Windows:
```cmd
pip install esphome
```

Sprawdzenie instalacji:
```bash
esphome version
```

### Krok 3: Przygotowanie Pliku Konfiguracji

```bash
# Skopiuj template sekrety
cp esphome/secrets.yaml.example esphome/secrets.yaml

# Edytuj plik sekretów swoimi danymi
nano esphome/secrets.yaml
```

**Plik `secrets.yaml` powinien zawierać:**
```yaml
wifi_ssid: "Twoja_nazwa_WiFi"
wifi_password: "Twoje_haslo_WiFi"
api_encryption_key: "Wygenerowany_klucz_lub_nowy"
ota_password: "Haslo_do_aktualizacji_OTA"
```

### Krok 4: Wgranie Firmware na ESP32

#### Przy Pierwszym Wgraniu (przez USB):

```bash
esphome run esphome/rolety-config.yaml
```

Kroki:
1. Podłącz ESP32 do komputera przez USB
2. Wybierz port COM/ttyUSB (ESPHome powinien go wykryć)
3. ESPHome automatycznie skompiluje i wgra firmware

**Oczekiwane wyjście:**
```
Building variant... success
Compiling... success
Uploading... success
Connected!
```

#### Następne Wgrania (Over-The-Air):

```bash
esphome upload esphome/rolety-config.yaml
```

Lub przez Home Assistant Web UI (jeśli dodano urządzenie)

---

## 🏠 Integracja z Home Assistant

### Metoda 1: Automatyczna Integracja (Rekomendowana)

1. Otwórz Home Assistant
2. Przejdź do **Settings** → **Devices & Services**
3. Kliknij na zakładkę **ESPHome**
4. Urządzenie "Roleta Okno" powinno się pojawić automatycznie
5. Kliknij na nie i wybierz **Add Device**

### Metoda 2: Ręczna Konfiguracja

Dodaj do `configuration.yaml` Home Assistant:

```yaml
esphome:
  
cover:
  - platform: template
    name: "Roleta Okno"
    device_id: "roleta_okno"
```

Następnie zrestartuj Home Assistant:
**Settings** → **Developer Tools** → **YAML** → **Restart Home Assistant**

---

## ⚙️ Konfiguracja Zaawansowana

### Zmiana Pinów GPIO

Edytuj `esphome/rolety-config.yaml`:

```yaml
stepper:
  - platform: uln2003
    id: my_stepper
    pin_a: GPIO13      # ← Zmień na inny pin
    pin_b: GPIO12      # ← Zmień na inny pin
    pin_c: GPIO14      # ← Zmień na inny pin
    pin_d: GPIO27      # ← Zmień na inny pin
```

Dostępne piny na ESP32:
- GPIO0-39 (unikaj: GPIO34-39 - tylko wejście, GPIO6-11 - SPI Flash)
- Rekomendowane: 0, 2, 4, 5, 12-19, 21-23, 25-27, 32-33

### Dostosowanie Prędkości

```yaml
stepper:
  - platform: uln2003
    max_speed: 500 steps/s       # Zmniejsz, jeśli silnik piszczy
    acceleration: 200 steps/s^2  # Powolniejsze przyspieszenie
    deceleration: 200 steps/s^2
```

Zalecane wartości:
- Bardzo powoli: 200-300 steps/s
- Normalnie: 400-500 steps/s
- Szybko: 600-800 steps/s

### Dodanie Limit Switchy (Czujników Pozycji)

Edytuj `esphome/rolety-config.yaml`:

```yaml
binary_sensor:
  - platform: gpio
    id: limit_close
    name: "Roleta - Pozycja Zamknięta"
    pin:
      number: GPIO32
      mode: INPUT_PULLUP
      inverted: true
    filters:
      - delayed_on: 100ms

  - platform: gpio
    id: limit_open
    name: "Roleta - Pozycja Otwarta"
    pin:
      number: GPIO33
      mode: INPUT_PULLUP
      inverted: true
    filters:
      - delayed_on: 100ms
```

---

## 📐 Kalibracja Rołety

### Procedura Kalibracji (Ważne!)

1. **Wstępne Ustawienie**
   ```yaml
   full_steps: 20000  # Domyślna wartość
   ```

2. **Wgranie Firmware**
   ```bash
   esphome run esphome/rolety-config.yaml
   ```

3. **Test w Home Assistant**
   - Otwórz kartę rołety
   - Przesuń suwak z **0% na 100%**
   - Obserwuj ruch i zmierz dystans

4. **Pomiar i Obliczenie**
   - Jeśli rołeta nie dojechała pełnie:
     ```yaml
     full_steps: 25000  # Zwiększ wartość
     ```
   - Jeśli rołeta zrobiła za dużo obrotów:
     ```yaml
     full_steps: 15000  # Zmniejsz wartość
     ```

5. **Powtórz Testy**
   - Wgraj nową konfigurację
   - Testuj aż do idealnego ruchu

### Formula Obliczenia

Dla silnika 28BYJ-48:
- **1 obrót = ~4096 kroków** (bez redukcji)
- Ze względu na redukcję wewnętrzną: dokładnie 4076 kroków na obrót

```
Jeśli roleta potrzebuje N obrotów:
full_steps = N × 4076
```

Przykład: 5 obrotów
```
full_steps = 5 × 4076 = 20380 ≈ 20000 (zaokrąglono)
```

---

## 🔧 Troubleshooting - Rozwiązywanie Problemów

### Brak Połączenia WiFi

**Symptomy:** Urządzenie nie łączy się do WiFi

**Rozwiązanie:**
1. Sprawdź SSID i hasło w `secrets.yaml`
2. Upewnij się, że WiFi pracuje na 2.4GHz (ESP32 nie wspiera 5GHz)
3. Zrestartuj router
4. Podświetl diagnostykę:
   ```bash
   esphome logs esphome/rolety-config.yaml
   ```

### Silnik Się Nie Rusza

**Symptomy:** Silnik cichy, brak ruchu

**Rozwiązanie:**
1. Sprawdź połączenia ULN2003:
   - GPIO13-27 podłączone do IN1-4?
   - GND podłączony?
2. Sprawdź zasilanie:
   - Voltaż na ULN2003 VCC = 5V?
   - Voltaż na silniku Red Pin = 5V?
3. Zmierz sygnały GPIO:
   ```bash
   esphome logs esphome/rolety-config.yaml
   ```
4. Przetestuj z Home Assistant:
   - Spróbuj otworzyć rołetę via UI

### Silnik Piszczy Bez Ruchu

**Symptomy:** Słychać pisk, ale silnik się nie kręci

**Rozwiązanie:**
1. Zmniejsz prędkość:
   ```yaml
   max_speed: 300 steps/s  # Z 500 na 300
   ```
2. Wgraj nową konfigurację
3. Jeśli dalej piszczy, zmniejsz dalej do 200 steps/s
4. Sprawdź połączenia silnika (kolory przewodów)

### ESP32 Resetuje Się Przy Starcie Silnika

**Symptomy:** ESP32 restartuje, logowanie przerywa się

**Rozwiązanie:**
1. **WAŻNE**: Podłącz mocniejszy zasilacz (min. 2A @ 5V)
2. Dodaj kondensator 1000µF (elektrolityczny):
   - Dodatek do +5V zasilania
   - Minusem do GND
3. Sprawdzić przewody zasilające na spadek napięcia
4. Upewnij się, że GND zasilacza połączony z GND ESP32

### Rołeta Się Grzeje

**Symptomy:** Silnik/sterownik gorący w dotyku

**Rozwiązanie:**
1. Upewnij się, że `sleep_when_done: true` jest w konfiguracji
2. Sprawdź, czy rołeta nie jest ograniczona w ruchu
3. Zmniejsz `max_speed` (zmniejszy prąd)
4. Wgraj firmware ponownie:
   ```bash
   esphome run esphome/rolety-config.yaml
   ```

### Silnik Działa, Ale Home Assistant Go Nie Widzi

**Symptomy:** ESPHome zgłasza się, ale rołeta nie pojawia się w HA

**Rozwiązanie:**
1. Sprawdź czy `api_encryption_key` jest prawidłowy
2. Dodaj urządzenie ręcznie w Home Assistant
3. Zrestartuj Home Assistant

### Zły Kierunek Otwarcia/Zamknięcia

**Symptomy:** Klikając "Otwórz", rołeta się zamyka

**Rozwiązanie Opcja 1** (YAML):
```yaml
cover:
  - platform: stepper
    direction_inverted: true
```

**Rozwiązanie Opcja 2** (Fizyczne - zamiana pinów):
```yaml
stepper:
  - platform: uln2003
    pin_a: GPIO27      # było pin_d
    pin_b: GPIO14      # było pin_c
    pin_c: GPIO12      # było pin_b
    pin_d: GPIO13      # było pin_a
```

---

## 📚 Zaawansowana Konfiguracja

### Automatyzacja - Zamknięcie o Zachodzie Słońca

```yaml
automation:
  - id: close_blind_at_sunset
    alias: "Zamknij Roletę o Zachodzie"
    trigger:
      - platform: sun
        event: sunset
        offset: "-00:30:00"  # 30 min przed zachodem
    action:
      - service: cover.close_cover
        target:
          entity_id: cover.roleta_okno
```

### Automatyzacja - Otwórz o Wschodzie Słońca

```yaml
automation:
  - id: open_blind_at_sunrise
    alias: "Otwórz Roletę o Wschodzie"
    trigger:
      - platform: sun
        event: sunrise
        offset: "+01:00:00"  # 1 godzinę po wschodzie
    action:
      - service: cover.open_cover
        target:
          entity_id: cover.roleta_okno
```

### Wiele Rolet Jednocześnie

W `esphome/rolety-config.yaml` dodaj drugi `stepper` i `cover`:

```yaml
stepper:
  - platform: uln2003
    id: stepper_1
    pin_a: GPIO13
    pin_b: GPIO12
    pin_c: GPIO14
    pin_d: GPIO27
    max_speed: 500 steps/s
    sleep_when_done: true

  - platform: uln2003
    id: stepper_2
    pin_a: GPIO25
    pin_b: GPIO26
    pin_c: GPIO32
    pin_d: GPIO33
    max_speed: 500 steps/s
    sleep_when_done: true

cover:
  - platform: stepper
    id: roleta_salon
    name: "Roleta Salon"
    stepper_id: stepper_1
    full_steps: 20000

  - platform: stepper
    id: roleta_sypialnia
    name: "Roleta Sypialnia"
    stepper_id: stepper_2
    full_steps: 18000
```

---

## 📊 Monitorowanie i Diagnostyka

### Podgląd Logów

```bash
esphome logs esphome/rolety-config.yaml
```

Oczekiwane komunikaty:
```
[I] [esphome.core] ESPHome 2024.1.0
[I] [esphome.components.wifi] WiFi Connected
[I] [esphome.components.api] API Server started
[I] [cover.stepper] Stepper initialized
```

### Podgląd Web Dashboard

```bash
esphome dashboard esphome/
```

Otwórz w przeglądarce: `http://localhost:6052`

---

## 🆘 Dodatkowa Pomoc

### Sprawdzanie Statusu Połączenia

```bash
esphome info esphome/rolety-config.yaml
```

### Czyszczenie Build'u

```bash
esphome clean esphome/rolety-config.yaml
```

### Pełna Kompilacja i Wgranie

```bash
esphome run esphome/rolety-config.yaml --upload-port /dev/ttyUSB0
```

(Zastąp `/dev/ttyUSB0` rzeczywistym portem urządzenia)

---

## 📖 Przydatne Linki

- [ESPHome Dokumentacja](https://esphome.io/)
- [Home Assistant Dokumentacja](https://www.home-assistant.io/)
- [Troubleshooting ESPHome](https://esphome.io/guides/faq.html)
- [Forum ESPHome](https://github.com/esphome/issues/discussions)

---

## ✅ Checklist Instalacji

- [ ] ESPHome zainstalowany
- [ ] Repozytorium sklonowane
- [ ] Plik `secrets.yaml` utworzony i uzupełniony
- [ ] Firmware skompilowany
- [ ] Firmware wgrany na ESP32
- [ ] WiFi połączone
- [ ] Home Assistant widzi urządzenie
- [ ] Rołeta kalibrowana
- [ ] Kierunek ruchu prawidłowy
- [ ] Zautomatyzowany harmonogram działania

---

**Ostatnia aktualizacja**: 2026-06-02  
**Wersja**: 1.0.0

