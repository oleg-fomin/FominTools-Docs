# Class Reference

## 📦 TWorkbook Class

### ⚙️ Properties

| Property | Type | Description |
|----------|------|-------------|
| Style | TStyle | Current formatting settings |
| ActiveSheet | LONG | Current worksheet index |
| RowHeight | REAL | Default row height |
| ColWidth | REAL | Default column width |

### 🔧 Methods

#### 🚀 Initialization
```clarion
Init()                  !Initialize workbook
Kill()                  !Clean up resources
Save(STRING filename)   !Save to disk
```

#### 📝 Data Entry
```clarion
TakeLabel(STRING ref, STRING text)
TakeNumber(STRING ref, REAL value)
TakeInteger(STRING ref, LONG value)
TakeDate(STRING ref, LONG date)
TakeTime(STRING ref, LONG time)
```

#### 🎨 Formatting
```clarion
SetFont(STRING name, <opt> LONG size, <opt> LONG color)
SetAlignment(LONG align)
SetVerticalAlignment(LONG align)
SetFormatPicture(STRING format)
ResetStyle()
```

## 🔍 Constants

### ↔️ Alignment
- xls::ALIGN_LEFT
- xls::ALIGN_CENTER
- xls::ALIGN_RIGHT
- xls::ALIGN_JUSTIFY

### ↕️ Vertical Alignment
- xls::VERT_ALIGN_TOP
- xls::VERT_ALIGN_CENTER
- xls::VERT_ALIGN_BOTTOM
