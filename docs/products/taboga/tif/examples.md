# Examples

Real-world implementation examples of the Taboga Interactive Filter Library.

## 🎯 Basic Customer Browse

### 📊 Simple Implementation

```clarion
! Basic customer browse with filtering
Customers PROCEDURE 

BRW1::View:Browse    VIEW(Customers)
                       PROJECT(CUS:Company)
                       PROJECT(CUS:FirstName)
                       PROJECT(CUS:LastName)
                       PROJECT(CUS:City)
                       PROJECT(CUS:State)
                       PROJECT(CUS:ZipCode)
                     END

Queue:Browse:1       QUEUE
CUS:Company            LIKE(CUS:Company)
CUS:FirstName          LIKE(CUS:FirstName)
CUS:LastName           LIKE(CUS:LastName)
CUS:City               LIKE(CUS:City)
CUS:State              LIKE(CUS:State)
CUS:ZipCode            LIKE(CUS:ZipCode)
CUS:CustNumber         LIKE(CUS:CustNumber)
Mark                   BYTE
ViewPosition           STRING(1024)
                     END

! Interactive Filter class
TIF2                 CLASS(TInteractiveFilterClass)
BrowseResetSort        PROCEDURE(BYTE Force),DERIVED
BrowseSetFilter        PROCEDURE(STRING Filter,STRING Id),DERIVED
InitComponents         PROCEDURE(),BYTE,PROC,DERIVED
                     END

QuickWindow          WINDOW('Browse Customers'),AT(,,447,197),RESIZE,CENTER
  LIST,AT(8,30,434,124),USE(?Browse:1),FROM(Queue:Browse:1)
  BUTTON,AT(5,158,14,14),USE(?TFilterFor),ICON('tint_FilterFor.ico'),FLAT
  CHECK,AT(19,158,14,14),USE(?TFilterToggle),ICON('tint_FilterToggle.ico'),FLAT
  BUTTON('Close'),AT(399,180,49,14),USE(?Close)
END

  CODE
  GlobalResponse = ThisWindow.Run()

ThisWindow.Init PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.Init()
  IF ReturnValue THEN RETURN ReturnValue.
  
  ! Initialize Interactive Filter
  TIF2.InitComponents
  ?TFilterToggle{PROP:Use} = TIF2.isInteractiveFilterOn
  TIF2.InteractiveFilterToggleControl = ?TFilterToggle
  DISABLE(?TFilterToggle)
  
  TIF2.UpdateQuery(0)        ! Auto-populate from browse
  TIF2.InitPopup            ! Setup context menus
  TIF2.CaseSensitivity = 0  ! Case insensitive
  
  RETURN ReturnValue

TIF2.InitComponents PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.InitComponents()
  
  SELF.myWindow           &= QuickWindow
  SELF.CollectionName = 'Customers'
  SELF.AllowFreeFormQuery = FALSE
  SELF.BrowseControl    = ?Browse:1
  SELF.BrowseView      &= BRW1::View:Browse
  SELF.BrowseQueueRef  &= Queue:Browse:1
  SELF.AllowQueryPersistance = TRUE
  SELF.LoadFilterEvent   = 4070
  SELF.SaveFilterEvent   = 4071
  SELF.OperatorInterface = 0
  
  RETURN ReturnValue
```

## 🎨 Advanced Product Browse

### 📋 List Interface with Filter Display

