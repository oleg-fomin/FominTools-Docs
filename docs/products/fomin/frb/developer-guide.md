# Developer Guide

This section covers how to integrate Fomin Report Builder into your Clarion application as a developer. It includes template usage, embedding logic, managing multiple `RunTimeReport` procedures, and empowering your users with calculated fields and expressions.

Whether you’re building a fresh reporting module or retrofitting an established legacy app, FRB gives you flexible options and runtime extensibility.

---

## 🏗️ Adding FRB to Your Clarion App

To add FRB support:

1. **Register Templates** (`frb.tpl`, `frbc.tpl`) in Template Registry
2. In your `.APP`:
   - Add `CorrectReportClosing` to your Frame procedure
   - (Optional) Add `RussianLocale` or handle `CLACASE` in environment
3. Create one or more `RunTimeReport` procedures (each defines a VIEW structure)
4. Add a `ReportManager` to organize report files
5. Expose variables and procedures as “Other Data” and “User Functions”

---

## 🔁 Designing `RunTimeReport` Procedures

Each `RunTimeReport` procedure defines:

| Setting                  | Purpose                                                               |
|--------------------------|-----------------------------------------------------------------------|
| Available File Fields    | Defines table structure (VIEW) for reporting                         |
| Field Name Binding       | Chooses dictionary field for runtime names (e.g. `External Name`)    |
| Other Data Variables     | Exposes app-level variables to report expressions                    |
| User Functions           | Enables custom logic in formula engine                               |
| Print Preview Option     | Determines behavior (Ask, Always, Never)                             |
| Pre-print Hooks          | Embed code into report lifecycle (Before Print Detail, etc.)         |

> 📌 You can create multiple `RunTimeReport` procedures, each bound to a unique file group or report type.

---

## 🌲 Managing Reports with `ReportManager`

- Add one or more `ReportManager` procedures
- Use `ReportManagerTree` to list `.rpt` files by type
- Define root names, search paths, and associated `RunTimeReport` procedures
- Customize interface with:
  - `ReportManagerUpdateButtons`
  - `ReportManagerSelectButton`
  - `ReportManagerExpandContractButtons`

Use variables or `.INI` files to allow user-defined search paths at runtime.

---

## 🧠 Bindable Data Fields

FRB accesses table and variable names using Clarion’s `BIND()` mechanism.

Bindable fields can come from:

- **Dictionary**: Tables, keys, and fields in the VIEW structure
- **Global/Module/Local Variables**: Added under “Other Data”
- **Derived Results**: Totals defined in report layout
- **Formulas**: Expressions within frame contents using `=`

> 🧑‍🔧 Don’t leave dictionary descriptions blank—FRB uses them for the field selector UI.

---

## 🧮 Exposing User-Defined Functions

You can expose custom procedures to the `EVALUATE()` runtime engine.

### Example:

| Property          | Value                                 |
|-------------------|----------------------------------------|
| Procedure Label   | `CalculateTax`                         |
| BIND Label        | `CalcTax`                              |
| Parameters        | `GROSS, RATE`                          |
| Description       | `Computes tax from gross and rate`     |

Use in report frame:

```clarion
=CalcTax(AmountDue, 0.15)
```

You can also expose SQL logic for advanced users:

```clarion
=SQL('SELECT COUNT(*) FROM Orders')
```

> ⚠️ Be mindful of execution context—some expressions run inside virtual methods.

---

## 📦 Variables Across Report Lifecycle

Variable scoping matters depending on when and where you want values to persist.

| Scope         | Use Case                                |
|---------------|------------------------------------------|
| Global        | Visible everywhere, stable across calls  |
| Module        | Ideal for shared intermediate state      |
| Local         | Do not use in `Before Print Detail` (CW2.0 only)  |

Use **Formula Class** embed points to assign or recalculate values during runtime. Supported classes include:

- Procedure Setup
- Before Print Detail
- Before Leaving Procedure

---

## ⚙️ Launching FRB from Code

Instead of using `ReportManager`, you can launch reports programmatically:

```clarion
RTReportFileName    = 'sales_summary.rpt'
RTReportSearchPath  = 'C:\Reports\'
RTReportEditAction  = 1  ! 1 = edit mode; 0 = print
START(RunTimeReportProc)
```

Use the `LaunchRunTimeReport` code template to simplify setup and avoid manual stack/thread logic.

---

## 🧩 Extend with `IdentifyFields`

To support inline expressions in your own procedures:

1. Add the `IdentifyFields` extension template
2. Add `InvokeDictionary` control template beside your formula entry field
3. Let users write expressions referencing app-level variables and field bindings

Ideal for filters, calculations, and search conditions outside of FRB reports.

---

## 🔍 Debugging Tips

- Enable “Preview Mode: Always” when testing report logic
- Use diagnostic `FRAME` contents to display formula output
- Validate field bindings using `BIND()` errors in the generated code

> 🔄 Use “Other Data” and `EMBED` code to test logic before running actual reports.

---

## ✅ Next Step

See how the FRB architecture works in more detail in [components.md](components.md), or explore [installation.md](installation.md) if you haven’t set up yet. To explore runtime behavior from the user’s perspective, read [usage.md](usage.md).

> _“You write the VIEWs and hook in the logic—your users take care of the layout.”_
