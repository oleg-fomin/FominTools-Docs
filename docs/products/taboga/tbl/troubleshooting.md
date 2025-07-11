# Troubleshooting Guide

## ⚠️ Common Issues

### 🎨 Drawing Issues
- **ThisWindow.Reset**: Drawing barcode in ThisWindow.Reset may crash when using FONTDIALOG
  - Solution: Draw barcode only when needed instead of Reset
  
- **Incorrect Text Display**: Barcode value printed under barcode is wrong
  - Solution: For template implementation, prefix field names with '!' (e.g., !BAR:Barcode)
  
- **Property Changes Not Working**: 
  - Don't set properties before calling Init (Init resets to defaults)
  - Always call Reset after changing properties, before Draw
  - Example sequence:
    ```clarion
    BC.Init(...)
    BC.SetProperty(...)
    BC.Reset
    BC.Draw(...)
    ```

- **XWidth Dimension Issues**:
  - Windows: Uses pixel rounding (1-19 mils = 1 pixel)
  - Reports: Always uses thousands of an inch
  - Higher resolution devices show more precise width changes

## 🔍 Error Codes

| Error Code | Value | Description | Solution |
|------------|-------|-------------|----------|
| BCError:NoError | 0 | Operation completed successfully | - |
| BCError:WindowNotInitialized | 1 | Invalid window/report reference | Check Init parameters<br>Verify window/report exists |
| BCError:BarcodeTypeNotSet | 2 | Missing or invalid barcode type | Check Draw parameters<br>Verify barcode type constant |
| BCError:BadCheckDigit | 3 | Invalid check digit provided | Either provide complete value with check digit<br>Or omit check digit for auto-calculation |
| BCError:InvalidValue | 4 | Data doesn't match barcode requirements | Check data length (e.g., UPC-A needs 11 digits)<br>Verify character set matches barcode type |
| BCError:OtherErrors | 5 | Special case errors | Example: UPC-E system number must be 0 or 1 |

## 💡 Best Practices

1. Always call Kill before procedure ends
2. Call Reset after any property changes
3. Verify data matches barcode type requirements
4. Test on target printer resolution
5. Handle error returns from Init/Reset or omit check digit for auto-calculation