```clarion
Products PROCEDURE 

! Enhanced configuration with visual feedback
TIF11                CLASS(TInteractiveFilterClass)
BrowseResetSort        PROCEDURE(BYTE Force),DERIVED
BrowseSetFilter        PROCEDURE(STRING Filter,STRING Id),DERIVED
InitComponents         PROCEDURE(),BYTE,PROC,DERIVED
                     END

QuickWindow          WINDOW('Browse Products'),AT(,,493,262),CENTER
  PROMPT('Current filter will be displayed below:'),AT(6,2,478,10)
  LIST,AT(8,65,475,115),USE(?Browse:1),FROM(Queue:Browse:1)
  
  ! Filter controls
  BUTTON,AT(4,204,14,14),USE(?TFilterFor),ICON('tint_FilterFor.ico'),FLAT
  CHECK,AT(18,204,14,14),USE(?TFilterToggle),ICON('tint_FilterToggle.ico'),FLAT
  
  ! Filter display
  PROMPT('Filter'),AT(5,223,349,36),USE(?TFilter)
  
  BUTTON('Close'),AT(441,206,49,14),USE(?Close)
END

TIF11.InitComponents PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.InitComponents()
  
  SELF.myWindow           &= QuickWindow
  SELF.CollectionName = 'Products'
  SELF.AllowFreeFormQuery = FALSE
  SELF.BrowseControl    = ?Browse:1
  SELF.BrowseView      &= BRW1::View:Browse
  SELF.BrowseQueueRef  &= Queue:Browse:1
  
  ! Use list interface for operators
  SELF.OperatorInterface = 1
  
  ! Display current filter
  SELF.InteractiveFilterResultFEQ = ?TFilter
  
  SELF.AllowQueryPersistance = TRUE
  SELF.LoadFilterEvent   = 4070
  SELF.SaveFilterEvent   = 4071
  
  RETURN ReturnValue
```

## 📅 Date/Numeric Filtering

### 🕐 Inventory History Browse

```clarion
InvHist PROCEDURE 

! Specialized for date and numeric data
TIF7                 CLASS(TInteractiveFilterClass)
BrowseResetSort        PROCEDURE(BYTE Force),DERIVED
BrowseSetFilter        PROCEDURE(STRING Filter,STRING Id),DERIVED
InitComponents         PROCEDURE(),BYTE,PROC,DERIVED
                     END

BRW1::View:Browse    VIEW(InvHist)
                       PROJECT(INV:Date)
                       PROJECT(INV:TransType)
                       PROJECT(INV:ProductNumber)
                       PROJECT(INV:Quantity)
                       PROJECT(INV:VendorNumber)
                       PROJECT(INV:Cost)
                       PROJECT(INV:Notes)
                       JOIN(VEN:KeyVendorNumber,INV:VendorNumber)
                         PROJECT(VEN:VendorName)
                       END
                       JOIN(PRO:KeyProductNumber,INV:ProductNumber)
                         PROJECT(PRO:Description)
                       END
                     END

TIF7.InitComponents PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.InitComponents()
  
  SELF.myWindow           &= QuickWindow
  SELF.CollectionName = 'InvHist'
  SELF.AllowFreeFormQuery = FALSE
  SELF.BrowseControl    = ?Browse:1
  SELF.BrowseView      &= BRW1::View:Browse
  SELF.BrowseQueueRef  &= Queue:Browse:1
  SELF.AllowQueryPersistance = TRUE
  SELF.LoadFilterEvent   = 4070
  SELF.SaveFilterEvent   = 4071
  SELF.OperatorInterface = 0
  
  RETURN ReturnValue

! Event handling for date/numeric context
ThisWindow.TakeFieldEvent PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.TakeFieldEvent()
  
  CASE FIELD()
  OF ?Browse:1
    CASE EVENT()
    ELSE
       ! Enhanced context-sensitive filtering
       CASE EVENT()
       OF 4068  ! Contains/Less than or equal
          CASE(TIF7.CurrentType)
          OF 1     ! String
             TIF7.FilterOnOperator(e_tint_Contains)
          OF 2     ! Numeric  
             TIF7.FilterOnOperator(e_tint_LessOrEqualThan)
          OF 3     ! Date
             TIF7.FilterOnOperator(e_tint_LessOrEqualThan)
          OF 4     ! Time
             TIF7.FilterOnOperator(e_tint_LessOrEqualThan)
          END
          
       OF 4069  ! Not contains/Greater than or equal
          CASE(TIF7.CurrentType)
          OF 1     ! String
             TIF7.FilterOnOperator(e_tint_NotContains)
          OF 2     ! Numeric
             TIF7.FilterOnOperator(e_tint_LargerOrEqualThan)
          OF 3     ! Date
             TIF7.FilterOnOperator(e_tint_LargerOrEqualThan)
          OF 4     ! Time
             TIF7.FilterOnOperator(e_tint_LargerOrEqualThan)
          END
       END
    END
  END
  
  RETURN ReturnValue
```

