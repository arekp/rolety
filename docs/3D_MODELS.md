# Modele 3D - Drukowanie i Montaż

## 📋 Przegląd Dostępnych Modeli

Projekt Rolety oferuje kilka gotowych do druku modeli 3D z Thingiverse, które ułatwiają montaż silnika, przekładni i roletki.

---

## 🖨️ Model 1: 28BYJ-48 Gearbox Mount

**Źródło**: [Thingiverse Thing #4792584](https://www.thingiverse.com/thing:4792584)

### Opis
Uchwyt montażowy specjalnie zaprojektowany do mocowania silnika 28BYJ-48 z wałem transmisyjnym. Umożliwia bezpieczne przymocowanie silnika do profilu aluminiowego.

### Parametry Druku
```
Material:        PLA lub PETG (zalecane PETG - wytrzymalsze)
Temperature:     200-210°C (PLA) / 230-250°C (PETG)
Bed Temperature: 60°C (PLA) / 80°C (PETG)
Print Speed:     50 mm/s
Infill:          20% (Gyroid)
Support:         TAK - zalecany
Build Time:      ~4-6 godzin
Waga materiału:  ~50g
```

### Komponenty do Zestawienia
- 1x Wydruk 3D (obudowa uchwytu)
- 2x Śruba M4×16
- 2x Nakrętka M4
- 4x Podkładka M4
- 1x Elastyczne sprzęgło (6-12mm)

### Instrukcja Montażu

#### Krok 1: Przygotowanie
```
1. Wydrukuj model ze wsporami
2. Usuń podpory ostrożnie (używaj narzędzia do drukowania)
3. Sprawdzić szczeliny na śruby M4
```

#### Krok 2: Montaż Silnika
```
1. Włóż silnik 28BYJ-48 do uchwytu
2. Wstaw śruby M4 przez boczne otwory
3. Dokręć nakrętkami M4 na krzyż (nie całkowicie)
4. Wyrównaj oś silnika
5. Dokręć całkowicie (moment ~2 Nm)
```

#### Krok 3: Podłączenie Wału
```
1. Osadź elastyczne sprzęgło na wału silnika
2. Podłącz do wału głównego rolatki
3. Sprawdzić luz - maksymalnie 2mm
```

### Potencjalne Problemy
| Problem | Przyczyna | Rozwiązanie |
|---------|-----------|------------|
| Śruba nie wpada | Wąskie szczeliny | Powiększ otwory (wiertła 4.5mm) |
| Luz przy montażu | Tolerancje | Dodaj podkładki M4 |
| Pęka podczas druku | Niewystarczająca wytrzymałość | Zwiększ infill do 30% |

### Alternatywy
- Druk z materiału TPU (elastomeru) dla większej nośności
- Druk z ABS dla temperatury otoczenia do 80°C
- Wzmocnienie gwoździkami lub klejem do żywicy

---

## 🖨️ Model 2: Stepper Motor Pulley Adapter

**Źródło**: [Thingiverse Thing #4865115](https://www.thingiverse.com/thing:4865115)

### Opis
Adapter przekładni (pulley) pośredniczący między wałem silnika 28BYJ-48 a wałkiem głównym rolatki. Umożliwia bezstopniowe osiągnięcie żądanego przełożenia.

### Parametry Druku
```
Material:        PLA (wystarczy)
Temperature:     200°C
Bed Temperature: 60°C
Print Speed:     60 mm/s
Infill:          25% (Linear)
Support:         TAK
Build Time:      ~3-4 godziny
Waga materiału:  ~35g
```

### Opcje Przełożenia

#### Wersja 1:1 (Bez Redukcji)
- Średnica silnika: 15mm
- Średnica wału: 15mm
- Zastosowanie: Lekkie rolety
- Moment obrotowy: Mały
- Prędkość: Duża

#### Wersja 2:1 (2x Redukcja)
- Średnica silnika: 15mm
- Średnica wału: 30mm
- Zastosowanie: Średnie rolety
- Moment obrotowy: 2x większy
- Prędkość: 2x wolniejsza

#### Wersja 3:1 (3x Redukcja)
- Średnica silnika: 15mm
- Średnica wału: 45mm
- Zastosowanie: Ciężkie rolety
- Moment obrotowy: 3x większy
- Prędkość: 3x wolniejsza

### Instrukcja Montażu

#### Krok 1: Przygotowanie
```
1. Wydrukuj puleję z wybranym przełożeniem
2. Usuń wszystkie podpory
3. Wygładź krawędzie (papier ścierny 120-200)
```

#### Krok 2: Montaż na Wale Silnika
```
1. Nasaż puleję na wał silnika
2. Wyrównaj wzdłuż osi
3. Zamocuj śrubą M3×10 z gniazdem imbusowym
4. Dokręć moment ~1 Nm
```

#### Krok 3: Podłączenie Paska/Łańcucha
```
Opcja A - Pas:
  1. Nasaż pas GT2 na puleję silnika
  2. Nasaż drugi koniec na puleję główną
  3. Napinacz: 1-2mm ugięcia przy naciśnięciu

Opcja B - Łańcuch:
  1. Zamocuj łańcuch #25 za pomocą spinek
  2. Napinacz: 5-10mm luzu
```

### Zalecane Materiały Łańcucha/Pasa
- **Pas GT2 6mm**: Cichy, dokładny, brak serwisu
- **Łańcuch #25**: Trwały, mocny, wymaga smarowania
- **Guma**: Najtańsza, mniej dokładna

---

## 🖨️ Model 3: Roller Blind Winding Drum

**Źródło**: [Thingiverse Thing #3923529](https://www.thingiverse.com/thing:3923529)

### Opis
Wałek zwijania (bęben) materialnych rolet. Umożliwia równomierne rozwijanie i zwijanie materiału. Wspiera różne średnice wałków (25-50mm).

### Parametry Druku
```
Material:        PETG (dla wytrzymałości)
Temperature:     240°C
Bed Temperature: 80°C
Print Speed:     40 mm/s (powolniej)
Infill:          30% (Gyroid)
Support:         TAK - Wielowarstwowy
Build Time:      ~8-12 godzin
Waga materiału:  ~150-200g
```

### Specyfikacja

#### Wymiary Bębna
- Średnica zewnętrzna: 50mm
- Średnica wału: 12mm (można powiększyć)
- Długość: 800mm (konfigurowalna)
- Waga maksymalna materiału: 5kg

#### Powierzchnia Zwijania
- Twarde karby powierzchni - zapobiega ślizganiu się materiału
- Równomierne rowki - dla równomiernego zwijania
- Dwie flansze boczne - utrzymują materiał w linii

### Instrukcja Montażu

#### Krok 1: Przygotowanie do Druku
```
1. Pobierz plik STL z Thingiverse
2. Dziel model na części (jeśli zbyt długi):
   - 2-3 części 300-400mm
   - Drukuj osobno
3. Dodaj otwory wklejowe (5-6mm głęb.)
```

#### Krok 2: Sklejanie Części
```
1. Użyj kleju do żywicy epoksydowej (2-komponentowy)
2. Nałóż cienką warstwę na część końcową
3. Wciśnij drugą część pod kątem prostym
4. Trzymaj 24 godziny w ściskach
5. Sprawdzić wyrównanie - bęben musi być prosty
```

#### Krok 3: Montaż w Profilu Aluminiowym
```
1. Zamontuj łożyska kulkowe (obie strony) w profilu
2. Włóż wał bębna do łożysk
3. Wyrównaj względem profilu
4. Przymocuj bęben śrubami M6 (opcjonalnie)
```

#### Krok 4: Przymocowanie Materiału
```
1. Wyrównaj wałek w poziomie (poziomnica)
2. Przymocuj początek materiału (wspornik lub klej)
3. Zawiń około 1m - sprawdzić czy równomiernie
4. Zamocuj koniec na karbach
```

### Materiały do Rolet

#### Tkanina Zaciemniająca
- **Grubość**: 0.3-0.5mm
- **Waga**: 250-400g/m²
- **Dostępny w**: Sklepy meblowe, allegro.pl
- **Alternatywa**: Materiał z materiałów tekstylnych

#### Materiał Transparentny
- **Grubość**: 0.2-0.3mm
- **Waga**: 100-150g/m²
- **Dostępny w**: Sklepy materiałów budowlanych

#### Siatka do Promieni Słonecznych
- **Grubość**: 0.3mm
- **Waga**: 120-200g/m²
- **Benefit**: Przezroczysta + zmniejsza temperaturę

### Potencjalne Problemy
| Problem | Przyczyna | Rozwiązanie |
|---------|-----------|------------|
| Materiał ślizga się | Zbyt mały nacisk | Zwiększ naciąg wałka |
| Nierówne zwijanie | Wałek nie wyrównany | Sprawdź poziomość |
| Pęknięcia na zgięciach | Zbyt szybkie drukowanie | Drukuj wolniej |
| Materiał się nie toczy | Wałek chropowaty | Wygładź papierem ściernym |

---

## 🖨️ Model 4: Control Box Enclosure

**Źródło**: [Thingiverse Thing #2392856](https://www.thingiverse.com/thing:2392856)

### Opis
Obudowa kontrolera do zamontowania na ścianie. Zawiera uchwyty dla ESP32, ULN2003 i zasilacza. Zapewnia ochronę przed kurzem i przypadkowym dotykiem.

### Parametry Druku
```
Material:        ABS lub PETG (odporna na promieniowanie UV)
Temperature:     240°C (ABS) / 240°C (PETG)
Bed Temperature: 100°C (ABS) / 80°C (PETG)
Print Speed:     40 mm/s
Infill:          20% (Honeycomb)
Support:         TAK - strukturalne
Build Time:      ~10-14 godzin
Waga materiału:  ~200g
```

### Komponenty Obudowy

#### Część Główna
- Wymiary zewnętrzne: ~200×150×100mm
- Wentylacja: 8-10 otworów 20mm
- Otwory montażowe: 4x M4

#### Pokrywa
- Wymiary: ~200×150×20mm
- Zawiasy: 2x (drukowane lub kupione)
- Zatrzask magnetyczny: 2x (opcjonalnie)

#### Uchwyty Wewnętrzne
- Szyna DIN: opcjonalnie do mocowania modułów
- Otwory M4 dla gumowych nóg
- Przewód prowadnic: dla kableli

### Instrukcja Montażu

#### Krok 1: Drukowanie i Montaż
```
1. Wydrukuj główną obudowę
2. Wydrukuj pokrywę osobno
3. Zainstaluj zawiasy drukowane lub kupione
4. Sprawdzić otwieranie pokrywy (powinno być gładkie)
```

#### Krok 2: Montaż Elektroniki Wewnątrz
```
1. Zainstaluj szynę DIN (jeśli dostępna)
2. Przymocuj ESP32 za pomocą śrub M4 i dystansów
3. Przymocuj ULN2003 w pobliżu ESP32
4. Zainstaluj zasilacz w dolnej części
```

#### Krok 3: Piny Wejścia/Wyjścia
```
1. Wywiercić otwory Ø20mm na boku obudowy
2. Zainstalować grommetki gumowe
3. Przywlec kable z silnikiem przez otwory
4. Zainstalować złączki XLR lub USB (opcjonalnie)
```

#### Krok 4: Montaż na Ścianie
```
1. Zaznacz położenie na ścianie
2. Wywiercić otwory M4 (lub M5)
3. Zainstaluj kołki i śruby ścienne
4. Przymocuj obudowę 4 śrubami
5. Sprawdzić czy poziom
```

### Schemat Wewnętrzny

```
┌─────────────────────────────────────┐
│      OBUDOWA KONTROLERA             │
│                                     │
│  ┌──────────────┐                   │
│  │   ESP32      │                   │
│  │              │     Szyna DIN     │
│  └──────────────┘     (opcja)       │
│                                     │
│  ┌──────────────┐                   │
│  │   ULN2003    │                   │
│  │   Driver     │                   │
│  └──────────────┘                   │
│                                     │
│  ┌──────────────┐                   │
│  │  Zasilacz    │                   │
│  │   5V / 12V   │                   │
│  └──────────────┘                   │
│                                     │
│  [Wentylacja]  [Przewód]  [Masa]   │
└─────────────────────────────────────┘
```

### Opcjonalne Ulepszenia

#### Wentylator Aktywny
```yaml
fan:
  - platform: gpio
    pin: GPIO4
    name: "Wentylator Obudowy"
```

#### Sensor Temperatury
```yaml
sensor:
  - platform: dht
    pin: GPIO5
    name: "Temperatura Obudowy"
```

#### LED Status
```yaml
light:
  - platform: gpio
    pin: GPIO2
    name: "Dioda Status"
```

---

## 🔨 Narzędzia Potrzebne do Druku 3D

### Do Obsługi Modeli
- Nóż/skalpel - Usuwanie podporów
- Papier ścierny - 120, 200, 400 grit
- Klej do żywicy epoksydowej - Do sklejania części
- Narzędzie do czyszczenia dysz - Utrzymanie drukarki
- Pęseta - Precyzyjne usuwanie drukowanek

### Przed Drukiem
- Kalibracja łóżka drukowania
- Czyszczenie dyszy (180°C)
- Sprawdzenie filamentu na pęknięcia

---

## 📊 Szacunkowe Koszty Druku

| Model | Materiał | Gram | Cena (PLN) |
|-------|----------|------|-----------|
| Mount Silnika | PETG | 50 | 5-10 |
| Pulley Adapter | PLA | 35 | 3-5 |
| Bęben Rolatki | PETG | 180 | 20-30 |
| Obudowa Kontrolera | ABS | 200 | 20-30 |
| **RAZEM** | - | **465** | **48-75** |

*Cena jednostkowa filamentu: ~0.10-0.15 PLN/gram*

---

## ⚠️ Wskazówki Bezpieczeństwa

1. **Druk 3D:**
   - Wentylacja pomieszczenia (zarówno do druku ABS)
   - Unikaj wdychania oparów nad drukowaną niezaszczepionym
   - Nie dotykaj dyszy podczas druku (gorąca!)

2. **Obróbka:**
   - Ostrze skalpela - Używaj ostrożnie
   - Papier ścierny - Maski ochronne (pył PLA/PETG)
   - Klej epoksydowy - Używaj rękawic, unikaj skóry

3. **Montaż:**
   - Śruby - Dokręcaj równomiernie (nie całkowicie na raz)
   - Wał - Sprawdzaj pracę bez zasilania
   - Wentylacja obudowy - Nie blokuj otworów

---

## 🎓 Gdzie Znaleźć Inne Modele

- [Thingiverse - Roller Blinds](https://www.thingiverse.com/search?q=roller%20blind&type=things&sort=relevant)
- [Printables - Roller Blinds](https://www.printables.com/search/roller%20blind)
- [MyMiniFactory - Automation](https://www.myminifactory.com/search?q=stepper+motor)
- [GitHub - Home Automation](https://github.com/topics/home-automation)

---

## 📞 Wsparcie i Problemy

Jeśli napotkasz problemy z drukiem:
1. Sprawdź komentarze na Thingiverse
2. Przeskaluj model (90%, 110%)
3. Zmień orientację drukowania
4. Skontaktuj się z autorem modelu

---

**Ostatnia aktualizacja**: 2026-06-02  
**Wersja**: 1.0.0

