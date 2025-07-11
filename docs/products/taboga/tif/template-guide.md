# Template Guide

This guide covers the complete template system for the Taboga Interactive Filter, based on the actual template implementation.

## 🔧 Template Architecture

### 📋 Extension Template

The `TEnableInteractiveFilter` extension must be applied at the application level:

```clarion
#EXTENSION(TEnableInteractiveFilter,'[T] Enables "InteractiveFilter" functionality on the application/DLL...')
```

### 🎯 Control Templates

Two control templates are available:

1. **TInteractiveFilter** - For ABC applications
2. **TInteractiveFilterLegacy** - For Legacy Clarion applications

## 🚀 Extension Configuration

### 📦 Application Setup

#### 🌐 Multi-Language Support

```clarion
! Interface language configuration
Interface File: TInteractiveFilter_EN.GUI
Translation File: TInteractiveFilter_EN.TRN
System Language: EN
```

#### 🔧 Multi-DLL Configuration

For multi-DLL projects:

```clarion
☑ Generate TInteractiveFilterClass as EXTERNAL
  External Source: Dynamic Link Library (DLL)
  
! Results in compilation symbols:
_TIntFilterLinkMode_=>0;_TIntFilterDllMode_=>1
```

**DLL Export Options** (when creating DLL):
```clarion
☑ Export TInteractiveFilterClass
☑ Export ABC PopupClass  ! If other apps don't use ABC
```

## 🎛️ Control Template Configuration

### 📊 Query Tab Options

#### 🎯 Popup Menu Items

```clarion
☑ Filter by selection              ! Right-click equals filtering
☑ Filter excluding selection       ! Right-click not-equals filtering
☑ Filter for                      ! Advanced filter dialog
☑ Remove Filter                   ! Clear all filters
☑ Contains/Does Not Contain Pair  ! Context-sensitive options
```

#### 🔤 Case Sensitivity

```clarion
◯ Force Case Sensitive           ! Exact case matching
● Force Case Insensitive         ! Ignore case (default)
◯ Manual (driver-dependent)      ! Auto-detect based on driver
```

**Generated Code:**
```clarion
! Case insensitive (0)
TIF2.CaseSensitivity = 0

! Case sensitive (1)  
TIF2.CaseSensitivity = 1

! Manual/Auto-detect (2)
TIF2.CaseSensitivity = 2
```

#### 💾 Filter Persistence

```clarion
☑ Allow saving and loading filters
  Save procedure: TInteractiveFilter_SaveFilter
  Load procedure: TInteractiveFilter_LoadFilter
```

#### 🖥️ Filter Display

```clarion
☑ Display filter being applied
  Filter Result Control: ?FilterDisplay
```

### 📋 Fields Tab Configuration

#### 🤖 Auto-Population (Recommended)

```clarion
☑ Auto Populate from BrowseBox
```

**Field Override Options:**
```clarion
! For each browse column:
Field: CUS:Company
  ☐ Always skip this field
  ☑ Override field header
    Title: Company Name
```

**Generated Override Code:**
```clarion
! Skip field entirely
SELF.AddItemOverride('CUS:CustNumber','',TRUE)

! Override title only
SELF.AddItemOverride('CUS:Company','Company Name',FALSE)
```

#### 📝 Manual Field Configuration

```clarion
! Manual field addition:
Field: CUS:Company
Title: Company Name  
Picture: @s20
☑ Caseless  ! For string fields
```

### 🆘 Help Tab Settings

#### 🔧 Advanced Options

```clarion
Filter Collection Name: Customers      ! Unique identifier for saved filters
☐ Apply filter when changing sort-orders
☐ Allow Free-Form Queries
```

#### 📡 Event Numbers

```clarion
Selection action: 4064             ! Filter by selection
Excluding Selection action: 4065   ! Filter excluding selection  
Filter for action: 4066           ! Advanced filter dialog
Remove query action: 4067         ! Remove all filters
Contains query action: 4068       ! Contains/Less than or equal
Not Contain query action: 4069    ! Not contains/Greater than or equal
Load Query action: 4070           ! Load saved filter
Save Query action: 4071           ! Save current filter
```

