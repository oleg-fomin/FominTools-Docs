# Handcode Implementation

## 🔧 Application Setup

Add to Global Embeds (After Global INCLUDEs):
```clarion
_TBARDllMode_    EQUATE(0)
_TBARLinkMode_   EQUATE(1)
INCLUDE('BARCODE.INC')
```

## 🪟 Window Implementation

### 1. Declare BarcodeClass
```clarion
! In Data Section
BC   BarcodeClass
```

### 2. Window Code Example
```clarion
! In ThisWindow.Init
BC.Init(window, ?Barcode)

! Drawing barcode
BC.Draw(BAR:Barcode, BAR:BarcodeType)

! Changing properties
BC.SetProperty(BC:HEIGHT, 50)
BC.Reset                     ! Required after property changes
BC.Draw(BAR:Barcode, BAR:BarcodeType)

! In ThisWindow.Kill
BC.Kill
```

## 📄 Report Implementation

```clarion
! In ThisReport.OpenReport (after parent call)
BC.Init(report, ?Barcode, ?detail)

! In ThisReport.TakeRecord (before parent call)
BC.Draw(BAR:Barcode, BAR:BarcodeType)

! In ThisReport.Kill (before parent call)
BC.Kill
```

**Important:** Always call BC.Reset before BC.Draw when changing properties.
