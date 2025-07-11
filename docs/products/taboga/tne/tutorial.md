# Tutorial

The TNativeExcel library is a native excel biff format interface for writing Excel 2003 files (BIFF8). Below we have a number of short example programs which show the basics of working with the library.

Please note that all of these tutorial examples have been included in the **TNativeExcelDemo.APP** included with this library.

For each example we have included the source code formatted in Courier New, and right afterwards the expected output.

## 1. 🎯 Basic Examples

### ✨ Minimum example of writing some labels

```clarion   
CODE
!#############################################################################
! EXAMPLE 1:  Simple excel workbook
!
! (c) Taboga Software (2009)
!#############################################################################
       program
    map
    end
 INCLUDE('TWorkbook.INC')
o       TWorkbook
           code
  o.Init()								!Initialize the library
  o.TakeLabel('A1','Test')				!Add some labels
  o.TakeLabel('A2','Second line')
  o.TakeLabel('A3','3line')
  o.TakeLabel('A4','4 line')
  o.TakeLabel('A10','40 line')
  o.Save()								!Save spreadsheet (library will ask for filename).
  o.Kill								!Finish up
```

### 🎨 Another simple example, but we now work with different fonts.   

```clarion   
CODE
       program
!#############################################################################
! EXAMPLE 2:  FONT PROPERTIES
!
! (c) Taboga Software (2009)
!#############################################################################
       program
 INCLUDE('TWorkbook.INC'),ONCE
    map
    end
o       TWorkbook

xFont1      SHORT
xFont2      SHORT
xFont3      SHORT
xFont4      SHORT
           code
  o.Init()
  o.SetColumnWidth(1,1,60)			!Set the width of the first column to 60 characters…
  !
  !By default, the font is Arial 10.   This is excel's default font.
  !
  !You can specify what font to use in one of three ways:
  !(1) You can change specific font properties one by one.
  !    
  !(2) You can call the SetFont method right before writing to the worksheet
  !    cells.   This changes ALL the font properties at once.
  !
  !(3) You can call SetFont first and store the FontID returned into a local
  !    variable, and calling SetCurrentFontID before writing data.
  !

  !1)  We can specify font properties one by one:
  o.style.FontName = 'Times New Roman'
  o.TakeLabel('A1','Times New Roman, and uses default size of 10')

  o.style.FontStyle = FONT:BOLD
  o.TakeLabel('A2','Add bold to font properties')

  o.style.FontColor = COLOR:RED
  o.TakeLabel('A3','Make color red now...')

  o.style.Reset()      !Font properties are RESET here...

  o.TakeLabel('A4','style.Reset() called, default is Arial 10')


  !2)  We can call directly the SetFont method before writing data:
  o.SetFont('Arial',10)                                 
  o.TakeLabel('A11','Arial 10')
  o.SetFont('Times New Roman', 10,, FONT:BOLD)          
  o.TakeLabel('A12','Time New Roman 10, BOLD')
  o.SetFont('Arial', 10, COLOR:BLUE , FONT:BOLD+FONT:UNDERLINE)     
  o.TakeLabel('A13','Arial 10, Bold&Underline + Blue')
  o.SetFont('Arial', 16,, FONT:BOLD + FONT:ITALIC)       
  o.TakeLabel('A14','Arial 15, Bold & Italic')
  o.SetFont('Arial', 12,,FONT:UNDERLINE,,,xlsUnderline::Double)
  o.TakeLabel('A15','Courier New 12, underline style = Double')
  o.SetFont('Arial', 12,,FONT:UNDERLINE,,,xlsUnderline::SingleAccounting)
  o.TakeLabel('A16,'Courier New 12, underline style = Single Accounting')
  o.SetFont('Arial', 12,,FONT:UNDERLINE,,,xlsUnderline::DoubleAccounting)
  o.TakeLabel('A17','Courier New 12, underline style = Double Accounting')

  !3)  You can also store a "Font ID" for the fonts you want and call
  !    SetCurrentFontID to set the font before writing.  If you have
  !    to change fonts a lot this method is faster.
  xFont1 = o.SetFont('Arial',10)                                 !font1
  xFont2 = o.SetFont('Times New Roman', 10,, FONT:BOLD)          !font2
  xFont3 = o.SetFont('Arial', 10, COLOR:BLUE , FONT:BOLD+FONT:UNDERLINE)     !font3
  xFont4 = o.SetFont('Arial', 16,, FONT:BOLD+FONT:ITALIC)        !font4

  o.SetCurrentFontID(xFont1)
  o.TakeLabel('A21','This uses font ID# = ' & xFont1)
  o.SetCurrentFontID(xFont2)
  o.TakeLabel('A22','This uses font ID# = ' & xFont2)
  o.SetCurrentFontID(xFont3)
  o.TakeLabel('A23','This uses font ID# = ' & xFont3)
  o.SetCurrentFontID(xFont4)
  o.TakeLabel('A24','This uses font ID# = ' & xFont4)

  o.SetCurrentFontID(xFont1)
  o.TakeLabel('A25','this cell also uses font ID# ' & xFont1)
  o.SetFont('Arial',10)     !This has the same effect as doing o.SetCurrentFontID(xFont1)
  o.TakeLabel('A26','this cell uses font id# ' & o.CurrentFontID & ', which it got using  ' & |
                    'SetFont')
  o.SetCurrentFontID(xFont4)
  o.TakeLabel('A27','This last cell uses FontID# ' & xFont4 & ', i.e. Arial 16 + BOLD +ITALIC '&|
                    'again, using the previously used font....')
  o.Save
  o.Kill  
```

