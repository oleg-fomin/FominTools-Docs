# Cell Formatting

## 🔤 Font Formatting

### ✍️ Setting Fonts
```clarion
wBook.Style.FontName = 'Arial'          !Font family
wBook.Style.FontSize = 10               !Font size in points
wBook.Style.FontColor = COLOR:BLUE      !Standard Clarion colors
wBook.Style.FontStyle = FONT:bold       !Font styles
```

### ↔️ Text Alignment
```clarion
wBook.SetAlignment(xls::ALIGN_RIGHT)           !Horizontal alignment
wBook.SetVerticalAlignment(xls::VERT_CENTER)   !Vertical alignment
```

## 🔢 Number Formats

TNE supports both Clarion and Excel format strings:

```clarion
!Clarion style
wBook.SetFormatPicture('@N10.2')    !Numbers with 2 decimals
wBook.SetFormatPicture('@D2')       !Short date format

!Excel style
wBook.SetFormatPicture('#,##0.00')  !Numbers with thousand separator
wBook.SetFormatPicture('mm/dd/yy')  !Date format
```

## 🎨 Style Management

All formatting remains active until explicitly reset:

```clarion
wBook.ResetStyle()                  !Reset all formatting to defaults
```