#### 🎨 Interface Options

```clarion
☑ Use a "Hot" Popup               ! Context-sensitive menus
Operator Interface: Buttons       ! Buttons vs List selection
  ◯ Buttons  ! Icon-based operator selection
  ◯ List     ! List-box operator selection
```

## 🏗️ Generated Code Structure

### 📋 Class Declaration

```clarion
! Generated for each template instance
TIF2                 CLASS(TInteractiveFilterClass)
BrowseResetSort        PROCEDURE(BYTE Force),DERIVED
BrowseSetFilter        PROCEDURE(STRING Filter,STRING Id),DERIVED  
InitComponents         PROCEDURE(),BYTE,PROC,DERIVED
                     END
```

### ⚡ Initialization Code

```clarion
! In ThisWindow.Init (ABC) or BeforeAccept (Legacy)
TIF2.InitComponents
?TFilterToggle{PROP:Use} = TIF2.isInteractiveFilterOn
TIF2.InteractiveFilterToggleControl = ?TFilterToggle
DISABLE(?TFilterToggle)

! Auto-population or manual field addition
TIF2.UpdateQuery(0)                    ! Auto-populate
! OR
TIF2.AddItem('CUS:Company','Company Name','@s20',e_tint_CaseInsensitive)

TIF2.InitPopup                         ! Setup context menus
TIF2.CaseSensitivity = 0              ! Configure case sensitivity
```

### 🎯 Event Handling

#### 📊 Browse Events

```clarion
! In browse TakeFieldEvent
CASE FIELD()
OF ?Browse:1
  CASE EVENT()
  ELSE
     IF EVENT() = 4064                 ! Filter by selection
        TIF2.FilterOnOperator(e_tint_equals)
     END
     IF EVENT() = 4065                 ! Filter excluding selection
        TIF2.FilterOnOperator(e_tint_NotEquals)
     END
     IF EVENT() = 4067                 ! Remove filter
        TIF2.FilterRemove
     END
     
     ! Context-sensitive filtering
     CASE EVENT()
     OF 4068  ! Contains/Less than or equal
        CASE(TIF2.CurrentType)
        OF 1  ! String
           TIF2.FilterOnOperator(e_tint_Contains)
        OF 2  ! Numeric  
           TIF2.FilterOnOperator(e_tint_LessOrEqualThan)
        OF 3  ! Date
           TIF2.FilterOnOperator(e_tint_LessOrEqualThan)
        END
     OF 4069  ! Not contains/Greater than or equal
        CASE(TIF2.CurrentType)
        OF 1  ! String
           TIF2.FilterOnOperator(e_tint_NotContains)
        OF 2  ! Numeric
           TIF2.FilterOnOperator(e_tint_LargerOrEqualThan)
        OF 3  ! Date
           TIF2.FilterOnOperator(e_tint_LargerOrEqualThan)
        END
     END
     
     ! Save/Load events
     IF EVENT() = TIF2.LoadFilterEvent
        TInteractiveFilter_LoadFilter(TIF2)
        POST(EVENT:NewSelection,?Browse:1)
     END
     IF EVENT() = TIF2.SaveFilterEvent
        TInteractiveFilter_SaveFilter(TIF2)
        SELECT(?Browse:1)
     END
  END
```

#### 🔘 Button Events

```clarion
! Filter button events
CASE ACCEPTED()
OF ?TFilterFor
  ThisWindow.Update()
  x# = TIF2.FilterFor()
  TIF2.FromButton = FALSE

OF ?TFilterToggle
  IF TIF2.isInteractiveFilterOn AND TIF2.GetFilter()
     TIF2.ApplyFilter()
  ELSE
     TIF2.DisableFilter()
  END
  SELECT(TIF2.BrowseControl)
```

### 🎨 Popup Menu Integration

#### 📊 SetAlerts Implementation

