
```markdown
# Testy Jednostkowe TcUnit - FB_ALARM_LogTable

Dokumentacja testów jednostkowych dla **Function Block FB_ALARM_LogTable** w TwinCAT 3. Testy realizowane za pomocą frameworku **TcUnit**. [file:312]

## 📋 Przegląd testów

| Kategoria | Test Case | Cel | Asertywne sprawdzenia |
|-----------|-----------|-----|----------------------|
| **ActiveAlarms** | `ActiveAlarms01_aAlarmsActiveSetTrue` | Alarm w grupie 0 (ID=0) | `aActiveID[^0]=TRUE`, pozostałe `FALSE` |
| **ActiveAlarms** | `ActiveAlarms02_aAlarmsActiveSetTrue` | Alarm w grupie 1 (ID=32) | `aActiveID[^32]=TRUE`, pozostałe `FALSE` |
| **AlarmLogs** | `AlarmLogs01_ProperOrder` | Logowanie 3 alarmów (0,64,128) | Prawidłowa kolejność: 128→64→0 |
| **AlarmLogs** | `AlarmLogs02_ProperOrder` | Logowanie 3 alarmów (32,96,160) | Prawidłowa kolejność: 160→96→32 |

**Całkowita liczba testów**: **4** (wszystkie `TEST_ORDERED`).

## 🔧 TestInit (Setup)
```ST
METHOD PRIVATE TestInit
```

- **MEMSET** zeruje `aAlarms` i `stAlarmTable`.
- `stCTDateTime.strHMIDateTime := 'Alarm No.'`.
- **Inicjalizacja wiadomości**: `aMsgAux[nI].sTextMessage := TO_STRING(nI)` (0-255).


## 📊 Szczegółowy opis test cases

### **1. ActiveAlarms01_aAlarmsActiveSetTrue**

**Setup**: `aAlarms[^0].nMessage := 16#00000001` (bit 0 grupy 0 → ID=0)
**Oczekiwane**:

```
✅ aActiveID = TRUE
❌ aActiveID = FALSE[^1]
❌ aActiveID = FALSE (grupa 1, bit 1)[^2]
❌ aActiveID = FALSE (grupa 3, bit 31)[^3]
❌ aActiveID = FALSE (grupa 7, bit 0)[^4]
```

**Asertywne**: `AssertTrue()` z komunikatami błędów.

### **2. ActiveAlarms02_aAlarmsActiveSetTrue**

**Setup**: `aAlarms[^1].nMessage := 16#00000001` (bit 0 grupy 1 → ID=32)
**Oczekiwane**:

```
✅ aActiveID = TRUE[^5]
❌ aActiveID = FALSE
❌ pozostałe = FALSE
```


### **3. AlarmLogs01_ProperOrder**

**Setup**: Alarmy ID **0** (gr0), **64** (gr2), **128** (gr4)
**Oczekiwane logi** (od najnowszego):

```
aLogMessages = "Alarm No. 128"[^1]
aLogMessages = "Alarm No. 64"[^6]
aLogMessages = "Alarm No. 0"[^7]
```

**Asertywne**: `AssertEquals_STRING()`.

### **4. AlarmLogs02_ProperOrder**

**Setup**: Alarmy ID **32** (gr1), **96** (gr3), **160** (gr5)
**Oczekiwane**:

```
aLogMessages = "Alarm No. 160"[^1]
aLogMessages = "Alarm No. 96"[^6]
aLogMessages = "Alarm No. 32"[^7]
```


## ✅ Pokrycie testami (Code Coverage)

| Funkcjonalność FB | Status | Test Case |
| :-- | :-- | :-- |
| Bitowe skanowanie `aActiveID` | ✅ | ActiveAlarms01/02 |
| `MEMCMP` detekcja zmian | ✅ | AlarmLogs01/02 |
| Shift logów `MEMMOVE` | ✅ | AlarmLogs01/02 |
| `bUpdateAlarmLogTable` | ✅ | AlarmLogs |
| **Testowane grupy**: 0,1,2,3,4,5 | **6/8 grup** | - |
| `bResetActive=TRUE` | ❌ **Brak** | - |
| `aAlarmsSettings` (auto-reset) | ❌ **Brak** | - |

## 🚀 Uruchomienie testów

```
1. Compile projekt TcUnit
2. Run → TcUnit → Execute Tests
3. Sprawdź raport w TcUnit View
```


## 📈 Zalecenia rozwoju testów

```
❌ Dodaj testy:
  - bResetActive=TRUE (blokada logowania)
  - aAlarmsSettings.aAutoResetTable (alarm vs warning)
  - Banner aktywnych alarmów (max 15)
  - Edge case: 256 aktywnych alarmów
  - TestInit weryfikacja zerowania
```

**Ostatnia aktualizacja**: 25.03.2026 | **Pokrycie**: ~60% core logiki [file:312]

```

Kopiuj do **README.md** – gotowe do GitHub/Wiki![^8]


<div align="center">⁂</div>

[^1]: https://www.gov.pl/web/kolumbia/apostille
[^2]: https://bip.warszawa.so.gov.pl/artykul/222/69/uwierzytelnienie-dokumentow
[^3]: https://parenting.pl/skrocony-odpis-aktu-urodzenia-przepisy-zupelny-odpis-aktu-urodzenia-wniosek
[^4]: https://gotwincat.blogspot.com/2017/
[^5]: https://notariusz-duszkiewicz.pl/klauzula-apostille-legalizacja-uwierzytelnienie/
[^6]: https://www.youtube.com/watch?v=_IwzqUg6c9w
[^7]: https://prawodlakazdego.pl/content/pe%C5%82nomocnictwo-od-osoby-przebywaj%C4%85cej-za-granic%C4%85
[^8]: FB_ALARM_LogTableTests.TcPOU```

