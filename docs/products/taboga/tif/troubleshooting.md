# Troubleshooting

Common issues and solutions for the Taboga Interactive Filter Library.

## 🚨 Installation Issues

### ❌ Template Not Found

**Problem:** "Template 'TInteractiveFilter' not found" error when registering.

**Solutions:**
1. **Verify File Location**
   ```
   ✅ Correct: C:\Clarion11\Template\TInteractiveFilter.TPL
   ❌ Wrong:   C:\Clarion11\LibSrc\TInteractiveFilter.TPL
   ```

2. **Register Template Manually**
   - Open Clarion IDE
   - **Setup** → **Template Registry**
   - **Register** → Browse to TPL file
   - **Confirm** registration successful

3. **Check File Permissions**
   - Ensure read access to template directory
   - Run Clarion as Administrator if needed

### ❌ Extension Restriction Error

**Problem:** "Control template requires TEnableInteractiveFilter extension" error.

**Solutions:**
1. **Add Extension First**
   ```
   Application Properties → Extensions
   Add: [T] Enables "InteractiveFilter" functionality
   ```

2. **Extension Order**
   - Extension must be added before control template
   - If added after, remove and re-add control template

3. **Verify Extension Active**
   ```clarion
   ! Should see in global includes:
   INCLUDE('TInteractiveFilter.INC'),ONCE
   ```

## 🔧 Compilation Errors

### ❌ Missing Include Files

**Problem:** "File not found: TInteractiveFilter.INC" compilation error.

**Solutions:**
1. **Check LibSrc Directory**
   ```
   ✅ Required files:
   C:\Clarion11\LibSrc\TInteractiveFilter.INC
   C:\Clarion11\LibSrc\TInteractiveFilter.CLW
   C:\Clarion11\LibSrc\TInteractiveFilter_EN.TRN
   ```

2. **Verify Include Path**
   ```clarion
   ! In generated code, should see:
   INCLUDE('TInteractiveFilter.INC'),ONCE
   ```

3. **Manual Include**
   ```clarion
   ! Add to global map if needed:
   MAP
     INCLUDE('TInteractiveFilter.INC'),ONCE
   END
   ```

### ❌ Class Not Found Errors

**Problem:** "TInteractiveFilterClass not found" compilation error.

**Solutions:**
1. **Multi-DLL Configuration Issue**
   ```clarion
   ! Check compilation symbols:
   _TIntFilterLinkMode_=>1;_TIntFilterDllMode_=>0  ! Standard
   _TIntFilterLinkMode_=>0;_TIntFilterDllMode_=>1  ! External DLL
   ```

2. **ABC Classes Not Enabled**
   ```clarion
   ! For Legacy Clarion, ensure ABC is available:
   _ABCLinkMode_=>1;_ABCDllMode_=>0
   ```

3. **Export Configuration**
   ```clarion
   ! In DLL project, ensure exports:
   TInteractiveFilterClass @?
   PopupClass @?  ! If other apps don't use ABC
   ```

### ❌ Icon File Errors

**Problem:** "Icon file not found" warnings or missing icons.

**Solutions:**
1. **Add Icons to Project**
   ```clarion
   #PROJECT('tint_FilterFor.ico')
   #PROJECT('tint_FilterToggle.ico')
   #PROJECT('tint_FilterBySelection.ico')
   #PROJECT('tint_FilterExcludingSelection.ico')
   #PROJECT('tint_FilterRemove.ico')
   #PROJECT('tint_Empty.ico')
   ```

2. **Icon File Location**
   ```
   ✅ Project directory or Clarion\Images
   ❌ Don't put in LibSrc or Template directories
   ```

3. **Alternative Icon Specification**
   ```clarion
   ! Use full path if needed:
   ICON('C:\MyProject\Icons\tint_FilterFor.ico')
   ```

## 🎯 Runtime Issues

### ❌ Filter Not Working

**Problem:** Right-click menu appears but filtering doesn't work.

**Solutions:**
1. **Check Event Handling**
   ```clarion
   ! Verify events are handled in TakeFieldEvent:
   CASE FIELD()
   OF ?Browse:1
     IF EVENT() = 4064  ! Filter by selection
        TIF2.FilterOnOperator(e_tint_equals)
     END
   ```

2. **Initialization Missing**
   ```clarion
   ! Ensure proper initialization:
   TIF2.InitComponents
   TIF2.UpdateQuery(0)
   TIF2.InitPopup
   ```

