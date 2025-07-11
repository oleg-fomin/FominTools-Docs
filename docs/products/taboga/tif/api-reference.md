# API Reference

Complete reference for the TInteractiveFilterClass and related components based on the actual implementation.

## 🔧 TInteractiveFilterClass

The main class implementing interactive filtering functionality.

### 🚀 Core Methods

#### 🏗️ Lifecycle Methods

```clarion
CONSTRUCT()                                    ! Constructor
DESTRUCT()                                     ! Destructor
Kill()                                         ! Cleanup method
InitComponents()                               ! Initialize components (virtual)
InitPopup()                                    ! Initialize popup menus
```

**InitComponents Implementation:**
```clarion
TIF2.InitComponents PROCEDURE
ReturnValue          BYTE,AUTO
  CODE
  ReturnValue = PARENT.InitComponents()
  
  SELF.myWindow           &= QuickWindow         ! Window reference
  SELF.CollectionName = 'BrowseName'            ! Filter collection identifier
  SELF.BrowseControl    = ?Browse:1              ! Browse control reference
  SELF.BrowseView      &= BrowseView             ! View reference
  SELF.BrowseQueueRef  &= BrowseQueue            ! Queue reference
  
  RETURN ReturnValue
```

#### 📊 Field Management

```clarion
UpdateQuery(BYTE CaseSensitivity)              ! Auto-populate from browse
AddItem(STRING pFieldName, STRING pTitle, <STRING pPicture>, BYTE pCaseSensitivity)
AddItemOverride(STRING pFieldName, STRING pTitleOverride, BYTE pSkipAlways=0)
```

**Usage Examples:**
```clarion
! Auto-populate all browse fields
TIF2.UpdateQuery(0)  ! 0=insensitive, 1=sensitive, 2=auto

! Add individual fields
TIF2.AddItem('CUS:Company','Company Name','@s20',e_tint_CaseInsensitive)
TIF2.AddItem('CUS:State','State','@s2',e_tint_CaseSensitive)

! Override or skip fields (call before UpdateQuery)
TIF2.AddItemOverride('CUS:CustNumber','',TRUE)           ! Skip field
TIF2.AddItemOverride('CUS:Company','Company Name',FALSE) ! Override title
```

### 🎯 Filter Operations

#### ⚡ Quick Filtering

```clarion
FilterBySelection()                            ! Filter equals current value
FilterExcludingSelection()                     ! Filter not equals current value
FilterOnOperator(LONG pOperator)              ! Filter with specific operator
FilterFor()                                    ! Show advanced filter dialog
```

**Operator Constants:**
```clarion
! String operators
e_tint_Equals           EQUATE(1)    ! Exact match
e_tint_NotEquals        EQUATE(2)    ! Not equal
e_tint_BeginsWith       EQUATE(11)   ! Starts with
e_tint_EndsWith         EQUATE(13)   ! Ends with  
e_tint_Contains         EQUATE(15)   ! Contains text
e_tint_NotContains      EQUATE(16)   ! Does not contain
e_tint_Soundex          EQUATE(19)   ! Soundex match

! Numeric operators
e_tint_LessOrEqualThan     EQUATE(3) ! Less than or equal
e_tint_LessThan            EQUATE(4) ! Less than
e_tint_LargerThan          EQUATE(5) ! Greater than
e_tint_LargerOrEqualThan   EQUATE(6) ! Greater than or equal
e_tint_Between             EQUATE(7) ! Between values
e_tint_NotBetween          EQUATE(8) ! Not between values

! Date operators
e_tint_CurrentMonth        EQUATE(31) ! Current month (any year)
e_tint_CurrentDay          EQUATE(32) ! Current day (any year)
e_tint_CurrentYear         EQUATE(33) ! Current year
e_tint_CurrentWeek         EQUATE(34) ! Current week (any year)
e_tint_CurrentWeekDay      EQUATE(35) ! Current weekday (any year)
e_tint_CurrentQuarter      EQUATE(36) ! Current quarter (any year)

! Time operators
e_tint_CurrentHour         EQUATE(91) ! Current hour
e_tint_CurrentMinute       EQUATE(92) ! Current minute
e_tint_AM                  EQUATE(93) ! Morning (12:00 AM - 12:00 PM)
e_tint_PM                  EQUATE(94) ! Afternoon (12:01 PM - 11:59 PM)
```