### 📐 This example shows how you can change a column's width and a row's height.     

```clarion   
CODE
!#############################################################################
! EXAMPLE 3:  Row Height and column width…
!
! (c) Taboga Software (2009)
!#############################################################################
 	program
    map
    end
 INCLUDE('TWorkbook.INC')

o       TWorkbook
           code
  o.Init()
  o.SetRowHeight(1,30)
  o.TakeLabel('A1','Excel Writer Class Demo #2')
  o.SetColumnWidth(1,1,40)							!Set column width to 40 characters wide…
  o.Save
  o.Kill
```

### 🎨 This example shows how to specify field format pictures.     

```clarion   
CODE
       program
!#############################################################################
! EXAMPLE 4:  Format Pictures
!
! (c) Taboga Software (2009)
!#############################################################################

 INCLUDE('TWorkbook.INC')
    map
    end
o       TWorkbook
           code
  o.Init
  o.TakeNumber('A1',5.12345)

  o.style.FormatPicture = '@N8.2'
  o.TakeNumber('A2',5.12345)

  o.style.FormatPicture = '@N8.3'
  o.TakeNumber('A3',5.12345)

  o.style.FormatPicture = '@N8.4'
  o.TakeNumber('A4',5.12345)

  o.style.FormatPicture = '@N8.5'
  o.TakeNumber('A5',5.12345)


  o.Save
  o.Kill
```

## 2. 🧮 Formulas

### ⚡ Various simple formulas