3. **Browse Integration**
   ```clarion
   ! Check SetAlerts method has popup setup:
   SELF.Popup.AddItem(TIF2.FilterBySelectionText,'TFilterSelection')
   ```

### ❌ Toggle Button Not Working

**Problem:** Filter toggle button doesn't enable/disable filters.

**Solutions:**
1. **Control Reference**
   ```clarion
   ! Verify toggle control is set:
   TIF2.InteractiveFilterToggleControl = ?TFilterToggle
   ```

2. **Property Binding**
   ```clarion
   ! Ensure property is bound:
   ?TFilterToggle{PROP:Use} = TIF2.isInteractiveFilterOn
   ```

3. **Event Handling**
   ```clarion
   ! Check TakeAccepted has toggle handling:
   OF ?TFilterToggle
     IF TIF2.isInteractiveFilterOn AND TIF2.GetFilter()
        TIF2.ApplyFilter()
     ELSE
        TIF2.DisableFilter()
     END
   ```

### ❌ Popup Menu Disabled

**Problem:** All popup menu items are grayed out/disabled.

**Solutions:**
1. **Field Detection**
   ```clarion
   ! Check SetCurrentInfo is called:
   IF TIF2.SetCurrentInfo(xField,xQueueFieldNo)
      ! Enable menu items
   ELSE
      ! Disable menu items
   END
   ```

2. **Mouse Position**
   ```clarion
   ! Verify mouse position detection:
   IF ?Browse:1{PROPLIST:MouseDownRow} <> 0 AND 
      ?Browse:1{PROPLIST:MouseDownZone}=LISTZONE:Field
      ! Process field selection
   END
   ```

3. **TakeNewSelection**
   ```clarion
   ! Ensure TakeNewSelection is called:
   BRW1.TakeNewSelection PROCEDURE
     ! Filter-specific code here
     PARENT.TakeNewSelection
   ```

## 💾 Persistence Issues

### ❌ Save/Load Not Working

**Problem:** Filter save/load functionality not available.

**Solutions:**
1. **Enable Persistence**
   ```clarion
   ! In template configuration:
   ☑ Allow saving and loading filters
   Save procedure: TInteractiveFilter_SaveFilter
   Load procedure: TInteractiveFilter_LoadFilter
   ```

2. **Procedure Implementation**
   ```clarion
   ! Verify procedures exist:
   TInteractiveFilter_SaveFilter PROCEDURE(* TInteractiveFilterClass pIFObj,<STRING pFilterName>)
   TInteractiveFilter_LoadFilter PROCEDURE(* TInteractiveFilterClass pIFObj)
   ```

3. **File Access**
   ```clarion
   ! Check file definitions exist:
   _TFilters            FILE,DRIVER('TOPSPEED')
   _TFilterRows         FILE,DRIVER('TOPSPEED')
   ```

### ❌ Saved Filters Not Loading

**Problem:** Saved filters appear in list but don't load correctly.

**Solutions:**
1. **Data File Structure**
   ```clarion
   ! Verify correct field structure:
   _TFRow:SavedFilterID    LONG
   _TFRow:FieldName        CSTRING(100)
   _TFRow:Operator         LONG
   _TFRow:ConditionLow     CSTRING(401)
   ! ...etc
   ```

2. **Collection Name Match**
   ```clarion
   ! Ensure collection names match:
   SELF.CollectionName = 'CustomerBrowse'  ! Must be consistent
   ```

3. **Filter Loading Logic**
   ```clarion
   ! Check filter reconstruction:
   pIFObj.FilterRows.Filter = _TFRow:Filter
   pIFObj.FilterRows.FieldName = _TFRow:FieldName
   ADD(pIFObj.FilterRows)
   pIFObj.ApplyFilter  ! Don't forget to apply
   ```

## 🎨 UI Problems

### ❌ Context Menu Text Wrong

**Problem:** Menu items show wrong text or placeholders.

**Solutions:**
1. **Macro Replacement**
   ```clarion
   ! Check macro replacement:
   SELF.Popup.SetText('TFilterSelection',
     TIF2.ReplaceMacro(TIF2.EqualsMacro,TIF2.EqualsBlank))
   ```

2. **Current Data Detection**
   ```clarion
   ! Verify current field/data is set:
   IF TIF2.SetCurrentInfo(xField,xQueueFieldNo)
      ! Macros will work correctly
   END
   ```

