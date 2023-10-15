# Apache POI

## Introduction

## Sample Snippets

**. Creating an XSSFWorkbook and Writing to a File:**
```java
XSSFWorkbook workbook = new XSSFWorkbook();
try (FileOutputStream fileOut = new FileOutputStream("workbook.xlsx")) {
    workbook.write(fileOut);
}
```

**. Creating Sheets:**
```java
XSSFWorkbook workbook = new XSSFWorkbook();
XSSFSheet sheet1 = workbook.createSheet("Sheet1");
```

**. Creating Cells:**
```java
XSSFWorkbook workbook = new XSSFWorkbook();
XSSFSheet sheet = workbook.createSheet("Sheet1");
```

**. Creating Date Cells:**
```java
cell.setCellValue(new Date());

XSSFCellStyle cellStyle = workbook.createCellStyle();
cellStyle.setDataFormat(workbook.getCreationHelper().createDataFormat().getFormat("m/d/yy h:mm"));
cell = row.createCell(1);
cell.setCellValue(new Date());
cell.setCellStyle(cellStyle);
```

**. Working with Different Types of Cells:**
```java
row.createCell(0).setCellValue(1.1);
row.createCell(1).setCellValue(new Date());
row.createCell(2).setCellValue(Calendar.getInstance());
row.createCell(3).setCellValue("a string");
row.createCell(4).setCellValue(true);
```

**. Files vs InputStreams:
Files is better than InputStream as it has low memory requirements
```java
XSSFWorkbook workbook;
try (InputStream inp = new FileInputStream("workbook.xlsx")) {
    workbook = new XSSFWorkbook(inp);
}
```

**. Demonstrating Alignment Options:**
```java
XSSFCellStyle cellStyle = workbook.createCellStyle();
cellStyle.setAlignment(HorizontalAlignment.CENTER);
cellStyle.setVerticalAlignment(VerticalAlignment.BOTTOM);

XSSFCell cell = row.createCell(0);
cell.setCellValue("Align It");
cell.setCellStyle(cellStyle);
```

**. Working with Borders:**
```java
XSSFCellStyle style = workbook.createCellStyle();
style.setBorderBottom(BorderStyle.THIN);
style.setBottomBorderColor(IndexedColors.BLACK.getIndex());
style.setBorderLeft(BorderStyle.THIN);
style.setLeftBorderColor(IndexedColors.GREEN.getIndex());
style.setBorderRight(BorderStyle.THIN);
style.setRightBorderColor(IndexedColors.BLUE.getIndex());
style.setBorderTop(BorderStyle.MEDIUM_DASHED);
style.setTopBorderColor(IndexedColors.BLACK.getIndex());
cell.setCellStyle(style);
```

**. Iterate Over Rows and Cells:**
```java
for (Row row : sheet) {
    for (Cell cell : row) {
        // Do something here
    }
}
```

**. Iterate Over Cells with Control of Missing/Blank Cells:**
```java
int rowStart = Math.min(15, sheet.getFirstRowNum());
int rowEnd = Math.max(1400, sheet.getLastRowNum());

for (int rowNum = rowStart; rowNum < rowEnd; rowNum++) {
   Row r = sheet.getRow(rowNum);
   if (r == null) {
      // Handle empty row
      continue;
   }
   int lastColumn = Math.max(r.getLastCellNum(), MY_MINIMUM_COLUMN_COUNT);
   for (int cn = 0; cn < lastColumn; cn++) {
      Cell c = r.getCell(cn, Row.MissingCellPolicy.RETURN_BLANK_AS_NULL);
      if (c == null) {
         // The cell is empty
      } else {
         // Do something useful with the cell's contents
      }
   }
}
```

**. Getting Cell Contents:**
```java
for (Row row : sheet) {
    for (Cell cell : row) {
        switch (cell.getCellType()) {
            case CellType.STRING:
                String stringValue = cell.getRichStringCellValue().getString();
                break;
            case CellType.NUMERIC:
                if (DateUtil.isCellDateFormatted(cell)) {
                    Date dateValue = cell.getDateCellValue();
                } else {
                    double numericValue = cell.getNumericCellValue();
                }
                break;
            case CellType.BOOLEAN:
                boolean booleanValue = cell.getBooleanCellValue();
                break;
            case CellType.FORMULA:
                String formula = cell.getCellFormula();
                break;
            case CellType.BLANK:
                // Cell is blank
                break;
            default:
                // Handle other cell types
        }
    }
}
```

