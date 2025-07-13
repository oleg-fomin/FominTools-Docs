# Building Reports

With Fomin Report Builder (FRB), your users can create, customize, preview, and print reports—right from your application—without needing any development skills.

This guide walks through everyday tasks like inserting new reports, applying filters, building layouts, and navigating the ReportManager interface.

---

## 🧭 Navigating ReportManager

The `ReportManager` procedure is the user’s central hub for managing reports. Its tree-style interface displays available `.rpt` files grouped by type or folder.

### ReportManager actions:

| Action         | Description                            |
|----------------|----------------------------------------|
| Insert         | Create a new report file               |
| Change         | Edit an existing report                |
| Delete         | Remove a report from the list          |
| Print          | Send the selected report to the printer |
| Expand/Contract | Collapse or expand report groups       |

> 💡 The layout and available actions depend on your use of `ReportManagerTree` and supporting control templates (e.g. `ReportManagerUpdateButtons`).

---

## 📄 Creating a New Report

1. Launch your Clarion app and open the `ReportManager`
2. Click **Insert** to start a new report
3. The **Report Formatter** opens—a canvas for laying out content using "frames"
4. Use the toolbar or right-click to:
   - Add report sections (headers, details, footers, groups)
   - Insert frames for data, formulas, or static text
5. Save the file with a `.rpt` extension (e.g. `CustomerList.rpt`)

> ✏️ Pro Tip: You can also clone an existing report using **Change → Save As...**, then tweak the layout.

---

## 🧱 Report Sections at a Glance

FRB lets users build multi-section reports. Each section controls how and when content prints:

| Section         | Purpose                                                 |
|------------------|---------------------------------------------------------|
| Page Header      | Appears at the top of each page                         |
| Report Header    | Printed once at the top of the first page               |
| Group Break      | Defines sort order (not printed directly)               |
| Group Header     | Printed before each group of records                    |
| Detail           | Core data section (one per record)                      |
| Group Footer     | Totals or summaries after a group                       |
| Report Footer    | Final totals and remarks (last page only)               |
| Page Footer      | Appears at the bottom of each page                      |

Add, remove, or reorder sections using the section menu or right-click context options.

---

## 🧮 Adding Data Frames

Frames are visual containers that show text, variables, or expressions.

### To add a frame:

1. Select a report section
2. Click **New Frame** or right-click → **Insert Frame**
3. Resize and position using your mouse or arrow keys
4. Set contents:
   - Text (e.g. `Customer Report`)
   - Expression (e.g. `=CustomerName`)
   - Formula (e.g. `=FORMAT(TODAY(),@D17)`)

### Formatting options:

- Horizontal & vertical alignment
- Fonts, colors, and borders
- Dynamic filling
- `Picture` formatting (e.g. date, time, currency)

> 🧠 Expressions starting with `=` are evaluated using Clarion's `EVALUATE()` function.

---

## 🔍 Sorting, Filters & Totals

### 🎯 Filtering Records

In **Report Properties**:

- Use logical expressions to limit records (e.g. `Balance > 1000`)
- Combine with `AND`, `OR`, `>=`, etc.

Use the [Dictionary] button to insert fields.

### 🔢 Sorting (Group Breaks)

To sort records:

1. Insert a **Group Break**
2. Set a **Sort Expression** (e.g. `LastName`)
3. Add **Group Header/Footer** to display grouped values or totals

Group order determines sorting hierarchy.

### Σ Calculating Totals

You can define result variables for:

- Sum
- Average
- Count
- Min / Max
- Custom `Evaluate()` expressions

Configure **Tally On** and **Reset** rules to control when calculations run (per record, group, or page).

---

## 🖨️ Printing & Previewing

Before printing, you can:

- Set page layout (margins, size, orientation)
- Define preview mode (always, never, ask at runtime)
- Choose print destination (printer or file)

Preview options include support for third-party tools like:

- **CPCS** (Creative Reporting Tools)
- **RPM** (Report and Presentation Manager)

---

## 🌍 Using the Dictionary & Formulas

Press **F2** or click the [Dictionary] button when entering:

- Filters
- Sort expressions
- Frame formulas

You’ll see a tree-style reference for:

- Available table fields
- Clarion operators and functions
- User-defined variables and totals

This makes building powerful expressions easy—even for non-programmers.

---

## ⛏️ Power Features

- **Save As...**: Duplicate reports quickly for reuse
- **Frame Copy/Paste**: Speed up layout building
- **Suppress Printing**: Use conditional logic to hide sections (e.g. `Page_Counter = 1`)
- **InvokeDictionary Button**: Lookup available formula variables at runtime

---

## 🧑‍🏫 Example: Date & Time in a Frame

Add this expression to a frame in the header:

```clarion
=FORMAT(TODAY(), @D17) & ' ' & FORMAT(CLOCK(), @T1)
```

> Outputs something like: `2025-07-09 18:45:00`

---

## 🧠 Keyboard Tips

- `Ctrl + Tab` → Cycle through sections
- `Ctrl + Arrow` → Move selected frame
- `Tab` / `Shift + Tab` → Select next/previous frame
- `Ctrl + Click` → Multi-select frames

---

## ✅ Next Step

Dive into the [developer guide](developer-guide.md) to see how to plug FRB’s capabilities into your Clarion project—including multiple `RunTimeReport` procedures, user functions, and embedding FRB actions in your code.

> _“Let your users own their reports. You just build the tools.”_
