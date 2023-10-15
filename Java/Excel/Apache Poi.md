# Apache POI

## Introduction

## Sample Snippets

**1. Creating an XSSFWorkbook and Writing to a File:**
```java
XSSFWorkbook workbook = new XSSFWorkbook();
try (FileOutputStream fileOut = new FileOutputStream("workbook.xlsx")) {
    workbook.write(fileOut);
}
```

**2. Creating Sheets:**
```java
XSSFWorkbook workbook = new XSSFWorkbook();
XSSFSheet sheet1 = workbook.createSheet("Sheet1");
```

**3. Creating Cells:**
```java
XSSFWorkbook workbook = new XSSFWorkbook();
XSSFSheet sheet = workbook.createSheet("Sheet1");
```

**4. Creating Date Cells:**
```java
cell.setCellValue(new Date());

XSSFCellStyle cellStyle = workbook.createCellStyle();
cellStyle.setDataFormat(workbook.getCreationHelper().createDataFormat().getFormat("m/d/yy h:mm"));
cell = row.createCell(1);
cell.setCellValue(new Date());
cell.setCellStyle(cellStyle);
```

**5. Working with Different Types of Cells:**
```java
row.createCell(0).setCellValue(1.1);
row.createCell(1).setCellValue(new Date());
row.createCell(2).setCellValue(Calendar.getInstance());
row.createCell(3).setCellValue("a string");
row.createCell(4).setCellValue(true);
```

**6. Files vs InputStreams:
Files is better than InputStream as it has low memory requirements
```java
XSSFWorkbook workbook;
try (InputStream inp = new FileInputStream("workbook.xlsx")) {
    workbook = new XSSFWorkbook(inp);
}
```

**7. Demonstrating Alignment Options:**
```java
XSSFCellStyle cellStyle = workbook.createCellStyle();
cellStyle.setAlignment(HorizontalAlignment.CENTER);
cellStyle.setVerticalAlignment(VerticalAlignment.BOTTOM);

XSSFCell cell = row.createCell(0);
cell.setCellValue("Align It");
cell.setCellStyle(cellStyle);
```

**8. Working with Borders:**
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

**9. Iterate Over Rows and Cells:**
```java
for (Row row : sheet) {
    for (Cell cell : row) {
        // Do something here
    }
}
```

**10. Iterate Over Cells with Control of Missing/Blank Cells:**
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

**11. Getting Cell Contents:**
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

**12. Text Extraction:**
```java
try (InputStream inp = new FileInputStream("workbook.xlsx")) {
    XSSFWorkbook workbook = new XSSFWorkbook(inp);
    XSSFRichTextString richText = new XSSFRichTextString(workbook.getSheetAt(0).getRow(0).getCell(0));
    String text = richText.getString();
}
```

**13. Fills and Colors:**
```java
XSSFCellStyle style = workbook.createCellStyle();
style.setFillForegroundColor(IndexedColors.ORANGE.getIndex());
style.setFillPattern(FillPatternType.SOLID_FOREGROUND);
cell.setCellStyle(style);
```

## Dos and Don't

## References