**. Text Extraction:**
```java
try (InputStream inp = new FileInputStream("workbook.xlsx")) {
    XSSFWorkbook workbook = new XSSFWorkbook(inp);
    XSSFRichTextString richText = new XSSFRichTextString(workbook.getSheetAt(0).getRow(0).getCell(0));
    String text = richText.getString();
}
```

**. Fills and Colors:**
```java
XSSFCellStyle style = workbook.createCellStyle();
style.setFillForegroundColor(IndexedColors.ORANGE.getIndex());
style.setFillPattern(FillPatternType.SOLID_FOREGROUND);
cell.setCellStyle(style);
```

Certainly, here's a summary of the key points from the provided code examples with minimal Java code snippets for each task:

1. **Merging Cells:**
    - Merge cells from row 1, columns 1 to 2.

   ```java
   sheet.addMergedRegion(new CellRangeAddress(1, 1, 1, 2));
   ```

2. **Working with Fonts:**
    - Create and style a font, set it to a cell, and write it to a file.

   ```java
   Font font = workbook.createFont();
   font.setFontHeightInPoints((short) 24);
   font.setFontName("Courier New");
   font.setItalic(true);

   CellStyle style = workbook.createCellStyle();
   style.setFont(font);

   Cell cell = row.createCell(1);
   cell.setCellValue("This is a test of fonts");
   cell.setCellStyle(style);
   ```

3. **Custom Colors (HSSF):**
    - Apply custom colors to cells, both foreground and font color.

   ```java
   HSSFCellStyle style = wb.createCellStyle();
   style.setFillForegroundColor(HSSFColor.LIME.index);
   style.setFillPattern(FillPatternType.SOLID_FOREGROUND);

   HSSFFont font = wb.createFont();
   font.setColor(HSSFColor.RED.index);
   style.setFont(font);
   ```

4. **Custom Colors (XSSF):**
    - Apply custom colors using XSSF (for newer Excel files).

   ```java
   XSSFCellStyle style = wb.createCellStyle();
   style.setFillForegroundColor(new XSSFColor(new java.awt.Color(128, 0, 128), new DefaultIndexedColorMap()));
   style.setFillPattern(FillPatternType.SOLID_FOREGROUND);
   ```

5. **Reading and Rewriting Workbooks:**
    - Load an existing workbook, modify it, and save it.

   ```java
   Workbook wb = WorkbookFactory.create(new FileInputStream("workbook.xls"));
   // Modify the workbook.
   try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
       wb.write(fileOut);
   }
   ```

6. **Using Newlines in Cells:**
    - Insert newlines within a cell and enable text wrapping.

   ```java
   CellStyle cs = wb.createCellStyle();
   cs.setWrapText(true);

   Cell cell = row.createCell(2);
   cell.setCellValue("Use \n with word wrap on to create a new line");
   cell.setCellStyle(cs);
   ```

7. **Data Formats:**
    - Apply custom data formats to cells.

   ```java
   CellStyle style = wb.createCellStyle();
   DataFormat format = wb.createDataFormat();
   style.setDataFormat(format.getFormat("0.0"));

   Cell cell = row.createCell(0);
   cell.setCellValue(11111.25);
   cell.setCellStyle(style);
   ```

8. **Fit Sheet to One Page:**
    - Set up a sheet to fit one page when printed.

   ```java
   PrintSetup ps = sheet.getPrintSetup();
   sheet.setAutobreaks(true);
   ps.setFitHeight((short) 1);
   ps.setFitWidth((short) 1);
   ```

9. **Set Print Area:**
    - Define a print area for the sheet.

   ```java
   wb.setPrintArea(0, "$A$1:$C$2");
   ```

10. **Set Page Numbers on Footer:**
    - Add page numbers to the footer.

    ```java
    Footer footer = sheet.getFooter();
    footer.setRight("Page " + HeaderFooter.page() + " of " + HeaderFooter.numPages());
    ```

11. **Using Convenience Functions:**
    - Utilize convenience functions for setting borders and styles.

    ```java
    RegionUtil.setBorderBottom(BorderStyle.MEDIUM_DASHED, region, sheet1, wb);
    RegionUtil.setBorderTop(BorderStyle.MEDIUM_DASHED, region, sheet1, wb);
    ```

