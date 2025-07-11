# Template Implementation

## 🚀 Basic Usage
Add barcode control templates to any window or report detail band.

## ⚙️ Template Properties

### 🎯 Barcode Configuration
- **Barcode Symbology**: Choose specific type or "Use Field" for dynamic selection from a field
- **Field that Holds Symbology**: When "Use Field" selected, specify which field contains the barcode type
- **Barcode Value**: 
  - Direct value: Enter the exact barcode data
  - Field reference: Prefix with '!' (e.g., !BAR:Barcode)
- **Draw Value Under Barcode**: Show human-readable text (uses DrawText property)
- **Wide to Narrow Ratio**: Ratio for specific symbologies (2-3 recommended):
  - CODE39 and Extended
  - Interleaved 2 of 5
  - Code 2 of 5

### 📐 Size and Position
- **Fixed Bar Width**: 
  - By default: Auto-adjusts to control width
  - When enabled: Uses specified width
  - Units: Mils (1/1000 inch) or pixels
- **Force Pixels**: 
  - Windows only: Convert width to pixels
  - Reports: Always uses mils
- **Clear Areas**: 
  - Left/Right: Quiet zones in narrow bars
  - Top/Bottom/Middle: Reserved for future use

### 🔄 Data Options
- **Draw Start/Stop Character**: 
  - For CODE39/Extended only
  - Requires DrawText=TRUE
  - Shows start/stop characters in text
- **Calculate Check Digits**: 
  - For optional check digit types:
    - CODE39 (Standard/Extended)
    - Interleaved 2 of 5
    - Code 2 of 5
    - CODABAR
  - Omit check digit in data for auto-calculation

### 🪟 Window Options
- **Draw Upon Entry**: 
  - Automatically draw in ThisWindow.Init
  - Caution with FONTDIALOG usage

## 💡 Implementation Notes

1. Field References
   - Always use '!' prefix for field names
   - Example: !BAR:Barcode not BAR:Barcode

2. Property Dependencies
   - Start/Stop requires DrawText=TRUE
   - Fixed width overrides automatic sizing
   - Force Pixels only works with Fixed Bar Width

3. Check Digit Handling
   - Optional: Let library calculate
   - Manual: Include in barcode data
   - UPC/EAN: Always required

**Note:** Always prefix field names with '!' in the Barcode Value property.
