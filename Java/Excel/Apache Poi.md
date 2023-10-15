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


## Dos and Don't

## References
* Chatgpt
* https://poi.apache.org/components/spreadsheet/quick-guide.html