12. **Shift Rows Up or Down:**
    - Shift rows on a sheet up or down.

    ```java
    sheet.shiftRows(5, 10, -5);
    ```

13. **Set a Sheet as Selected:**
    - Mark a sheet as selected.

    ```java
    sheet.setSelected(true);
    ```

14. **Set the Zoom Magnification:**
    - Set the zoom magnification for a sheet.

    ```java
    sheet1.setZoom(75);
    ```

15. **Splits and Freeze Panes:**
    - Create freeze and split panes on sheets.

    ```java
    sheet1.createFreezePane(3, 2, 3, 2);
    sheet2.createSplitPane(2000, 2000, 0, 0, Sheet.PANE_LOWER_LEFT);
    ```

16. **Repeating Rows and Columns:**
    - Set up repeating rows and columns for printouts.

    ```java
    sheet1.setRepeatingRows(CellRangeAddress.valueOf("4:5"));
    sheet2.setRepeatingColumns(CellRangeAddress.valueOf("A:C"));
    ```

17. **Headers and Footers:**
    - Add headers to a sheet.

    ```java
    Header header = sheet.getHeader();
    header.setCenter("Center Header");
    header.setLeft("Left Header");
    header.setRight("Right Header");
    ```

18. **Drawing Shapes:**
    - Create shapes on a sheet.

    ```java
    HSSFPatriarch patriarch = sheet.createDrawingPatriarch();
    HSSFSimpleShape shape = patriarch.createSimpleShape(a);
    shape.setShapeType(HSSFSimpleShape.OBJECT_TYPE_OVAL);
    ```

19. **Images:**
    - Add an image to a sheet.

    ```java
    InputStream is = new FileInputStream("image1.jpeg");
    byte[] bytes = IOUtils.toByteArray(is);
    int pictureIdx = wb.addPicture(bytes, Workbook.PICTURE_TYPE_JPEG);
    is.close();

    CreationHelper helper = wb.getCreationHelper();
    Sheet sheet = wb.createSheet();
    Drawing drawing = sheet.createDrawingPatriarch();
    ClientAnchor anchor = helper.createClientAnchor();
    Picture pict = drawing.createPicture(anchor, pictureIdx);
    pict.resize();
    ```

20. **Outlining:**
    - Create outlines to group rows and columns.

    ```java
    sheet.groupRow(5, 14);
    sheet.groupColumn(4, 7);
    ```

**Named Ranges and Named Cells:**

```java
// Creating Named Range
Name namedCell = wb.createName();
namedCell.setNameName("TestName1");
String reference = "TestSheet!$A$1:$A$1"; // Area reference
namedCell.setRefersToFormula(reference);

// Reading from Named Range
int namedCellIdx = wb.getNameIndex("TestName1");
Name aNamedCell = wb.getNameAt(namedCellIdx);

// Accessing Named Cell
AreaReference aref = new AreaReference(aNamedCell.getRefersToFormula());
CellReference[] crefs = aref.getAllReferencedCells();
```

**Reading Cell Comments:**

```java
Cell cell = sheet.getRow(3).getCell(5);
Comment comment = cell.getCellComment();
if (comment != null) {
    RichTextString str = comment.getString();
    String author = comment.getAuthor();
}
```

**Adjust Column Width to Fit Contents:**

```java
Sheet sheet = workbook.getSheetAt(0);
sheet.autoSizeColumn(0); // Adjust width of the first column
```

**Reading Hyperlinks:**

```java
Sheet sheet = workbook.getSheetAt(0);
Cell cell = sheet.getRow(0).getCell(0);
Hyperlink link = cell.getHyperlink();
if (link != null) {
    System.out.println(link.getAddress());
}
```

**Data Validations - Create an Explicit List Constraint:**

```java
Sheet sheet = workbook.createSheet("Data Validation");
DataValidationHelper dvHelper = sheet.getDataValidationHelper();
DataValidationConstraint dvConstraint = dvHelper.createExplicitListConstraint(
    new String[]{"10", "20", "30"});
CellRangeAddressList addressList = new CellRangeAddressList(0, 0, 0, 0);
DataValidation validation = dvHelper.createValidation(dvConstraint, addressList);
sheet.addValidationData(validation);
```

**Creating Linked or Dependent Drop-Down Lists:**

