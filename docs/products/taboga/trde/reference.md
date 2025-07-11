# API Reference

## 📦 TDataExport Class

### ⚙️ Properties
| Property | Type | Description |
|----------|------|-------------|
| Format | LONG | Export format |
| Options | GROUP | Export options |
| Template | STRING | Template name |

### 🔧 Methods

#### 🚀 Core Methods
```clarion
Init()                          !Initialize exporter
SetFormat(LONG format)          !Set output format
Export(STRING filename)         !Export to file
```

## 🔍 Constants

### 📊 Export Formats
- FORMAT:Excel
- FORMAT:HTML 
- FORMAT:PDF
- FORMAT:XML
