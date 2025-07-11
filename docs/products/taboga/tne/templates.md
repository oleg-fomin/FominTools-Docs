# Template Implementation

## 📋 Overview

TNativeExcel includes templates for both ABC and Legacy chains, making Excel file generation simple in any Clarion application.

## 🌐 Global Extension

Add TNativeExcel support to your application using the global extension:

```clarion
[APPLICATION]
TEnableNativeExcel=1
```

## 🪟 Window Control Templates

### 📤 Excel Export Control
Adds Excel export capability to any browse or report:

- **Properties**
    - Target File: Output filename or prompt
    - Auto Format: Apply default formatting
    - Column Mapping: Configure data columns
    - Export Options: Headers, formatting, sheets

### 👁️ Excel Preview Control
Preview Excel output before saving:

- **Properties**
    - Preview Mode: Screen/Print/Both
    - Auto Size: Adjust columns
    - Zoom Level: Initial zoom

## 💡 Implementation Examples

```clarion
! Basic browse export
ExcelExport CLASS
  CODE
  SELF.Init()
  SELF.SetFile('output.xls')
  SELF.ExportBrowse()
```

## ⭐ Best Practices

- Use consistent column formats
- Set appropriate column widths
- Apply header formatting
- Include totals where relevant
