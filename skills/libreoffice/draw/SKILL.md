---
name: libreoffice-draw
description: "Comprehensive vector graphics and diagram creation with LibreOffice Draw. Create ODG drawings, convert between formats (SVG, PDF, PNG), automate diagram generation, and integrate with office workflows."
source: personal
risk: safe
domain: office-productivity
category: graphics-processing
version: 1.0.0
---

# LibreOffice Draw

## Overview

LibreOffice Draw skill for creating, editing, converting, and automating vector graphics and diagram workflows using the native ODG (OpenDocument Drawing) format.

## When to Use This Skill

Use this skill when:
- Creating vector graphics and diagrams in ODG format
- Converting between ODG, SVG, PDF, PNG formats
- Automating diagram and flowchart generation
- Creating technical drawings and schematics
- Batch processing graphics operations
- Working with open standards for vector graphics

## Core Capabilities

### 1. Graphics Creation
- Create new ODG drawings from scratch
- Generate diagrams from templates
- Create flowcharts and org charts
- Design technical drawings
- Build vector illustrations

### 2. Format Conversion
- ODG to other formats: SVG, PDF, PNG, JPG
- Other formats to ODG: SVG, PDF
- Batch conversion of multiple files
- Preserve layers, vectors, and quality

### 3. Diagram Automation
- Template-based diagram generation
- Automated flowchart creation
- Dynamic shape and connector generation
- Batch diagram production
- Data-driven visualization

### 4. Graphics Manipulation
- Shape creation and manipulation
- Path and bezier curve editing
- Layer management
- Text and label insertion
- Image embedding and positioning

### 5. Integration
- Command-line automation via soffice
- Python scripting with UNO
- Integration with workflow tools
- REST API integration

## Workflows

### Creating a New Drawing

#### Method 1: Command-Line
```bash
soffice --draw template.odg
```

#### Method 2: Python with UNO
```python
import uno

def create_drawing():
    local_ctx = uno.getComponentContext()
    resolver = local_ctx.ServiceManager.createInstanceWithContext(
        "com.sun.star.bridge.UnoUrlResolver", local_ctx
    )
    ctx = resolver.resolve(
        "uno:socket,host=localhost,port=8100;urp;StarOffice.ComponentContext"
    )
    smgr = ctx.ServiceManager
    doc = smgr.createInstanceWithContext("com.sun.star.drawing.DrawingDocument", ctx)
    page = doc.getDrawPages().getByIndex(0)
    # Add shapes to page
    doc.storeToURL("file:///path/to/drawing.odg", ())
    doc.close(True)
```

### Converting Drawings

```bash
# ODG to SVG
soffice --headless --convert-to svg drawing.odg

# ODG to PDF
soffice --headless --convert-to pdf drawing.odg

# ODG to PNG
soffice --headless --convert-to png:PNG_drawing drawing.odg

# SVG to ODG
soffice --headless --convert-to odg drawing.svg

# Batch convert
for file in *.odg; do
    soffice --headless --convert-to pdf "$file"
done
```

### Flowchart Automation
```python
import subprocess
import tempfile
from pathlib import Path

def create_flowchart(shapes_data, output_path):
    """
    Create flowchart from shape definitions
    shapes_data: list of shape definitions
    """
    # Create template-based flowchart
    # Implementation depends on specific requirements
    pass
```

## Format Conversion Reference

### Supported Input Formats
- ODG (native), SVG, PDF

### Supported Output Formats
- ODG, SVG, PDF, PNG, JPG, GIF, BMP, WMF, EMF

## Command-Line Reference

```bash
soffice --headless
soffice --headless --convert-to <format> <file>
soffice --draw  # Draw
```

## Python Libraries

```bash
pip install ezodf     # ODF handling
pip install odfpy     # ODF manipulation
pip install svgwrite  # SVG generation
```

## Best Practices

1. Use layers for organization
2. Create templates for recurring diagrams
3. Use vector formats for scalability
4. Name objects for easy reference
5. Store ODG source files in version control
6. Test conversions thoroughly
7. Export to SVG for web use
8. Handle conversion failures gracefully
9. Log automation operations
10. Clean temporary files

## Troubleshooting

### Cannot open socket
```bash
killall soffice.bin
soffice --headless --accept="socket,host=localhost,port=8100;urp;"
```

### Quality Issues in PNG Export
```bash
# Use higher resolution
soffice --headless --convert-to png:PNG_drawing_Export   --filterData='{"Width":2048,"Height":2048}' drawing.odg
```

## Resources

- [LibreOffice Draw Guide](https://documentation.libreoffice.org/)
- [UNO API Reference](https://api.libreoffice.org/)
- [SVG Specification](https://www.w3.org/TR/SVG/)

## Related Skills

- libreoffice-writer
- libreoffice-calc
- libreoffice-impress
- libreoffice-base
- workflow-automation
