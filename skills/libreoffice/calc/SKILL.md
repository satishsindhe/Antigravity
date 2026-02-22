---
name: libreoffice-calc
description: "Comprehensive spreadsheet creation, editing, and automation with LibreOffice Calc. Create ODS spreadsheets, convert between formats (XLSX, CSV, PDF), automate data processing, and integrate with office workflows."
source: personal
risk: safe
domain: office-productivity
category: spreadsheet-processing
version: 1.0.0
---

# LibreOffice Calc

## Overview

LibreOffice Calc skill for creating, editing, converting, and automating spreadsheet workflows using the native ODS (OpenDocument Spreadsheet) format.

## When to Use This Skill

Use this skill when:
- Creating new spreadsheets in ODS format
- Converting between ODS, XLSX, CSV, PDF formats
- Automating data processing and analysis
- Creating formulas, charts, and pivot tables
- Batch processing spreadsheet operations
- Working with open standards for spreadsheets

## Core Capabilities

### 1. Spreadsheet Creation
- Create new ODS spreadsheets from scratch
- Generate spreadsheets from templates
- Create data entry forms
- Build dashboards and reports
- Design charts and graphs

### 2. Format Conversion
- ODS to other formats: XLSX, CSV, PDF, HTML
- Other formats to ODS: XLSX, XLS, CSV, DBF
- Batch conversion of multiple files
- Preserve formulas, formatting, charts

### 3. Data Automation
- Formula automation and calculations
- Data import from CSV, database, APIs
- Data export to various formats
- Batch data processing
- Scheduled report generation

### 4. Data Analysis
- Pivot tables and data summarization
- Statistical functions and analysis
- What-if analysis and scenarios
- Data validation and filtering
- Conditional formatting

### 5. Integration
- Command-line automation via soffice
- Python scripting with UNO
- Database connectivity (JDBC, ODBC)
- REST API integration

## Workflows

### Creating a New Spreadsheet

#### Method 1: Command-Line
```bash
soffice --calc template.ods
```

#### Method 2: Python with UNO
```python
import uno

def create_spreadsheet():
    local_ctx = uno.getComponentContext()
    resolver = local_ctx.ServiceManager.createInstanceWithContext(
        "com.sun.star.bridge.UnoUrlResolver", local_ctx
    )
    ctx = resolver.resolve(
        "uno:socket,host=localhost,port=8100;urp;StarOffice.ComponentContext"
    )
    smgr = ctx.ServiceManager
    doc = smgr.createInstanceWithContext("com.sun.star.sheet.SpreadsheetDocument", ctx)
    sheets = doc.getSheets()
    sheet = sheets.getByIndex(0)
    cell = sheet.getCellByPosition(0, 0)  # A1
    cell.setString("Hello from LibreOffice Calc!")
    doc.storeToURL("file:///path/to/spreadsheet.ods", ())
    doc.close(True)
```

#### Method 3: Using ezodf
```python
import ezodf

doc = ezodf.newdoc('ods', 'spreadsheet.ods')
sheet = doc.sheets[0]
sheet['A1'].set_value('Hello')
sheet['B1'].set_value('World')
doc.save()
```

### Converting Spreadsheets

```bash
# ODS to XLSX
soffice --headless --convert-to xlsx spreadsheet.ods

# ODS to CSV
soffice --headless --convert-to csv spreadsheet.ods

# ODS to PDF
soffice --headless --convert-to pdf spreadsheet.ods

# XLSX to ODS
soffice --headless --convert-to ods spreadsheet.xlsx

# Batch convert
for file in *.ods; do
    soffice --headless --convert-to xlsx "$file"
done
```

### Data Processing Automation
```python
import subprocess
import csv
from pathlib import Path

def process_csv_to_ods(csv_path, ods_path):
    """Convert CSV to ODS with formatting"""
    # First convert using soffice
    subprocess.run([
        'soffice', '--headless',
        '--convert-to', 'ods',
        csv_path
    ])
    
    # Then apply formatting using UNO
    # (implementation depends on specific requirements)
```

### Formula Automation
```python
import uno

def create_formula_spreadsheet():
    local_ctx = uno.getComponentContext()
    resolver = local_ctx.ServiceManager.createInstanceWithContext(
        "com.sun.star.bridge.UnoUrlResolver", local_ctx
    )
    ctx = resolver.resolve(
        "uno:socket,host=localhost,port=8100;urp;StarOffice.ComponentContext"
    )
    smgr = ctx.ServiceManager
    doc = smgr.createInstanceWithContext("com.sun.star.sheet.SpreadsheetDocument", ctx)
    sheet = doc.getSheets().getByIndex(0)
    
    # Set values
    sheet.getCellByPosition(0, 0).setDoubleValue(100)  # A1
    sheet.getCellByPosition(0, 1).setDoubleValue(200)  # A2
    
    # Set formula: =A1+A2
    cell = sheet.getCellByPosition(0, 2)  # A3
    cell.setFormula("SUM(A1:A2)")
    
    doc.storeToURL("file:///path/to/formulas.ods", ())
    doc.close(True)
```

### Batch Data Processing
```python
import subprocess
from pathlib import Path
from concurrent.futures import ProcessPoolExecutor

def convert_to_pdf(input_file, output_dir):
    subprocess.run([
        'soffice', '--headless',
        '--convert-to', 'pdf',
        '--outdir', str(output_dir),
        str(input_file)
    ])

def batch_convert(pattern, output_format, output_dir):
    input_files = list(Path('.').glob(pattern))
    Path(output_dir).mkdir(parents=True, exist_ok=True)
    
    with ProcessPoolExecutor() as executor:
        executor.map(convert_to_pdf, input_files, [output_dir] * len(input_files))
    
    print(f"Converted {len(input_files)} files")

# Usage
batch_convert('*.ods', 'pdf', './output/pdf')
```

## Format Conversion Reference

### Supported Input Formats
- ODS (native), XLSX, XLS, CSV, DBF, HTML

### Supported Output Formats
- ODS, XLSX, XLS, CSV, PDF, HTML

## Command-Line Reference

```bash
soffice --headless
soffice --headless --convert-to <format> <file>
soffice --calc  # Calc
```

## Python Libraries

```bash
pip install ezodf     # ODS handling
pip install odfpy     # ODF manipulation
pip install pandas    # Data analysis
```

## Best Practices

1. Use named ranges for clarity
2. Document complex formulas
3. Use data validation for input control
4. Create templates for recurring reports
5. Store ODS source files in version control
6. Test conversions thoroughly
7. Use CSV for data exchange
8. Handle conversion failures gracefully
9. Log automation operations
10. Clean temporary files

## Troubleshooting

### Cannot open socket
```bash
killall soffice.bin
soffice --headless --accept="socket,host=localhost,port=8100;urp;"
```

### Formula Not Calculating
```bash
# Recalculate in LibreOffice GUI or use:
soffice --headless --convert-to ods --force spreadsheet.ods
```

## Resources

- [LibreOffice Calc Guide](https://documentation.libreoffice.org/)
- [UNO API Reference](https://api.libreoffice.org/)
- [ezodf Documentation](http://ezodf.rst2.org/)

## Related Skills

- libreoffice-writer
- libreoffice-impress
- libreoffice-draw
- libreoffice-base
- xlsx-official
- workflow-automation
