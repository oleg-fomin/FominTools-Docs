# Getting Started

## 📦 Installation

### 🔧 Library Files

Ensure the following files are in your Clarion installation:

#### 📁 LibSrc Directory
```
libsrc\
├── TInteractiveFilter.INC    # Class definitions and includes
├── TInteractiveFilter.CLW    # Class implementation
└── TInteractiveFilter.TRN    # Translation strings
```

#### 📁 Template Directory  
```
template\
└── TInteractiveFilter.TPL   # Template definitions
```

#### 🎨 Icon Files
```
images\
├── tint_FilterFor.ico
├── tint_FilterToggle.ico
├── tint_FilterBySelection.ico
├── tint_FilterExcludingSelection.ico
├── tint_FilterRemove.ico
├── tint_Empty.ico
├── tint_FreeFormQuery.ico
├── tint_LoadQuery.ico
└── tint_SaveQuery.ico
```

### ⚙️ Template Registration

1. **Open Clarion IDE**
2. **Template Registry** → **Register Template**
3. **Browse** to `TInteractiveFilter.TPL`
4. **Confirm** registration

## 🚀 Basic Setup

### 1️⃣ Enable Extension

First, enable the Interactive Filter extension in your application:

1. **Application Properties** → **Extensions**
2. **Add Extension** → **[TABOGA] Enables "InteractiveFilter" functionality**
3. **Configure** multi-DLL settings if applicable

```clarion
! Extension automatically adds global includes:
INCLUDE('TInteractiveFilter.INC'),ONCE
```

### 2️⃣ Add to Browse Window

Apply the control template to your browse procedure:

1. **Select Browse Procedure**
2. **Window Designer** → **Controls**
3. **Add Control** → **[TABOGA] Interactive Filter for browse**
4. **Position** the filter controls on your window

#### 🎨 Default Controls Added
```clarion
BUTTON,AT(,,14,14),USE(?TFilterFor),ICON('tint_FilterFor.ico'),FLAT
CHECK,AT(14,0,14,14),USE(?TFilterToggle),ICON('tint_FilterToggle.ico'),FLAT
```

### 3️⃣ Configure Template

#### 📊 Query Tab
- ✅ **Filter by selection** - Right-click equals filtering
- ✅ **Filter excluding selection** - Right-click not-equals filtering  
- ✅ **Filter for** - Advanced filter dialog
- ✅ **Remove Filter** - Clear all filters
- ✅ **Contains/Does Not Contain Pair** - Context-sensitive options

#### 🔧 Case Sensitivity
- **Force Case Sensitive** - Exact case matching
- **Force Case Insensitive** - Ignore case (default)
- **Manual** - Driver-dependent default

#### 💾 Filter Persistence
```clarion
☑ Allow saving and loading filters
  Save procedure: TInteractiveFilter_SaveFilter
  Load procedure: TInteractiveFilter_LoadFilter
```

### 4️⃣ Field Configuration

#### 🤖 Auto-Population (Recommended)
```
☑ Auto Populate from BrowseBox
```
- Automatically includes all browse columns
- Override field titles as needed
- Skip fields selectively

#### 📝 Manual Configuration
```clarion
! Add fields manually
QBEField: CUS:Company
QBETitle: Company Name
QBEPicture: @s20
QBENoCase: Yes
```

## 🔄 Generated Code

The template automatically generates the necessary integration code:

### 📋 Class Declaration
```clarion
TIF2                 CLASS(TInteractiveFilterClass)
BrowseResetSort        PROCEDURE(BYTE Force),DERIVED
BrowseSetFilter        PROCEDURE(STRING Filter,STRING Id),DERIVED
InitComponents         PROCEDURE(),BYTE,PROC,DERIVED
                     END
```

### ⚡ Initialization
```clarion
! Automatic initialization in ThisWindow.Init
TIF2.InitComponents
?TFilterToggle{PROP:Use} = TIF2.isInteractiveFilterOn
TIF2.InteractiveFilterToggleControl = ?TFilterToggle
DISABLE(?TFilterToggle)

TIF2.UpdateQuery(0)        ! Auto-populate fields
TIF2.InitPopup            ! Setup context menus
TIF2.CaseSensitivity = 0  ! Configure case handling
```

### 🎯 Event Handling
```clarion
! Filter button events
OF ?TFilterFor
  x# = TIF2.FilterFor()
  TIF2.FromButton = FALSE

OF ?TFilterToggle  
  IF TIF2.isInteractiveFilterOn AND TIF2.GetFilter()
     TIF2.ApplyFilter()
  ELSE
     TIF2.DisableFilter()
  END
```

## 🎨 UI Integration

### 📊 Browse Popup Menu
The template automatically adds context menu items:

- **Filter by "Value"** - Quick equal filter
- **Filter excluding "Value"** - Quick not-equal filter  
- **Contains "Value"** - String contains filter
- **On or before "Value"** - Date/numeric comparison
- **Filter for...** - Advanced dialog
- **Remove filter** - Clear all filters

### 🔧 Visual Feedback
- **Toggle Button** - Shows active filter state
- **Filter Display** - Optional current filter text
- **Context-Sensitive Menus** - Adapt to data type

## ✅ Verification

After setup, verify the installation:

1. **Compile** your application successfully
2. **Run** and open a browse window
3. **Right-click** on any column - see filter options
4. **Click** the filter button - opens filter dialog
5. **Apply** a filter - see toggle button activate

## 🚀 Next Steps

- **[Tutorial](tutorial.md)** - Detailed implementation walkthrough
- **[Template Guide](template-guide.md)** - Advanced configuration options
- **[Examples](examples.md)** - Real-world implementation examples

---

*You're now ready to add powerful filtering capabilities to your browse windows!*
