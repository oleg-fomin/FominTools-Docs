# Taboga Interactive Filter

**Taboga Interactive Filter** provides powerful, intuitive filtering capabilities for Clarion browse procedures. Originally created by Edgard L. Riba (Taboga Software) in 2008-2009 and enhanced by Fomin Tools, this library transforms standard browse windows into sophisticated data filtering interfaces.

## ✨ Key Features

### 🎯 **Right-Click Filtering**
- Filter by selection with a simple right-click
- Filter excluding selection for inverse filtering
- Context-sensitive menu options based on data type

### 🔧 **Advanced Filter Dialog**
- Multiple operator support (equals, contains, between, etc.)
- Data type-aware filtering (string, numeric, date, time)
- Complex criteria with AND/OR logic
- Visual operator selection interface

### 💾 **Filter Persistence**
- Save frequently used filters with custom names
- Load saved filters across application sessions
- Collection-based organization by browse context
- Import/export filter configurations

### ⚡ **Performance Optimized**
- Instant filter application
- Efficient query generation
- SQL and ISAM driver support
- Smart case sensitivity handling

### 🎨 **Visual Feedback**
- Toggle button shows active filter state
- Optional filter display control
- Context-sensitive popup menus
- Professional icon set included

## 🏗️ Architecture

The library is built around the `TInteractiveFilterClass` which integrates seamlessly with ABC browse classes:

```clarion
TIF2                 CLASS(TInteractiveFilterClass)
BrowseResetSort        PROCEDURE(BYTE Force),DERIVED
BrowseSetFilter        PROCEDURE(STRING Filter,STRING Id),DERIVED
InitComponents         PROCEDURE(),BYTE,PROC,DERIVED
                     END
```

## 🎯 Use Cases

### 📊 **Data Analysis**
- Quick filtering for data exploration
- Complex criteria for detailed analysis
- Saved filters for recurring reports

### 🔍 **User Interface Enhancement**
- Intuitive right-click operations
- No training required for end users
- Professional appearance and behavior

### 🏢 **Enterprise Applications**
- Multi-user filter sharing
- Standardized filtering across modules
- Consistent user experience

## 📋 Requirements

- **Clarion**: Version 6.3 or later
- **ABC Templates**: Required for ABC version
- **Template Family**: ABC or Legacy Clarion support
- **File Drivers**: TopSpeed, SQL, or other Clarion-supported drivers

## 🚀 Quick Start

1. **Enable the Extension**
   ```
   Add TEnableInteractiveFilter extension to your application
   ```

2. **Add to Browse**
   ```
   Apply TInteractiveFilter control template to your browse window
   ```

3. **Configure Fields**
   ```
   Auto-populate from browse or manually select fields
   ```

4. **Compile and Run**
   ```
   Ready to use - no additional code required!
   ```

## 📖 Documentation Structure

- **[Getting Started](getting-started.md)** - Installation and basic setup
- **[Tutorial](tutorial.md)** - Step-by-step implementation guide
- **[Template Guide](template-guide.md)** - Template configuration options
- **[API Reference](api-reference.md)** - Complete class and method reference
- **[Examples](examples.md)** - Real-world implementation examples
- **[Troubleshooting](troubleshooting.md)** - Common issues and solutions

## 🎬 Video Demonstration

*[Video walkthrough will be added here showing the interactive filtering capabilities]*

## 📞 Support

For support and additional information:

- **Email**: [support@fomintools.com](mailto:support@fomintools.com)
- **Fomin Tools**: [https://fomintools.com](https://fomintools.com)
- **Original Author**: Edgard L. Riba ([Taboga Software](https://web.archive.org/web/20220401002815/http://tabogasoftware.com/taboga_report_s_data_exporter_library41.htm))

---

*Transform your Clarion browse windows with professional, intuitive filtering capabilities that your users will love!*