#### 🔧 Filter Management

```clarion
FilterRemove()                                 ! Remove all filters
FilterRemoveLast()                             ! Remove last filter component
GetFilter()                                    ! Get complete filter string
ApplyFilter()                                  ! Apply current filter
DisableFilter()                                ! Temporarily disable filter
isFilterAvailable()                            ! Check if filters exist
```

### 📋 Advanced Filter Management

```clarion
AppendFilterEntry(*CSTRING pQueryEntry, *CSTRING pFieldName, LONG pOperator,
                  *CSTRING pConditionLow, <*CSTRING pConditionHigh>,
                  *CSTRING pPicture, BYTE pCaseSensitivity, STRING pJoinOp,
                  BYTE pNewGroup, STRING pNewGroupJoinOp)

ClearFilter()                                  ! Clear all filter entries
ClearLastFilterEntry()                        ! Clear last filter entry
SetCurrentInfo(STRING pField, LONG pQueueFieldNo) ! Set current field context
```

### 🔄 Browse Integration

```clarion
BrowseSetFilter(STRING Filter, STRING Id)     ! Set browse filter (virtual)
BrowseResetSort(BYTE Force)                   ! Reset browse sort (virtual)
TakeNewSelection()                             ! Handle selection change (virtual)
```

### 🎨 UI Utilities

```clarion
ReplaceMacro(*CSTRING pMacro, <*CSTRING pBlankMacro>) ! Replace text macros
FreeFormQuery()                                ! Free-form query dialog
```

## ⚙️ Properties

### 🎨 Core Configuration

```clarion
myWindow                &WINDOW                ! Window reference
CollectionName          STRING                 ! Filter collection name
BrowseControl           UNSIGNED               ! Browse control FEQ
BrowseView              &VIEW                  ! Browse view reference
BrowseQueueRef          &QUEUE                 ! Browse queue reference
```

### 🔧 Behavior Settings

```clarion
AllowFreeFormQuery      BYTE                   ! Enable free-form queries
AllowQueryPersistance   BYTE                   ! Enable save/load filters
CaseSensitivity         BYTE                   ! Case handling mode
OperatorInterface       BYTE                   ! 0=buttons, 1=list interface
```

### 📊 State Properties

```clarion
isInteractiveFilterOn   BYTE                   ! Filter active status
CurrentSortOrder        BYTE                   ! Current sort order
CurrentType             BYTE                   ! Current field type
isCurrentDataBlank      BYTE                   ! Current data is blank
FromButton              BYTE                   ! Event from button
ExtraPopupItemsAdded    BYTE                   ! Dynamic items state
```

### 🎛️ Control References

```clarion
InteractiveFilterToggleControl UNSIGNED       ! Toggle control FEQ
InteractiveFilterResultFEQ     UNSIGNED       ! Result display control
```

### 📋 Event Configuration

```clarion
LoadFilterEvent         LONG                   ! Load filter event number
SaveFilterEvent         LONG                   ! Save filter event number
```

### 📝 Text Properties

```clarion
FilterBySelectionText        STRING            ! Menu text
FilterExcludingSelectionText STRING            ! Menu text
FilterByContainsText         STRING            ! Menu text
FilterByNotContainsText      STRING            ! Menu text
FilterForText                STRING            ! Menu text
FilterRemoveText             STRING            ! Menu text
```

### 🔗 Macro Properties

```clarion
EqualsMacro             STRING                 ! Equals text macro
EqualsBlank             STRING                 ! Blank equals text
NotEqualsMacro          STRING                 ! Not equals macro
NotEqualsBlank          STRING                 ! Blank not equals text
ContainsMacro           STRING                 ! Contains macro
NotContainsMacro        STRING                 ! Not contains macro
LessOrEqualMacro        STRING                 ! Less/equal macro
MoreOrEqualMacro        STRING                 ! More/equal macro
OnOrBeforeMacro         STRING                 ! Date before macro
OnOrAfterMacro          STRING                 ! Date after macro
```

