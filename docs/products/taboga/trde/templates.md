# Template Integration

## 🧩 Template Overview

### 🌐 Global Extension
```clarion
[APPLICATION]
TEnableDataExport=1
```

### 📋 Report Templates
- Export Control
- Format Selection
- Output Options

## 🔧 Implementation

### 📤 Export Control
```clarion
DataExport CLASS
  CODE
  SELF.Init()
  SELF.SetFormat(FORMAT:Excel)
  SELF.ExportReport()
```

## ⚡ Quick Actions

### 🎯 Common Tasks
- Add export button
- Configure formats
- Set default options