```clarion   
CODE
!#############################################################################
! FORMULA EXAMPLE 1:  Some simple formulas
!
! (c) Taboga Software (2009)
!#############################################################################
       program

 INCLUDE('TWorkbook.INC')
    map
    end
o       TWorkbook
           code
  o.Init()                              !Initialize the library.....
  !
  !Add some numbers to the worksheet...
  o.TakeNumber('A1',1)
  o.TakeNumber('A2',2) 
  o.TakeNumber('A3',3)
  o.TakeNumber('A4',4)
  o.TakeNumber('A5',5)
  o.TakeNumber('A6',6)
  o.TakeNumber('A7',7)
  o.TakeNumber('A8',8)
  o.TakeNumber('A9',9)
  o.TakeNumber('A10',10)
  o.TakeNumber('A11',11)
  o.TakeNumber('A12',12)
  o.TakeNumber('A13',13)
  o.TakeNumber('A14',14)
  o.TakeNumber('A15',15)
  o.TakeNumber('A16',16)
  o.TakeNumber('A17',17)
  o.TakeNumber('A18',18)
  o.TakeNumber('A19',19)
  o.TakeNumber('A20',20)
  !
  !Do some basic formulas...
  o.Takeformula('B1','=3+5+3')
  o.Takeformula('B2','=(3+5*3)-4+3')
  o.Takeformula('B3','=B$5')
  o.Takeformula('B4','=ABS(A1)')
  o.Takeformula('B5','=ABS(44.33)')
  o.Takeformula('B6','=2+4*5')
  o.Takeformula('B7','=A3*-A1')
  o.Takeformula('B8','=COUNT(A1:A6)')
  o.Takeformula('B9','=3%')
  o.Takeformula('B10','=4*3*2')
  o.Takeformula('B11','=4^3^2')
  o.Takeformula('B12','=SUM(A1,A2:A4,B6,C8:D10)')
  o.Takeformula('B13','=SUM(A2,A1+3)')
  o.Takeformula('B14','=SUM(A1,B1+4,C1:D34,ABS($F15))')
  o.Takeformula('B15','=2*4+4*3-2+11-14/3*11*1-3')
  o.Takeformula('B16','=SUM(A1)')
  o.Takeformula('B17','=4^3*2')
  o.Takeformula('B18','=4*3^2')
  !
  o.Save
  o.Kill
```

### 🔢 More formulas

```clarion   
       program

 INCLUDE('TWorkbook.INC')
    map
    end
o       TWorkbook
           code
  o.Init()
  !
  !Add some numbers...
  o.TakeNumber('A1',1)
  o.TakeNumber('A2',2)
  o.TakeNumber('A3',3)
  o.TakeNumber('A4',4)
  o.TakeNumber('A5',5)
  o.TakeNumber('A6',6)
  o.TakeNumber('A7',7)
  o.TakeNumber('A8',8)
  o.TakeNumber('A9',9)
  o.TakeNumber('A10',10)
  o.TakeNumber('A11',11)
  o.TakeNumber('A12',12)
  o.TakeNumber('A13',13)
  o.TakeNumber('A14',14)
  o.TakeNumber('A15',15)
  o.TakeNumber('A16',16)
  o.TakeNumber('A17',17)
  o.TakeNumber('A18',18)
  o.TakeNumber('A19',19)
  o.TakeNumber('A20',20)


  o.TakeFormula('B1','=COUNT(A1:A20)')
  o.TakeFormula('B2','=IF(A1=1,1,0)')       
  o.TakeFormula('B3','=ISNA(A35:A57)')
  o.TakeFormula('B4','=ISERROR(A35:A57)')
  o.TakeFormula('B5','=SUM(A1:A20)')
  o.TakeFormula('B6','=AVERAGE(A1:A20)')
  o.TakeFormula('B7','=MIN(A1:A20)')
  o.TakeFormula('B8','=MAX(A1:A20)')
  o.TakeFormula('B9','=ROW(A1)')
  o.TakeFormula('B10','=COLUMN(A1)')
  o.TakeFormula('B11','=NA()')
  o.TakeFormula('B12','=NPV(A1,A20)')
  o.TakeFormula('B13','=STDEV(A1:A20)')
  o.TakeFormula('B14','=DOLLAR(A1,A20)')
  o.TakeFormula('B15','=FIXED(A1,A20)')
  o.TakeFormula('B16','=SIN(A20)')
  o.TakeFormula('B17','=COS(A20)')
  o.TakeFormula('B18','=TAN(A20)')
  o.TakeFormula('B19','=ARCTAN(A20)')
  o.TakeFormula('B20','=PI()')
  o.TakeFormula('B21','=SQRT(A20)')
  o.TakeFormula('B22','=EXP(A5)')
  o.TakeFormula('B23','=LN(A20)')
  o.TakeFormula('B24','=LOG10(A20)')
  o.TakeFormula('B25','=ABS(-A20*100)')
  o.TakeFormula('B26','=INT(A8/A3)')
  o.TakeFormula('B27','=SIGN(-A20)')
  o.TakeFormula('B28','=ROUND(A20*4.22413,A2)')

  
  !
  o.Save()
  o.Kill 
```
