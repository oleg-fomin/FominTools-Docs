# Feature Highlights

Fomin Report Builder (FRB) offers a rich set of features that empower both developers and end-users to create, manage, and print professional-quality reports—without writing code. This page highlights the key capabilities across user interface, data handling, formatting, and extensibility.

---

## 🖥️ User Interface

- Word processor–style layout editor with familiar toolbar buttons
- Supports English, Russian, and any custom language via `FRB_NLS.INI`
- Coordinate system: Inches or Metric
- Export report pages as WMF to Microsoft Word (for preview, editing, printing)
- Customize default Frame properties (font, alignment, borders, etc.)
- Per-printer scaling (X/Y % stored in `WIN.INI`) for precise hardcopy output

---

## 🗂️ Report Management

- Tree-style `ReportManager` interface with unlimited folders and report types
- Create unlimited `.rpt` files
- Use long filenames (Windows 95+ style)
- Clone reports easily via **Open → Save As...**

---

## 🧮 Data Handling & Expressions

- Uses Clarion `VIEW` structures for multi-table access (ISAM & SQL)
- High-speed filters using fields, expressions, and Clarion functions
- Sort by any field or expression
- Dictionary-driven expression builder with tree-style hierarchy
- End-user–defined totals:
  - Sum, Count, Average, Min, Max, Evaluate
  - Reset on group, page, or report end
- Calculated fields (Evaluate-only, never reset)
- Runtime prompts for user-entered values or browse selections
- Array support (up to 4 dimensions)
- Built-in date/time variables (`TODAY()`, `CLOCK()`)
- Page number and record count per page
- Picture formatting for strings, dates, numbers (e.g. SSN → `123-45-6789`)
- Wizard-style Picture builder
- Developer-defined computed fields indistinguishable from dictionary fields

---

## 🖋️ Layout & Formatting

- Supported sections: Page Header, Report Header, Group Break, Group Header, Detail, Group Footer, Report Footer, Page Footer
- Unlimited Frames per section
- Frame contents:
  - Static text
  - Dynamic expressions (e.g. `=CustomerName`)
- Borders and fills for Frames (color, thickness)
- Grid-style layout using adjacent bordered Frames
- Font selection per report, section, or Frame
- Vertical and horizontal alignment
- Multi-select Frames for group operations
- Developer-exposed runtime functions usable in expressions
- Embed graphics (e.g. logos, letterheads)
- **Trailing Text**: prints long MEMO fields with page overflow control
- **Mail Merge**: combine raw text with data fields
- **Barcode Frames**: vertical/horizontal barcodes (requires Taboga Barcode Library)

---

## 🧩 Extensibility

- Developer-defined runtime functions (e.g. `SQL()`, `CalcTax()`)
- Runtime prompts for user input (e.g. date range, selection)
- Computed fields and filters based on user input
- Support for third-party previewers:
	- CPCS
	- RPM (Lodestar)
	- IceTips
	- TinTools
- Compatible with:
	- J-Spell
	- SoftVelocity IP Driver
	- ClarioNET
	- Query Wizard
	- xSearch, xQuickFilter
	- Many others

---

## 🧠 Advanced Features

- Runtime scaling per printer (adjust X/Y % in `WIN.INI`)
- Export to Word via DDE (each page as WMF image)
- Developer-defined “invisible” computed fields
- Conditional section printing (e.g. `Page_Counter > 1`)
- Page-level counters and totals
- Dynamic Picture formatting at runtime

---

## 🔗 Related Pages

- [Using Fomin Report Builder](usage.md)
- [Developer Guide](developer-guide.md)
- [Glossary](glossary.md)