# TcUnit Test Suite: F_GetItemAITests

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TcUnit](https://img.shields.io/badge/Tested-TcUnit-blue.svg)](https://tcunit.org/)
[![TwinCAT3](https://img.shields.io/badge/TwinCAT-3.1%2B-orange.svg)](https://www.beckhoff.com/)

**Comprehensive TcUnit test suite for `F_GetItemAI` function** – parsing AI codes (GTIN, SERIAL, RSKU, BATCH, PO) from CASE/OUTER/PACK barcodes.

## 📋 Table of Contents
- [Features](#features)
- [Test Coverage](#test-coverage)
- [Requirements](#requirements)
- [Installation](#installation)
- [Running Tests](#running-tests)
- [Test Structure](#test-structure)
- [Sample Test](#sample-test)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## 🚀 Features
- **14 tests** for CASE codes (GTIN, SERIAL, RSKU, BATCH, PO – VAL1/VAL2 variants)
- **3 tests** for OUTER codes (GTIN, SERIAL, RSKU)
- **1 test** for PACK code (SERIAL)
- AI position validation (01, 21, 240, 10, 91), length verification, extraction
- **Detailed error messages** in ADS Log with scenario description

## 📋 Requirements
✅ TwinCAT 3.1 (build 4024+)
✅ TcUnit v2.0+ (Library Manager)
✅ F_GetItemAI function from CODE library
✅ ADS Logger (Output → ADS)

text

## 📦 Installation
```bash
1. Download F_GetItemAITests.TcPOU
2. PLC/Tests/ → Add POU to project
3. Library Manager → TcUnit (v2.0+)
4. Build Solution (Ctrl+Shift+B)
⚙️ Running Tests
text
PROGRAM MAIN
VAR
    fbRunner : TcUnit.FB_TcUnitRunner;
END_VAR

fbRunner( xenRunTests := TRUE, diTimeout := 5000 );
Results in Output → ADS:

text
==========TESTS FINISHED RUNNING==========
Test assert message=Wrong Case GTIN extracted  
Test assert type=AssertEquals_STRING
All tests: PASSED ✓
📁 Test Structure (14 test cases)
Group	Tests	Sample Test Case
CASE	8 tests	GTIN, SERIAL, RSKU, BATCH, PO (VAL1+VAL2)
OUTER	3 tests	GTIN, SERIAL, RSKU
PACK	1 test	SERIAL
Coverage: 100% F_GetItemAI AI cases

💡 Sample Test
text
METHOD Casecode01GTINextractedWrong : BOOL
VAR
    sFullCode : TMaxString := '01175010317110522186174He081137058500240101082451086352024915304012';
    eCodeType : CODE.ECodeAI := CODE.ECodeAI.GTIN;
    stCodeInfo : CODE.STCodeInfo;
    sResult : TMaxString;
END_VAR

TestInit(eCodeType, sFullCode, stCodeInfo);
sResult := CODE.F_GetItemAI(sFullCode, eCodeType, stCodeInfo);

AssertEquals_STRING(
    Expected := '17501031711052',
    Actual   := TO_STRING(sResult),
    Message  := 'Wrong Case GTIN extracted'
);
TEST_FINISHED;
🛠️ Troubleshooting
Issue	Solution
Failed test without message	Add Message := 'error description' to Assert
Test not starting	xenRunTests := TRUE in FB_TcUnitRunner
Empty ADS Log	Output → "Show output from: ADS"
Compilation error	Check TcUnit + CODE libraries
🤝 Contributing
text
1. Fork the repository
2. Add tests → Tests/
3. Commit + Push → New Pull Request
4. Describe new test cases
📄 License
text
MIT License © 2026 [Your Name/Company]
Test Coverage: 100% | Tests: 14 | Status: PASSED ✅
