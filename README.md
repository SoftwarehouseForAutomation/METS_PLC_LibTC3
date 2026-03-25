# TcUnit Test Suite: F_GetItemAITests

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TcUnit](https://img.shields.io/badge/Tested-TcUnit-blue.svg)](https://tcunit.org/)
[![TwinCAT3](https://img.shields.io/badge/TwinCAT-3.1%2B-orange.svg)](https://www.beckhoff.com/)

**Kompleksowy zestaw testów jednostkowych TcUnit dla funkcji `F_GetItemAI`** – parsowanie kodów AI (GTIN, SERIAL, RSKU, BATCH, PO) z kodów kreskowych CASE/OUTER/PACK.

## 📋 Spis treści
- [Funkcje](#funkcje)
- [Testowane przypadki](#testowane-przypadki)
- [Wymagania](#wymagania)
- [Instalacja](#instalacja)
- [Uruchomienie testów](#uruchomienie-testow)
- [Struktura testów](#struktura-testow)
- [Rozwiązywanie problemów](#rozwiazywanie-problemow)
- [Wkład](#wklad)
- [Licencja](#licencja)

## 🚀 Funkcje
- **14 testów** dla kodów CASE (GTIN, SERIAL, RSKU, BATCH, PO – w tym VAL2)
- **3 testy** dla kodów OUTER (GTIN, SERIAL, RSKU)
- **1 test** dla kodu PACK (SERIAL)
- **Pełne komunikaty błędów** w ADS Log (np. "Wrong Case GTIN extracted")
- Walidacja pozycji AI, długości i ekstrakcji danych

## 📋 Wymagania
- **TwinCAT 3.1** (build 4024+)
- **TcUnit v2.0+** (biblioteka)
- **ADS Logger** włączony w Output
- Funkcja `F_GetItemAI` z biblioteki `CODE`

## 📦 Instalacja
1. Dodaj plik `F_GetItemAITests.TcPOU` do projektu PLC:
PLC/
├── Tests/
│ └── F_GetItemAITests.TcPOU // FBTestSuite
└── CODE/ // F_GetItemAI + typy ECodeAI/STCodeInfo

2. Library Manager → **Dodaj TcUnit**
3. **Build Solution** (F7)

## ⚙️ Uruchomienie testów
```pascal
// W TcUnit Runner
xenRunTests := TRUE;
diTimeout   := 5000;  // 5s

Wyniki w Output → ADS:
Test assert message=Wrong Case GTIN extracted
Test assert type=AssertEquals_STRING
==========TESTS FINISHED RUNNING==========
📁 Struktura testów
F_GetItemAITests (FBTestSuite)
├── 01.CASE Code (8 testów)
│   ├── Casecode01GTINextractedWrong
│   ├── Casecode02SERIALextractedWrong
│   ├── Casecode03RSKUextractedWrong
│   ├── Casecode04BATCHextractedWrong
│   ├── Casecode05POextractedWrong
│   ├── Casecode06GTINVAL2extractedWrong
│   └── Casecode07SERIALVAL2extractedWrong
├── 02.Outer Code (3 testy)
│   ├── Outercode01GTINextractedWrong
│   ├── Outercode02SERIALextractedWrong
│   └── Outercode03RSKUextractedWrong
└── 03.Pack Code (1 test)
    └── Packcode01SERIALextractedWrong

Przykład testu (Casecode01GTINextractedWrong):
METHOD Casecode01GTINextractedWrong
VAR
    sFullCode : TMaxString := '01175010317110522186174He081137058500240101082451086352024915304012';
    eCodeType : CODE.ECodeAI := CODE.ECodeAI.GTIN;
    stCodeInfo : CODE.STCodeInfo;
    sFGetItemAI : TMaxString;
END_VAR

TestInit(eCodeType, sFullCode, stCodeInfo);
sFGetItemAI := CODE.F_GetItemAI(sFullCode, eCodeType, stCodeInfo);

AssertEquals_STRING(
    Expected := '17501031711052',
    Actual   := TO_STRING(sFGetItemAI),
    Message  := 'Wrong Case GTIN extracted'
);
TEST_FINISHED;
🛠️ Rozwiązywanie problemów
| Problem                 | Rozwiązanie                        |
| ----------------------- | ---------------------------------- |
| Failed test bez message | Dodaj Message := 'opis' do Assert* |
| Test nie startuje       | xenRunTests := TRUE w Runner       |
| ADS Log pusty           | Output → ADS → TcUnit channel      |
| Błąd kompilacji         | TcUnit library + typy CODE         |

🤝 Wkład
Fork repozytorium

Dodaj nowe testy do F_GetItemAITests

Pull Request z opisem nowych przypadków

📄 Licencja
Ten projekt używa licencji MIT.
© 2026 [Twoje Imię/Nazwa firmy]