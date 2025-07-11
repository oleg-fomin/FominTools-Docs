# Barcode Symbologies

The barcode library supports the following symbologies:

## 🎯 Standard Symbologies

### 🔳 Code 39
By far the most common barcode scheme outside retail area and is read by most scanners, although it is not as compact as Code 93 or Code 128. The normal Code 39 scheme encode both numbers and upper case letters, and was the first alphanumeric symbology.

### 🔲 Code 39 Extended
Similar to Code39 except that this allows the full ASCII 128 character set.

### 🔲 Code 128
Very high density coding scheme. It has three coding schemes and permits the inclusion of special characters not present on the keyboard. The Taboga Barcode library **automatically** determines the **optimum** coding scheme to use.

#### ⚙️ Function Codes
For special applications like UCC128 barcodes, use these special characters:

| Function Code | Character to Use |
|---------------|------------------|
| Function 1    | `<201>`          |
| Function 2    | `<202>`          |
| Function 3    | `<203>`          |
| Function 4    | `<204>`          |

### 🔳 Code 128 Variants
- **Code 128A**: Only using character table "A"
- **Code 128B**: Only using character table "B"
- **Code 128C**: Only using character table "C"
- **UCC128**: CODE128 variant with FNC1 added:
  ```
  <STARTC><FNC1><Barcode Data><103 Mod CheckDigit><StopChar>
  ```

### 🛍️ UPC/EAN
- **UPC A**: Universal Product Code version A. Encodes 11 digits.
- **UPC E**: Universal Product Code version E. Compressed version of UPC A.
- **EAN 13**: Main scheme used throughout Europe for retail article numbering. Numeric only, encodes 12 digits + check digit.
- **EAN 8**: Encodes 7 digits + check digit.

### 🏭 Industrial Codes
- **Interleaved 2 of 5**: High density, continuous numeric symbology coding digit pairs. Adds leading 0 for odd digit count.
- **Code 2 of 5**: Similar to Interleaved 2 of 5, but with lower density. Also known as Industrial 2 of 5.
- **Codabar**: Encodes 16 different characters: digits 0-9, and '-', '$', '/', '+'. Optional check digit.

## 🔄 Derived Symbologies

These symbologies are built using base symbologies:

- **EAN128**: Used mostly in shipping containers (Code 128 based)
- **JAN**: Japanese version of EAN13 (prefix 49)
- **ISBN**: Book numbering (EAN13 with prefix 978)
- **ISSN**: Periodical numbering (EAN13 with prefix 977)
- **Ansi 3 of 9**: Alternate name for CODE39
- **Ansi Extended 3 of 9**: Alternate name for Extended CODE39
- **Industrial 2 of 5**: Alternate name for Code 2 of 5
