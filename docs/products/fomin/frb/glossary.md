# Glossary

This glossary provides definitions and examples for the core concepts used in Fomin Report Builder (FRB). It's a handy reference for understanding the sections, controls, and expressions you'll encounter while creating or editing reports.

---

## 🧩 Report Sections

FRB reports are composed of stacked sections, each with a specific purpose:

| Section         | Description                                                               |
|------------------|---------------------------------------------------------------------------|
| **Page Header**   | Appears at the top of each page                                           |
| **Report Header** | Printed once at the start of the report (first page only)                |
| **Group Break**   | Logical anchor for sorting/grouping (not printed)                        |
| **Group Header**  | Printed above each new group based on the Group Break                    |
| **Detail**        | Prints once per record; the main data section                             |
| **Group Footer**  | Totals or summaries printed after each group                             |
| **Report Footer** | Printed once at the end of the report                                     |
| **Page Footer**   | Appears at the bottom of each page                                        |

---

## 📦 Frame

A **Frame** is the primary visual element in FRB’s layout editor:

- Think of it as a text box or dynamic label
- Holds static text or evaluated expressions
- Supports formatting: alignment, font, color, borders, fill
- May include conditional logic or formulas

If a Frame starts with `=`, the content is treated as an expression to evaluate (e.g. `=CustomerName` or `=SUM(Total)`).

---

## 🖋️ Picture

A **Picture** is a formatting pattern applied to data inside a Frame:

- Controls display of numbers, dates, text
- Example: `@D17` formats a date as `YYYY-MM-DD`
- Used like: `=FORMAT(TODAY(), @D17)`

---

## 📄 `.rpt` File

A report file that:

- Stores layout, filter, sorting, and frame info
- Is independent of your app’s executable
- Can be copied, edited, emailed, or printed without recompilation

Stored internally as a TopSpeed database for portability.

---

## 🧠 Dictionary

The built-in selector for inserting fields and functions into:

- Filters
- Sorting expressions
- Frame content
- Conditions for section printing

Includes:

- Table fields from the current VIEW
- System functions (e.g. `FORMAT()`, `TODAY()`)
- User-defined variables and functions

---

## 🎛️ Expression

FRB expressions are runtime formulas evaluated using Clarion's `EVALUATE()` function.

Examples:

- `=Amount * 1.15`
- `=FORMAT(TODAY(), @D17)`
- `=IF(Total > 10000, 'High', 'Normal')`

Used in:

- Frame contents
- Section printing conditions
- Totals
- Filters and sort orders

---

## 🧮 Totals

FRB can calculate and insert summary values:

| Type      | Description                             |
|-----------|------------------------------------------|
| **Sum**    | Adds a field’s values                   |
| **Average**| Mean of a field across records          |
| **Count**  | Tallies number of records               |
| **Min/Max**| Tracks minimum or maximum values        |
| **Evaluate**| Executes a custom expression            |

Each result is stored in a variable that can be shown in a Frame or used in another formula.

---

## ⬇️ Sort Expression

Attached to a **Group Break**, it determines:

- The field or formula to sort by
- The hierarchy of grouping in reports

Example: `=City` or `=SUBSTRING(ZipCode, 1, 3)`

---

## 🔎 Record Filter

A logical expression that limits which records are included in a report.

Examples:

- `Balance > 1000`
- `InvoiceDate >= DEFORMAT('2025/01/01', @D5)`
- `State = 'CA' OR State = 'NY'`

---

## 🔣 BIND Label

A name assigned to a variable or field so it can be referenced in report expressions.

You define BINDs for:

- Table fields (based on Dictionary)
- App-level variables ("Other Data")
- Totals and result variables
- Custom user-defined procedures

---

## 🧩 Control Templates

Reusable UI elements that tie Clarion controls to FRB runtime:

| Template                    | Purpose                                       |
|-----------------------------|-----------------------------------------------|
| `ReportManagerTree`         | Shows `.rpt` files in a hierarchical list     |
| `InvokeDictionary`          | Button to open the Dictionary window          |
| `ReportManagerUpdateButtons`| Adds Insert, Change, Delete buttons           |
| `ReportManagerSelectButton` | Adds a Print button                           |
| `ReportManagerExpandContractButtons` | Add Expand/Collapse buttons         |

---

## 📑 Print Preview

A user option controlled in the `RunTimeReport` procedure:

- **Always**: Preview before printing
- **Never**: Skip preview
- **Ask**: Prompt user at runtime

Supports integration with:

- Creative Reporting Tools (CPCS)
- Report & Presentation Manager (RPM)

---

## 📐 Layout Tools

- **Property Box**: Floating window for setting Frame options
- **Section Properties**: Configure size and logic of each section
- **Frame Toolbar**: Quick access to font, alignment, color, and layout controls
- **Right-click Context Menu**: Add sections, frames, and manipulate layout

---

## ⚙️ Runtime Variables

Special variables built into FRB:

| Variable        | Purpose                              |
|------------------|--------------------------------------|
| `Page_Counter`   | Tracks current page number           |
| `Report_Title`   | Custom title string (if set)         |
| `TODAY()`        | Returns current date                 |
| `CLOCK()`        | Returns current time                 |

Use these in frames and expressions for dynamic info.

---

## 🔗 Related Pages

- [Using Fomin Report Builder](usage.md)
- [Developer Guide](developer-guide.md)
- [Components](components.md)