```clarion
! In browse SetAlerts method
SELF.Popup.AddItem(TIF2.FilterBySelectionText,'TFilterSelection')
SELF.Popup.AddItemEvent('TFilterSelection',4064,?Browse:1)
SELF.Popup.SetIcon('TFilterSelection','tint_FilterBySelection.ico')

SELF.Popup.AddItem(TIF2.FilterExcludingSelectionText,'TFilterNotSelection')
SELF.Popup.AddItemEvent('TFilterNotSelection',4065,?Browse:1)
SELF.Popup.SetIcon('TFilterNotSelection','tint_FilterExcludingSelection.ico')

! Dynamic context-sensitive items
SELF.Popup.AddItem('','TFilterExtra1')
SELF.Popup.AddItemEvent('TFilterExtra1',4068,?Browse:1)
SELF.Popup.SetIcon('TFilterExtra1','tint_Empty.ico')

SELF.Popup.AddItem('','TFilterExtra2')
SELF.Popup.AddItemEvent('TFilterExtra2',4069,?Browse:1)
SELF.Popup.SetIcon('TFilterExtra2','tint_Empty.ico')

TIF2.ExtraPopupItemsAdded = TRUE

! Filter management
SELF.Popup.AddItemMimic('TFilterFor',?TFilterFor,TIF2.FilterForText)
SELF.Popup.AddItem(TIF2.FilterRemoveText,'TFilterRemove')
SELF.Popup.AddItemEvent('TFilterRemove',4067,?Browse:1)
SELF.Popup.SetIcon('TFilterRemove','tint_FilterRemove.ico')
SELF.Popup.AddItem('-','TFilterSeparator')
```

#### 🔄 Dynamic Menu Updates

```clarion
! In browse TakeNewSelection method
IF ?Browse:1{PROPLIST:MouseDownRow} <> 0 AND ?Browse:1{PROPLIST:MouseDownZone}=LISTZONE:Field
  SELF.UpdateBuffer
  xQueueFieldNo = ?Browse:1{PROPLIST:FieldNo,?Browse:1{PROPLIST:MouseDownField}}
  xField = UPPER(CLIP(WHO(Queue:Browse:1,xQueueFieldNo)))
  
  IF TIF2.SetCurrentInfo(xField,xQueueFieldNo)
     ! Update menu text with current data
     SELF.Popup.SetText('TFilterSelection',TIF2.ReplaceMacro(TIF2.EqualsMacro,TIF2.EqualsBlank))
     SELF.Popup.SetText('TFilterNotSelection',TIF2.ReplaceMacro(TIF2.NotEqualsMacro,TIF2.NotEqualsBlank))
     
     ! Context-sensitive options based on data type
     SELF.Popup.SetText('TFilterExtra1',CHOOSE(TIF2.CurrentType,|
       TIF2.ReplaceMacro(TIF2.ContainsMacro),|          ! String
       TIF2.ReplaceMacro(TIF2.LessOrEqualMacro),|       ! Numeric
       TIF2.ReplaceMacro(TIF2.OnOrBeforeMacro)))        ! Date
  END
END
```

## 🎯 Template Customization

### 📋 Custom Event Numbers

To avoid conflicts with other templates:

```clarion
! Change default event numbers
Selection action: 5000             ! Instead of 4064
Excluding Selection action: 5001   ! Instead of 4065
Filter for action: 5002           ! Instead of 4066
Remove query action: 5003         ! Instead of 4067
```

### 🔧 Custom Procedures

Override default save/load procedures:

```clarion
Save procedure: MyCustomSaveFilter
Load procedure: MyCustomLoadFilter
```

**Custom Procedure Signature:**
```clarion
MyCustomSaveFilter PROCEDURE(* TInteractiveFilterClass pIFObj, <STRING pFilterName>)
MyCustomLoadFilter PROCEDURE(* TInteractiveFilterClass pIFObj)
```

### 🎨 Legacy Clarion Support

For Legacy Clarion applications, the template generates slightly different code:

```clarion
! Legacy popup handling
BuildPopupMenu ROUTINE
  TIF2.TakeNewSelection
  ItemName = 'TFilterSelection'
  IconName = 'tint_FilterBySelection.ico'
  ItemText = TIF2.ReplaceMacro(TIF2.EqualsMacro,TIF2.EqualsBlank)
  DO BuildPopupItem
  
  ! Build popup text string
  PopupText = TPopupText & PopupText
```