## 💾 Custom Save/Load Implementation

### 🔧 Enhanced Filter Persistence

```clarion
! Custom save procedure with additional validation
MyCustomSaveFilter PROCEDURE(* TInteractiveFilterClass pIFObj, <STRING pFilterName>)
xSavedFilterName     LIKE(_TF:SavedFilterName)
xFilterCount         LONG
Window WINDOW('Save Filter'),AT(,,300,80),GRAY,CENTER,MODAL
  PROMPT('Filter Name:'),AT(10,10)
  ENTRY(@s80),AT(10,25,200,12),USE(xSavedFilterName)
  STRING(@n3),AT(220,25),USE(xFilterCount)
  BUTTON('&Save'),AT(155,50,45,14),USE(?Save),DEFAULT
  BUTTON('&Cancel'),AT(205,50,45,14),USE(?Cancel)
END

  CODE
  ! Get current filter count
  xFilterCount = RECORDS(pIFObj.FilterRows)
  
  ! Validate filter exists
  IF NOT xFilterCount
     MESSAGE('No filter criteria to save!','Save Filter',ICON:Exclamation)
     RETURN TRUE
  END
  
  ! Get filter name from user
  IF NOT OMITTED(2)
     xSavedFilterName = CLIP(pFilterName)
  ELSE
     OPEN(Window)
     ACCEPT
        CASE FIELD()
        OF ?Save
           IF NOT xSavedFilterName
              MESSAGE('Please enter a filter name!','Save Filter',ICON:Exclamation)
              SELECT(?xSavedFilterName)
              CYCLE
           END
           POST(EVENT:CloseWindow)
        OF ?Cancel
           xSavedFilterName = ''
           POST(EVENT:CloseWindow)
        END
     END
     CLOSE(Window)
  END
  
  IF NOT xSavedFilterName
     RETURN TRUE  ! User cancelled
  END
  
  ! Save with timestamp
  CLEAR(_TFilters)
  _TF:FilterCollectionName = pIFObj.CollectionName
  _TF:SavedFilterName = xSavedFilterName & ' [' & FORMAT(TODAY(),@d6) & ']'
  
  ! Continue with standard save logic...
  ! (Implementation details as in tutorial)
  
  MESSAGE('Filter "' & xSavedFilterName & '" saved successfully!','Save Filter',ICON:Asterisk)
  RETURN FALSE

! Custom load with preview
MyCustomLoadFilter PROCEDURE(* TInteractiveFilterClass pIFObj)
xPreviewFilter       STRING(1000)
xAppendMode          STRING(10)

! Enhanced load dialog with preview
LoadWindow WINDOW('Load Saved Filter'),AT(,,400,250),GRAY,CENTER,MODAL
  LIST,AT(10,20,200,150),USE(?FilterList),FROM(FilterQueue)
  PROMPT('Preview:'),AT(220,20)
  TEXT,AT(220,35,170,100),USE(xPreviewFilter),READONLY
  OPTION('Load Mode'),AT(220,140,170,40),USE(xAppendMode),BOXED
    RADIO('Replace Current'),VALUE('REPLACE')
    RADIO('Add with AND'),VALUE('AND')
    RADIO('Add with OR'),VALUE('OR')
  END
  BUTTON('&Load'),AT(270,200,50,14),USE(?Load)
  BUTTON('&Cancel'),AT(330,200,50,14),USE(?Cancel)
END

  CODE
  ! Build filter list with preview
  ! Show enhanced load dialog
  ! Apply selected filter with chosen mode
  
  RETURN
```

## 🎯 Field Override Examples

### 📝 Selective Field Configuration

