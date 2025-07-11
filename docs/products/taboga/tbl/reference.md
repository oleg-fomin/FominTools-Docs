# Class Reference

## 🔧 Properties

### Display Properties
- **BarColor**: Color of barcode bars (inherits from string field text color)
- **BackColor**: Background color (inherits from string field)
- **DrawText**: Show human-readable text beneath barcode
- **myFont**: Text font (inherits from string field)
- **myFontSize**: Text size (inherits from string field)
- **Wide2NarrowRatio**: For CODE39/INT2OF5 (default: 2)
- **XWidth**: Narrow bar width (1/1000 inch or pixels)
- **ForcePixels**: Use pixels for XWidth (windows only)

### Clear Areas (in narrow bars)
- **ClearAreaLeft**: Left margin
- **ClearAreaRight**: Right margin
- **ClearAreaTop**: Top margin (not implemented)
- **ClearAreaBottom**: Bottom margin (not implemented)
- **ClearAreaMiddle**: Middle spacing (not implemented)

### Data Properties
- **PrintStartStop**: Show start/stop for CODE39
- **AddCheckDigit**: Auto-calculate check digits

## 📡 Methods

### Init
```clarion
Init(WINDOW theWindow, UNSIGNED theControl, UNSIGNED theBand=0, BYTE barType=BARCODE:UPC_A)
```

### Reset and Kill
```clarion
Reset        ! After changing properties
Kill         ! Cleanup
```

### Draw
```clarion
Draw(STRING bc_string, <BYTE bc_type>)
```

## ⚠️ Error Codes
```clarion
BCError:NoError              EQUATE(0)
BCError:WindowNotInitialized EQUATE(1)
BCError:BarcodeTypeNotSet    EQUATE(2)
BCError:BadCheckDigit        EQUATE(3)
BCError:InvalidValue         EQUATE(4)
BCError:OtherErrors          EQUATE(5)
```
```
Draw barcode with:
- bc_string: Data to encode
- bc_type: Barcode type (optional)

## Error Codes

```clarion
BCError:NoError                 EQUATE(0)   ! Success
BCError:WindowNotInitialized    EQUATE(1)   ! Invalid window/report reference
BCError:BarcodeTypeNotSet       EQUATE(2)   ! Missing barcode type
BCError:BadCheckDigit           EQUATE(3)   ! Invalid check digit provided
BCError:InvalidValue            EQUATE(4)   ! Invalid data for barcode type
BCError:OtherErrors             EQUATE(5)   ! Other errors (e.g., invalid UPC-E system)
```
