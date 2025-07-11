# Workbook Creation

## 📖 Basic Concepts

The TNativeExcel library builds Excel workbooks entirely in memory before saving to disk. All operations are performed on a TWorkbook object which represents your Excel file.

## 🚀 Quick Start

```clarion
PROGRAM
  MAP
  END

INCLUDE('TWorkbook.INC')
wBook      TWorkbook

  CODE
  wBook.Init()                              !Initialize workbook
  wBook.TakeLabel('A1','Hello World')       !Write text to cell A1
  wBook.TakeNumber('B1',42.5)              !Write number to cell B1
  wBook.Save('example.xls')                 !Save to disk
  wBook.Kill()                              !Clean up
```

## 🔧 Core Operations

### 📝 Data Entry
- `TakeLabel()` - Write text to a cell
- `TakeNumber()` - Write numbers with decimal places
- `TakeInteger()` - Write whole numbers
- `TakeDate()` - Write dates
- `TakeTime()` - Write times

### 🎯 Cell References
Cells can be referenced either by:
- Excel-style references (e.g. 'A1', 'B2')
- Row/Column pairs (e.g. 1,1 for cell A1)

### 💾 Memory Management
The workbook is built entirely in memory until saved. Always call:
1. `Init()` to initialize
2. `Save()` to write to disk
3. `Kill()` to clean up