```clarion
! Manual field configuration with overrides
TIF2.InitComponents PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.InitComponents()
  
  ! Standard configuration
  SELF.myWindow           &= QuickWindow
  SELF.CollectionName = 'CustomersAdvanced'
  SELF.BrowseControl    = ?Browse:1
  SELF.BrowseView      &= BRW1::View:Browse
  SELF.BrowseQueueRef  &= Queue:Browse:1
  
  ! Field overrides (applied before UpdateQuery)
  SELF.AddItemOverride('CUS:CustNumber','',TRUE)           ! Skip internal ID
  SELF.AddItemOverride('CUS:Company','Company Name',FALSE) ! Override title
  SELF.AddItemOverride('CUS:State','State/Province',FALSE) ! Localized title
  
  ! Auto-populate with overrides applied
  SELF.UpdateQuery(0)
  
  ! Manual additions for special fields
  SELF.AddItem('CUS:LastModified','Last Updated','@d6',e_tint_CaseSensitive)
  SELF.AddItem('CUS:CreditLimit','Credit Limit','@n$10.2',e_tint_CaseSensitive)
  
  RETURN ReturnValue
```

## 🔧 Legacy Clarion Implementation

### 🏛️ Non-ABC Application

```clarion
! Legacy Clarion implementation
Customers PROCEDURE 

! Legacy popup handling variables
ItemName             CSTRING(101)
IconName             CSTRING(255)
ItemText             CSTRING(101)
TPopupText           CSTRING(1000)
PopupText            CSTRING(1000)

TIF2                 CLASS(TInteractiveFilterClass)
BrowseResetSort        PROCEDURE(BYTE Force),DERIVED
BrowseSetFilter        PROCEDURE(STRING Filter,STRING Id),DERIVED
InitComponents         PROCEDURE(),BYTE,PROC,DERIVED
TakeNewSelection       PROCEDURE(),DERIVED
                     END

Window WINDOW('Browse Customers'),AT(,,400,200)
  LIST,AT(10,30,380,130),USE(?Browse:1),FROM(BrowseQueue)
  BUTTON,AT(10,170,14,14),USE(?TFilterFor),ICON('tint_FilterFor.ico'),FLAT
  CHECK,AT(30,170,14,14),USE(?TFilterToggle),ICON('tint_FilterToggle.ico'),FLAT
  BUTTON('Close'),AT(350,170,40,14),USE(?Close)
END

  CODE
  OPEN(Window)
  
  ! Initialize filter
  TIF2.InitComponents
  ?TFilterToggle{PROP:Use} = TIF2.isInteractiveFilterOn
  TIF2.InteractiveFilterToggleControl = ?TFilterToggle
  DISABLE(?TFilterToggle)
  
  TIF2.UpdateQuery(0)
  TIF2.InitPopup
  TIF2.CaseSensitivity = 0
  
  ! Legacy accept loop
  ACCEPT
    CASE FIELD()
    OF ?Browse:1
      CASE EVENT()
      OF EVENT:NewSelection
        DO TakeNewSelection
      ELSE
        ! Filter event handling
        CASE EVENT()
        OF 4064
          TIF2.FilterOnOperator(e_tint_equals)
        OF 4065
          TIF2.FilterOnOperator(e_tint_NotEquals)
        OF 4067
          TIF2.FilterRemove
        END
      END
      
    OF ?TFilterFor
      IF EVENT() = EVENT:Accepted
         x# = TIF2.FilterFor()
      END
      
    OF ?TFilterToggle
      IF EVENT() = EVENT:Accepted
         IF TIF2.isInteractiveFilterOn AND TIF2.GetFilter()
            TIF2.ApplyFilter()
         ELSE
            TIF2.DisableFilter()
         END
      END
      
    OF ?Close
      IF EVENT() = EVENT:Accepted
         POST(EVENT:CloseWindow)
      END
    END
  END
  
  CLOSE(Window)

TIF2.InitComponents PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.InitComponents()
  
  SELF.myWindow           &= Window
  SELF.CollectionName = 'CustomersLegacy'
  SELF.BrowseControl    = ?Browse:1
  SELF.BrowseQueuePrefix = 'BRW1:'
  SELF.AllowQueryPersistance = FALSE  ! Simplified for legacy
  SELF.OperatorInterface = 0
  
  RETURN ReturnValue

TakeNewSelection ROUTINE
  ! Legacy popup menu building
  TIF2.TakeNewSelection
  PopupText = TPopupText
  ! Handle popup selection...
```

