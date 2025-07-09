# Installation & Setup

Ready to extend your Clarion application with runtime reporting? This guide walks you through how to install Fomin Report Builder (FRB), register its templates, and integrate it into your project.

---

## 🧩 System Requirements

- **Clarion Versions**: Clarion for Windows 2.0, Clarion 4, or Clarion 5
- **Template Chains Supported**: Legacy and ABC
- **Target Architecture**: 16-bit or 32-bit (FRB supports both)
- **Operating System**: Windows 95 and later (including modern OS support via compatibility layers)

---

## 📁 File Structure Overview

FRB consists of:

| Folder               | Purpose                                  |
|----------------------|-------------------------------------------|
| `BIN/`               | Runtime DLLs and Help files               |
| `TEMPLATE/`          | FRB and FRBC template sets                |
| `LIB/` and `LIBSRC/` | Library files and Report Builder Classes |
| `IMAGES/`            | Icons for ReportManager Tree             |
| `EXAMPLE.FRB/`       | Sample app (`.APP`, `.DCT`, `.TPS`, etc) |
| `WINDOWS/`           | `frb_nls.ini` (for NLS support)           |

Place these files according to your installed Clarion version:
C5/ 
├── BIN/ 
│	│   
│	├── C5FRB.DLL 
│	│   
│	├── C5FRBx.DLL 
│	│   
│	└── *.HLP 
├── LIB/ 
│	│   
│	└── *.LIB 
└── TEMPLATE/ 
	│   
	├── frb.tpl 
	│   
	├── frbc.tpl 
	│   
	└── frblocal.tpw

> ℹ️ DLLs must be included in your `PATH` or co-located with your executable.

---

## 🧰 Step-by-Step Installation

### 1. Template Registration

In the Clarion IDE:

- Navigate to `Setup` → `Template Registry`
- Click `Register`
- Add the following templates:
  - `frb.tpl` (Legacy)
  - `frbc.tpl` (ABC)

> 🔁 Do this for each Clarion version you plan to use (e.g. C4, C5).

---

### 2. Global Extensions

In your `.APP`:

- Add `RussianLocale` extension (optional for non-English languages)
- Add `CorrectReportClosing` to your Frame procedure (ensures proper MDI child shutdown)

---

### 3. Runtime Integration

Create the following procedures using the FRB template set:

| Procedure Type     | Description                                   |
|--------------------|-----------------------------------------------|
| `RunTimeReport`     | Core runtime editor/engine for `.RPT` files  |
| `ReportManager`     | Tree-based interface for managing reports     |

You can use one `RunTimeReport` procedure per table group (VIEW structure), then reference them from one or more `ReportManager` procedures.

> 💡 Want to launch FRB from any point in your app? Use the `LaunchRunTimeReport` code template or invoke it manually using:
```clarion
RTReportFileName     = 'filename.rpt'
RTReportSearchPath   = 'path'
RTReportEditAction   = 1  ! 0=Print, 1=Edit
## 🌐 National Language Support (NLS)

FRB supports multilingual UI out-of-the-box:

| Language | Method                     |
|----------|----------------------------|
| English  | Built-in                   |
| Russian  | Built-in                   |
| Other    | Via `FRB_NLS.INI` file     |

### 📝 Setup Steps

1. Copy `frb_nls.ini` to your Windows directory (e.g. `C:\Windows`)
2. Translate messages inside `frb_nls.ini` to your target language
3. Set the `CLACASE` environment variable for correct runtime casing of non-English variables

> ℹ️ If `frb_nls.ini` is missing, it will be generated automatically with English defaults when the app runs.

---

## 💾 Sample Application

Explore the included inventory example at `EXAMPLE.FRB\`, which contains:

- `inventry.app`, `inventry.dct`, `inventry.tps`: Application source and data
- Sample `.rpt` files to explore reporting capabilities
- Usage of `IdentifyFields`, `InvokeDictionary`, and formula-driven reports

This is a great starting point for testing, prototyping, or adapting into real-world systems.

---

## ✅ What’s Next?

Time to [empower your users](usage.md)! Learn how they can build and manage rich report forms—without writing a line of code.

> _“Just install once—your users handle the rest.”_