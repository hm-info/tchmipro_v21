# Barcode Printing Data Specification from CNC File

**Document Version:** 1.0
**Date:** 2026-06-19
**Status:** Draft

---

# 1. Purpose

This document defines the format and processing requirements for barcode printing data embedded within CNC files.

The barcode printing system shall extract part information from the CNC file and print barcode labels for all parts in the specified sequence.

---

# 2. Scope

This specification applies to all CNC files generated for production where barcode labels are required.

The barcode printing data shall be provided within a dedicated section of the CNC file.

---

# 3. Data Structure

The barcode data section shall be enclosed between the following markers:

```text id="h14u2e"
(LABEL_DATA_START)

...

(LABEL_DATA_END)
```

Only records located between these markers shall be processed by the barcode printing software.

---

# 4. Record Format

Each record shall contain the following information:

```text id="q5kq3f"
(PartID,PartName,PartLength,Position:1,Part1,1010,1)
```

## Field Description

| Field      | Description                     |
| ---------- | ------------------------------- |
| PartID     | Unique identifier of the part   |
| PartName   | Name of the part                |
| PartLength | Part length in millimeters      |
| Position   | Sequential position of the part |

---

# 5. Example

```text id="9ifv2j"
(LABEL_DATA_START)

(PartID,PartName,PartLength,Position:1,Frame_Left,1010,1)
(PartID,PartName,PartLength,Position:2,Frame_Right,1010,2)
(PartID,PartName,PartLength,Position:3,Sash_Top,1020,3)
(PartID,PartName,PartLength,Position:4,Sash_Bottom,1020,4)

(LABEL_DATA_END)
```

---

# 6. Processing Requirements

The software shall:

1. Load the CNC file.
2. Locate the LABEL_DATA section.
3. Read all records between LABEL_DATA_START and LABEL_DATA_END.
4. Parse each record.
5. Generate one barcode label for each valid record.
6. Print labels sequentially according to the Position field.

---

# 7. Error Handling

The software shall:

* Ignore records located outside the LABEL_DATA section.
* Log invalid or malformed records.
* Continue processing remaining valid records.
* Generate an error message if the LABEL_DATA section cannot be found.

---

# 8. Acceptance Criteria

The implementation shall be considered successful when:

* All records inside the LABEL_DATA section are detected.
* One barcode label is generated for each valid record.
* Labels are printed in the correct sequence.
* Invalid records do not interrupt processing.
* Missing LABEL_DATA sections generate a warning or error message.

---

# 9. Future Extensions

Additional fields may be introduced in future revisions, including:

* Profile Name
* Order Number
* Customer Name
* Material Type
* Production Batch Number

The parser should be designed to support future extensions where possible.