3. **Translation Files**
   ```clarion
   ! Check translation file is loaded:
   TInteractiveFilter_EN.TRN  ! For English
   ```

### ❌ Filter Display Control

**Problem:** Filter display control doesn't show current filter.

**Solutions:**
1. **Control Assignment**
   ```clarion
   ! Set the result control:
   TIF2.InteractiveFilterResultFEQ = ?FilterDisplay
   ```

2. **Control Type**
   ```clarion
   ! Use appropriate control type:
   STRING(@s1000),USE(?FilterDisplay)  ! For filter text
   ```

3. **Display Update**
   ```clarion
   ! Verify display is updated after filter:
   TIF2.ApplyFilter()  ! Should update display automatically
   ```

## 🔍 Performance Issues

### ❌ Slow Filtering

**Problem:** Filter application takes too long.

**Solutions:**
1. **Field Indexing**
   ```clarion
   ! Ensure filtered fields are indexed:
   CUS:StateKey         KEY(CUS:State),DUP,NOCASE,OPT
   ```

2. **Case Sensitivity**
   ```clarion
   ! For SQL drivers, use case sensitive:
   TIF2.CaseSensitivity = 1  ! Better SQL generation
   ```

3. **Filter Complexity**
   ```clarion
   ! Avoid too many nested groups:
   ! Simple: (Field1='A' AND Field2='B')
   ! Complex: ((Field1='A' OR Field1='B') AND (Field2='C' OR Field2='D'))
   ```

### ❌ Memory Issues

**Problem:** Application runs out of memory with many filters.

**Solutions:**
1. **Clear Old Filters**
   ```clarion
   ! Clear filters when not needed:
   TIF2.ClearFilter()
   ```

2. **Queue Management**
   ```clarion
   ! Monitor queue growth:
   FREE(TIF2.FilterRows)  ! If needed
   ```

3. **Browse Queue Size**
   ```clarion
   ! Limit browse queue if very large datasets:
   BRW1.SetLimit(1000)  ! If available
   ```

## 🛠️ Debug Tips

### 🔍 Enable Debug Information

1. **Add Debug Variables**
   ```clarion
   DebugMode            BYTE(1)
   DebugFilter          STRING(2000)
   
   ! In filter methods:
   IF DebugMode
      DebugFilter = TIF2.GetFilter()
      MESSAGE('Current Filter: ' & DebugFilter)
   END
   ```

2. **Check Filter Queue**
   ```clarion
   ! Examine filter components:
   LOOP i = 1 TO RECORDS(TIF2.FilterRows)
      GET(TIF2.FilterRows,i)
      MESSAGE('Filter ' & i & ': ' & TIF2.FilterRows.Filter)
   END
   ```

3. **Verify Events**
   ```clarion
   ! Log events:
   CASE EVENT()
   OF 4064
      MESSAGE('Filter by selection triggered')
   OF 4065
      MESSAGE('Filter excluding selection triggered')
   END
   ```

### 📊 Common Event Numbers

```clarion
! Template default events:
4064 (0FE0h) - Filter by selection
4065 (0FE1h) - Filter excluding selection
4066 (0FE2h) - Filter for dialog
4067 (0FE3h) - Remove filter
4068 (0FE4h) - Contains/Less than equal
4069 (0FE5h) - Not contains/Greater than equal
4070 (0FE6h) - Load saved filter
4071 (0FE7h) - Save current filter
```

## 📞 Getting Help

### 🔧 Before Contacting Support

1. **Check Version Compatibility**
   - Clarion version vs library version
   - ABC vs Legacy Clarion template family

2. **Verify Complete Setup**
   - Extension enabled
   - All files in correct locations
   - Template properly configured

3. **Test with Demo Application**
   - Use included demo to verify installation
   - Compare your implementation to working demo

### 📧 Support Information

- **Fomin Tools**: [http://fomintools.com](http://fomintools.com)
- **Original Author**: Edgard L. Riba (Taboga Software)
- **Version**: 2016.11.16

### 📋 Information to Include

When reporting issues, include:
- Clarion version
- Template family (ABC/Legacy)
- Complete error messages
- Minimal reproduction case
- Template configuration settings

---

*Most issues can be resolved by carefully following the setup instructions and verifying the configuration matches the working examples.*