```java
// Create a named range for the first list
Name namedRange = workbook.createName();
namedRange.setNameName("FirstList");
namedRange.setRefersToFormula("DataValidation!$A$1:$A$3");

// Create a data validation for the first list
CellRangeAddressList firstListAddress = new CellRangeAddressList(0, 0, 0, 0);
DataValidationHelper dvHelper = sheet.getDataValidationHelper();
DataValidationConstraint firstListConstraint = dvHelper.createFormulaListConstraint("FirstList");
DataValidation firstListValidation = dvHelper.createValidation(firstListConstraint, firstListAddress);
sheet.addValidationData(firstListValidation);

// Create named ranges for the options in the second list
Name namedRangeA = workbook.createName();
namedRangeA.setNameName("OptionA");
namedRangeA.setRefersToFormula("DataValidation!$B$1:$B$3");

Name namedRangeB = workbook.createName();
namedRangeB.setNameName("OptionB");
namedRangeB.setRefersToFormula("DataValidation!$C$1:$C$3");

// Create a data validation for the second list with INDIRECT
CellRangeAddressList secondListAddress = new CellRangeAddressList(0, 0, 1, 1);
DataValidationConstraint secondListConstraint = dvHelper.createFormulaListConstraint("INDIRECT(UPPER($A$1))");
DataValidation secondListValidation = dvHelper.createValidation(secondListConstraint, secondListAddress);
sheet.addValidationData(secondListValidation);
```


**Autofilters:**
```java
XSSFWorkbook wb = new XSSFWorkbook();
XSSFSheet sheet = wb.createSheet();
sheet.setAutoFilter(new CellRangeAddress(4, 199, 2, 5));
```

**Conditional Formatting:**
```java
XSSFWorkbook workbook = new XSSFWorkbook();
XSSFSheet sheet = workbook.createSheet();
XSSFSheetConditionalFormatting sheetCF = sheet.getSheetConditionalFormatting();
XSSFFontFormatting fontFmt = new XSSFFontFormatting();
fontFmt.setFontStyle(true, false);
fontFmt.setFontColor(new XSSFColor(java.awt.Color.DARK_RED));
XSSFBorderFormatting bordFmt = new XSSFBorderFormatting();
bordFmt.setBorderBottom(BorderStyle.THIN);
bordFmt.setBorderTop(BorderStyle.THICK);
bordFmt.setBorderLeft(BorderStyle.DASHED);
bordFmt.setBorderRight(BorderStyle.DOTTED);
XSSFDataFormat fmt = (XSSFDataFormat) workbook.createDataFormat();
XSSFColor yellow = new XSSFColor(java.awt.Color.YELLOW);
XSSFColor red = new XSSFColor(java.awt.Color.RED);
XSSFColor blue = new XSSFColor(java.awt.Color.BLUE);
XSSFColor green = new XSSFColor(java.awt.Color.GREEN);
XSSFColor auto = new XSSFColor(java.awt.Color.BLACK);
XSSFConditionalFormattingRule rule1 = sheetCF.createConditionalFormattingRule(ComparisonOperator.EQUAL, "0");
rule1.createFontFormatting().setFontStyle(true, false);
rule1.createFontFormatting().setFontColor(red);
rule1.createBorderFormatting().setBorderBottom(BorderStyle.THIN);
rule1.createBorderFormatting().setBorderTop(BorderStyle.THICK);
rule1.createBorderFormatting().setBorderLeft(BorderStyle.DASHED);
rule1.createBorderFormatting().setBorderRight(BorderStyle.DOTTED);
rule1.createPatternFormatting().setFillBackgroundColor(yellow);
XSSFConditionalFormattingRule rule2 = sheetCF.createConditionalFormattingRule(ComparisonOperator.BETWEEN, "-10", "10");
XSSFConditionalFormattingRule[] cfRules = { rule1, rule2 };
CellRangeAddress[] regions = { CellRangeAddress.valueOf("A3:A5") };
sheetCF.addConditionalFormatting(regions, cfRules);
```

**Hiding and Un-Hiding Rows:**
Hiding Rows:
```java
XSSFWorkbook workbook = new XSSFWorkbook();
XSSFSheet sheet = workbook.createSheet("Sheet1");
XSSFRow row = sheet.createRow(0);
row.setZeroHeight();
```
Un-Hiding Rows:
```java
XSSFWorkbook workbook = new XSSFWorkbook();
XSSFSheet sheet = workbook.getSheetAt(0);
for (Row row : sheet) {
    if (row.getZeroHeight()) {
        row.setZeroHeight(false);
    }
}
```

