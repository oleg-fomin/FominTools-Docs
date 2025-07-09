# Components

Fomin Report Builder is structured around a flexible, modular architecture that blends Clarion templates, object-oriented class libraries, and external DLLs. This lets developers easily integrate powerful reporting capabilities into their applications while giving users full control at runtime.

---

## 🧱 Core Components Overview

| Component                  | Purpose                                                   |
|----------------------------|-----------------------------------------------------------|
| RBC-library                | Object-oriented core in a DLL (Report Builder Classes)    |
| FRB/FRBC Template Sets     | Template sets for Legacy (FRB) and ABC (FRBC) chains      |
| ReportManager              | Tree-style report organizer interface                     |
| RunTimeReport              | Main runtime procedure for building/editing reports       |
| Dictionary Interface       | Field and formula expression helper                       |
| Code & Extension Templates | Templates for buttons, previewers, and code hooks         |

---

## 🔧 RBC-library

The **RBC (Report Builder Classes)** library is the compiled core of FRB.

- Delivered as a DLL or LIB (e.g. standalone `ClaFRB.DLL` or local `ClaFRBL.LIB`)
- Contains report formatter, processor, and runtime engine
- Uses object-oriented Clarion to manage `.rpt` files
- Works in MDI/SDI applications

Every FRB-enabled application loads this DLL at runtime, enabling report layout, data binding, filtering, evaluation, and printing.

> 📁 Distribute the DLL(s) with your `.EXE` without any runtime royalties.

---

## 🧩 Template Sets: FRB & FRBC

### ✅ `FRB.TPL`  
- For **Legacy** template chain  
- Works in Clarion for Windows 2.0, Clarion 4, Clarion 5

### ✅ `FRBC.TPL`
- For **ABC** template chain
- Also works with Clarion 4 and Clarion 5
- Supports the `FRBClass` object model

> 🎯 Both sets include identical functional components via Clarion templates and control templates.

---

## 🗂️ Procedures

### `RunTimeReport`
- The **main runtime report editor**
- One procedure = one report type (VIEW structure)
- Let users create, edit, filter, sort, and print `.rpt` reports
- Exposes settings like preview mode, print dialog, field binding, other data, and user functions

### `ReportManager`
- Tree-style report file browser
- Displays `.rpt` files grouped by type and directory
- Lets users Insert, Change, Delete, and Print
- Customizable root-level titles and search paths
- Built using the `ReportManagerTree` control template

> 🔄 Multiple `RunTimeReport` procedures can be registered with a single `ReportManager`.

---

## 🧰 Control Templates

These extend your Clarion UI with buttons and LIST controls that wire into FRB's runtime:

| Template Name                      | Function                                  |
|------------------------------------|-------------------------------------------|
| `ReportManagerTree`               | Tree-like view of `.rpt` files             |
| `ReportManagerUpdateButtons`      | Insert, Change, Delete buttons             |
| `ReportManagerSelectButton`       | Print selected report                      |
| `ReportManagerExpandContractButtons` | Expand/Collapse tree branches          |
| `InvokeDictionary`                | Lookup for runtime formula variables       |

Use these templates to enrich the `ReportManager` and enable deep runtime interaction.

---

## 🧠 Extension & Code Templates

| Template Name          | Use Case                                                                 |
|------------------------|--------------------------------------------------------------------------|
| `CorrectReportClosing` | Ensures proper MDI shutdown of formatter windows                        |
| `IdentifyFields`       | Exposes calculated fields to other app procedures                       |
| `PreviewCPCS`          | Enables CPCS-style preview integration                                  |
| `PreviewRPM`           | Enables Report & Presentation Manager (RPM) preview                     |
| `LaunchRunTimeReport`  | Embeds FRB report launching logic in any procedure via code injection   |

---

## 🧑‍🔧 Application Dictionary Integration

When building `RunTimeReport`, you configure:

- **Table Fields**: Available for use in filters and frames
- **Other Data**: Module/global/local variables exposed to users
- **User Functions**: Custom procedures users can reference in formulas (e.g. SQL queries)

You also define:
- Field name bindings
- Dictionary help through the [Dictionary] interface
- Optional variable prefixes by table

> 🧩 The more you expose cleanly, the more your users can leverage FRB’s flexibility.

---

## 🧮 Expression & Formula Engine

FRB uses Clarion’s `EVALUATE()` function to process:

- Arithmetic and string logic
- Date/time operations
- Trigonometric functions
- User-defined procedures
- Frame contents, section conditions, filters

Expressions are stored directly inside `.rpt` files and evaluated dynamically—no compilation needed.

---

## 📦 File Format

Each report is saved as a `.RPT` file:

- Self-contained: includes layout, data bindings, filters, and expressions
- Stored in a TopSpeed database container
- Portable, editable, and easy to distribute

Reports can be previewed, duplicated, and sent via email—no app update required.

---

## 📎 Resource Files Summary

You’ll use or deploy:

| File                          | Purpose                               |
|-------------------------------|----------------------------------------|
| `C5FRB.DLL`, `C4FRB.DLL`      | Runtime engines                        |
| `frb.tpl`, `frbc.tpl`         | Template sets for Clarion             |
| `frbclass.clw`, `frbclass.inc`| RBC class source (for reference)      |
| `frb_nls.ini`                 | National Language Support file         |
| `.rpt` files                  | User-created report definitions       |

---

## 🔗 See Also

- [Installation & Setup](installation.md)
- [Developer Guide](developer-guide.md)
- [Using Fomin Report Builder](usage.md)