## 📊 Queue Structures

### TFilterQueue (FilterRows)

```clarion
ID                      LONG                   ! Filter entry ID
Filter                  STRING                 ! Generated filter expression
IsManual                BYTE                   ! Manual/free-form flag
FieldName               STRING                 ! Field name
Operator                LONG                   ! Filter operator
ConditionLow            STRING                 ! Low condition value
ConditionHigh           STRING                 ! High condition value
Picture                 STRING                 ! Field picture
CaseSensitivity         BYTE                   ! Case sensitivity
JoinOp                  STRING                 ! Join operator (AND/OR)
NewGroup                BYTE                   ! New group flag
NewGroupJoinOp          STRING                 ! New group join operator
```

### TFieldQueue (FieldQ)

```clarion
Name                    STRING                 ! Field name
Title                   STRING                 ! Display title
Picture                 STRING                 ! Field picture
Low                     STRING                 ! Low value
Middle                  STRING                 ! Middle value
High                    STRING                 ! High value
CaseSensitivity         BYTE                   ! Case sensitivity
Justification           BYTE                   ! Text justification
```

### FieldOverrideQueue

```clarion
FieldName               STRING                 ! Field to override
TitleOverride           STRING                 ! Override title
SkipAlways              BYTE                   ! Skip field flag
```

## 🔧 Case Sensitivity Constants

```clarion
e_tint_CaseInsensitive  EQUATE(0)              ! Case insensitive
e_tint_CaseSensitive    EQUATE(1)              ! Case sensitive
e_tint_Configurable     EQUATE(2)              ! User configurable
```

## 📋 Template Events

### Default Event Numbers

```clarion
! Filter events (from template)
4064                                           ! Filter by selection
4065                                           ! Filter excluding selection
4066                                           ! Filter for dialog
4067                                           ! Remove filter
4068                                           ! Contains/Less than or equal
4069                                           ! Not contains/Greater than or equal
4070                                           ! Load saved filter
4071                                           ! Save current filter
```

### Template Symbols

```clarion
! Hex values used in template
0FE0h                                          ! Selection event (4064)
0FE1h                                          ! Excluding selection (4065)
0FE2h                                          ! Filter for event (4066)
0FE3h                                          ! Remove query (4067)
0FE4h                                          ! Contains query (4068)
0FE5h                                          ! Not contains query (4069)
0FE6h                                          ! Load query (4070)
0FE7h                                          ! Save query (4071)
```

## 🎨 Popup Class Integration

The library integrates with ABC PopupClass:

```clarion
Popup                   &PopupClass            ! Main popup reference
PopupFBS                &PopupClass            ! Filter by selection popup
PopupQBF                &PopupClass            ! Query by form popup
```

### Popup Methods Used

```clarion
! Popup management
SELF.Popup.AddItem(TEXT, ID)
SELF.Popup.AddItemEvent(ID, EVENT, CONTROL)
SELF.Popup.SetIcon(ID, ICONFILE)
SELF.Popup.SetText(ID, TEXT)
SELF.Popup.SetItemEnable(ID, ENABLED)
SELF.Popup.DeleteItem(ID)
SELF.Popup.AddItemMimic(ID, CONTROL, TEXT)
```

## 🔄 Legacy Clarion Support

For Legacy Clarion applications:

```clarion
BrowseQueuePrefix       STRING                 ! Queue prefix for legacy
ForceRefresh            BYTE                   ! Force refresh flag
```

### Legacy Popup Handling

```clarion
! Legacy popup variables (template generated)
ItemName                CSTRING(101)           ! Current item name
IconName                CSTRING(255)           ! Current icon name
ItemText                CSTRING(101)           ! Current item text
TPopupText              CSTRING(1000)          ! Complete popup text
```

## 📁 File Integration

For filter persistence:

```clarion
! Persistence files (created by save/load procedures)
_TFilters               FILE                   ! Filter definitions
_TFilterRows            FILE                   ! Filter components
```

---

*This API reference provides complete coverage of the TInteractiveFilterClass and its integration with Clarion browse procedures.*