**Setting Cell Properties:**
```java
XSSFWorkbook workbook = new XSSFWorkbook();
XSSFSheet sheet = workbook.createSheet("Sheet1");
Map<String, Object> properties = new HashMap<>();
properties.put(CellUtil.BORDER_TOP, BorderStyle.MEDIUM);
properties.put(CellUtil.BORDER_BOTTOM, BorderStyle.MEDIUM);
properties.put(CellUtil.BORDER_LEFT, BorderStyle.MEDIUM);
properties.put(CellUtil.BORDER_RIGHT, BorderStyle.MEDIUM);
properties.put(CellUtil.TOP_BORDER_COLOR, new XSSFColor(java.awt.Color.RED));
properties.put(CellUtil.BOTTOM_BORDER_COLOR, new XSSFColor(java.awt.Color.RED));
properties.put(CellUtil.LEFT_BORDER_COLOR, new XSSFColor(java.awt.Color.RED));
properties.put(CellUtil.RIGHT_BORDER_COLOR, new XSSFColor(java.awt.Color.RED));

XSSFRow row = sheet.createRow(1);
XSSFCell cell = row.createCell(1);
CellUtil.setCellStyleProperties(cell, properties);
```

**Drawing Borders:**
```java
XSSFWorkbook wb = new XSSFWorkbook();
XSSFSheet sh = wb.createSheet("Sheet1");
XSSFPropertyTemplate pt = new XSSFPropertyTemplate();
pt.drawBorders(new CellRangeAddress(1, 3, 1, 3), BorderStyle.MEDIUM, BorderExtent.ALL);
pt.drawBorders(new CellRangeAddress(5, 7, 1, 3), BorderStyle.MEDIUM, BorderExtent.OUTSIDE);
pt.drawBorders(new CellRangeAddress(5, 7, 1, 3), BorderStyle.THIN, BorderExtent.INSIDE);
pt.drawBorders(new CellRangeAddress(9, 11, 1, 3), BorderStyle.MEDIUM, new XSSFColor(java.awt.Color.RED), BorderExtent.OUTSIDE);
pt.drawBorders(new CellRangeAddress(9, 11, 1, 3), BorderStyle.MEDIUM, new XSSFColor(java.awt.Color.BLUE), BorderExtent.INSIDE_VERTICAL);
pt.drawBorders(new CellRangeAddress(9, 11, 1, 3), BorderStyle.MEDIUM, new XSSFColor(java.awt.Color.GREEN), BorderExtent.INSIDE_HORIZONTAL);
pt.drawBorders(new CellRangeAddress(10, 10, 2, 2), BorderStyle.NONE, BorderExtent.ALL);
pt.applyBorders(sh);
```

**Creating a Pivot Table:**
```java
XSSFWorkbook wb = new XSSFWorkbook();
XSSFSheet sheet = wb.createSheet();
// Create and set data in the sheet
AreaReference source = new AreaReference("A1:D4", SpreadsheetVersion.EXCEL2007);
CellReference position = new CellReference("H5");
XSSFPivotTable pivotTable = sheet.createPivotTable(source, position);
pivotTable.addRowLabel(0);
pivotTable.addColumnLabel(DataConsolidateFunction.SUM, 1);
pivotTable.addColumnLabel(DataConsolidateFunction.AVERAGE, 2);
pivotTable.addReportFilter(3);
```

**Cells with Multiple Styles (Rich Text Strings):**
HSSF Example (XLS):
```java
HSSFCell hssfCell = row.createCell(idx);
HSSFRichTextString richString = new HSSFRichTextString("Hello, World!");
richString.applyFont(0, 6, font1);
richString.applyFont(6, 13, font2);
hssfCell.setCellValue(richString);
```

XSSF Example (XLSX):
```java
XSSFCell cell = row.createCell(1);
XSSFRichTextString rt = new XSSFRichTextString("The quick brown fox");
rt.applyFont(0, 10, font1);
rt.applyFont(10, 19, font2);
rt.append(" Jumped over the lazy dog", font3);
cell.setCellValue(rt);
```

## Dos and Don't

## References
* Chatgpt
* https://poi.apache.org/components/spreadsheet/quick-guide.html