## 🔧 Advanced Template Features

### 📊 Sort Order Integration

```clarion
! Automatic filter management across sort orders
IF NewOrder <> TIF2.CurrentSortOrder
   ! Check if filter exists for new sort order
   GET(SELF.Order,NewOrder)
   IF NOT ERRORCODE() AND NOT SELF.Order.Filter &= NULL
      SELF.Order.Filter.ID = '99TABOGA'
      GET(SELF.Order.Filter,-SELF.Order.Filter.ID)
      IF NOT ERRORCODE()
         IF SELF.Order.Filter.Filter = TIF2.GetFilter()
            TIF2.isInteractiveFilterOn = TRUE
         ELSE
            TIF2.isInteractiveFilterOn = FALSE
            SELF.SetFilter('','99TABOGA')
         END
      END
   ELSE
      TIF2.isInteractiveFilterOn = FALSE
   END
   TIF2.CurrentSortOrder = NewOrder
END
```

### 🎛️ InitComponents Customization

```clarion
! Generated InitComponents with all options
TIF2.InitComponents PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.InitComponents()
  
  SELF.myWindow           &= QuickWindow
  SELF.CollectionName = 'Customers'
  SELF.AllowFreeFormQuery = FALSE               ! From template setting
  SELF.BrowseControl    = ?Browse:1
  SELF.BrowseView      &= BRW1::View:Browse
  SELF.BrowseQueueRef  &= Queue:Browse:1
  SELF.AllowQueryPersistance = TRUE             ! From template setting
  SELF.LoadFilterEvent   = 4070                ! From template setting
  SELF.SaveFilterEvent   = 4071                ! From template setting
  SELF.OperatorInterface = 0                   ! From template setting
  
  ! Field overrides (if auto-populate enabled)
  SELF.AddItemOverride('CUS:CustNumber','',TRUE)           ! Skip field
  SELF.AddItemOverride('CUS:Company','Company Name',FALSE) ! Override title
  
  RETURN ReturnValue
```

## 📁 Required Files

### 🎨 Icon Resources

The template automatically includes these icons in your project:

```clarion
#PROJECT('tint_FreeFormQuery.ico')
#PROJECT('tint_SaveQuery.ico')
#PROJECT('tint_LoadQuery.ico')
#PROJECT('tint_FilterBySelection.ico')
#PROJECT('tint_FilterExcludingSelection.ico')
#PROJECT('tint_FilterRemove.ico')
#PROJECT('tint_Empty.ico')
```

### 📊 Data Files

For filter persistence, these files are required:

```clarion
! Filter definitions
_TFilters            FILE,DRIVER('TOPSPEED')
PrimaryKey             KEY(_TF:SavedFilterID),PRIMARY
byFilterCollectionName KEY(_TF:FilterCollectionName,_TF:SavedFilterName),DUP
Record                 RECORD,PRE(_TF)
SavedFilterID            LONG
FilterCollectionName     CSTRING(101)
SavedFilterName          STRING(80)
                       END
                     END

! Individual filter components
_TFilterRows         FILE,DRIVER('TOPSPEED')
PrimaryKey             KEY(_TFRow:SavedFilterID,_TFRow:ID),PRIMARY
Record                 RECORD,PRE(_TFRow)
SavedFilterID            LONG
ID                       SHORT
Filter                   CSTRING(501)
IsManual                 BYTE
FieldName                CSTRING(100)
Picture                  CSTRING(41)
CaseSensitivity          BYTE
Operator                 LONG
ConditionLow             CSTRING(401)
ConditionHigh            CSTRING(401)
JoinOp                   CSTRING(4)
NewGroup                 BYTE
NewGroupJoinOp           CSTRING(4)
                       END
                     END
```

## 📖 Next Steps

- **[API Reference](api-reference.md)** - Complete method documentation
- **[Examples](examples.md)** - Implementation examples
- **[Troubleshooting](troubleshooting.md)** - Common issues and solutions

---

*Master the template system to customize the Interactive Filter for your specific needs!*