## 🎨 Multi-DLL Configuration

### 🔧 DLL with Exported Classes

```clarion
! In main DLL application
APPLICATION('FilterLibrary'),DLL

! Extension configuration
[T] Enables "InteractiveFilter" functionality
  ☑ Export TInteractiveFilterClass
  ☑ Export ABC PopupClass
  
! Compilation symbols generated:
! _TIntFilterLinkMode_=>1;_TIntFilterDllMode_=>0

! In client application
APPLICATION('ClientApp'),EXE

! Extension configuration  
[T] Enables "InteractiveFilter" functionality
  ☑ Generate TInteractiveFilterClass as EXTERNAL
    External Source: Dynamic Link Library (DLL)
    
! Compilation symbols generated:
! _TIntFilterLinkMode_=>0;_TIntFilterDllMode_=>1
```

## 📊 Performance Optimization

### ⚡ High-Performance Configuration

```clarion
! Optimized for large datasets
TIF2.InitComponents PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.InitComponents()
  
  ! Standard configuration
  SELF.myWindow           &= QuickWindow
  SELF.CollectionName = 'LargeDataset'
  SELF.BrowseControl    = ?Browse:1
  SELF.BrowseView      &= BRW1::View:Browse
  SELF.BrowseQueueRef  &= Queue:Browse:1
  
  ! Performance optimizations
  SELF.CaseSensitivity = 1                     ! Case sensitive for SQL
  SELF.AllowFreeFormQuery = FALSE              ! Disable complex queries
  SELF.OperatorInterface = 0                   ! Faster button interface
  
  ! Only add indexed fields
  SELF.AddItem('CUS:State','State','@s2',e_tint_CaseSensitive)      ! Indexed
  SELF.AddItem('CUS:ZipCode','Zip','@s10',e_tint_CaseSensitive)     ! Indexed
  SELF.AddItem('CUS:Company','Company','@s20',e_tint_CaseSensitive) ! Indexed
  
  ! Skip non-indexed fields to prevent slow queries
  
  RETURN ReturnValue
```

## 🎯 Integration Examples

### 🔗 With Query-by-Example

```clarion
! Combining with existing QBE functionality
BRW1.SetAlerts PROCEDURE
  CODE
  ! Add QBE integration
  IF QBEActive
     SELF.Popup.AddItem('Switch to QBE','SwitchQBE')
  END
  
  ! Standard filter popup items
  SELF.Popup.AddItem(TIF2.FilterBySelectionText,'TFilterSelection')
  SELF.Popup.AddItemEvent('TFilterSelection',4064,?Browse:1)
  SELF.Popup.SetIcon('TFilterSelection','tint_FilterBySelection.ico')
  
  PARENT.SetAlerts
```

### 🎨 Custom Event Handling

```clarion
! Custom event numbers to avoid conflicts
TIF2.InitComponents PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.InitComponents()
  
  ! Custom event numbers
  SELF.LoadFilterEvent   = 5000  ! Instead of 4070
  SELF.SaveFilterEvent   = 5001  ! Instead of 4071
  
  ! Rest of configuration...
  RETURN ReturnValue

! Corresponding event handling
ThisWindow.TakeFieldEvent PROCEDURE
  CODE
  ! Handle custom events
  CASE EVENT()
  OF 5000  ! Custom load event
     MyCustomLoadFilter(TIF2)
  OF 5001  ! Custom save event
     MyCustomSaveFilter(TIF2)
  END
  
  RETURN PARENT.TakeFieldEvent()
```

---

*These examples demonstrate the flexibility and power of the Taboga Interactive Filter Library across different scenarios and configurations.*
