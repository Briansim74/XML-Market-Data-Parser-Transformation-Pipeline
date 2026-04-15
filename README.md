# XML Market Data Parser & Transformation Pipeline

## Trading Problem
Raw market data is not usable for trading. It must be structurally consistent before it can feed pricing, risk, or execution models.

## Core Idea
Transforms hierarchical XML market feeds into structured datasets suitable for quantitative trading systems.

## Key Function
- Converts hierarchical exchange data into tabular form
- Preserves schema relationships and traceability
- Normalizes inconsistent financial data structures
- Enables downstream model reliability

## Trading Mapping
- XML feed → raw exchange data
- Parser → market data ingestion layer
- Structured output → model-ready inputs
- Transformation → pre-pricing / pre-risk conditioning

## Key Insights
- Market data is a structural problem, not a data problem
- Schema consistency determines downstream model reliability
- Parsing design affects system correctness
- Data preprocessing is a prerequisite for any trading strategy

## Dual Parsing Logic (Robustness vs Control)
- High-level parsing for flexibility (xmltodict)
- Low-level streaming parsing for deterministic control (ElementTree)

## Core Takeaway
Robust trading systems begin with correct data structure, not modeling complexity.

</br></br></br></br>


<details>
<summary><strong>Implementation Details & Usage</strong></summary>

# XML Market Data Parser & Transformation Pipeline

Financial Data Feed Handling | XML → Structured Data for Quant Systems

## Overview
This project implements a data ingestion and transformation pipeline for structured financial data, converting hierarchical XML-based datasets into normalized, tabular formats suitable for downstream quantitative analysis and trading systems.

It is designed to mirror a simplified version of a market data feed handler, a critical component in trading infrastructure where structured exchange data must be parsed, validated, and made usable for pricing, risk, and execution systems.

## Motivation
In modern electronic markets, data is not directly usable in raw form. Exchanges and clearing systems often provide:
- Deeply nested XML structures
- Hierarchical trade and instrument data
- Non-uniform schemas across datasets

<br>

Transforming this into structured, queryable data is a prerequisite for any trading or risk system.

This project focuses on building that transformation layer.

## Core Functionality
### 1. XML → Tabular Conversion
Parses hierarchical XML datasets into structured tabular formats (CSV / XLSX)
Extracts both:
- Data values
- Corresponding XPath structures (schema awareness)

### 2. Tabular → XML Reconstruction
Converts flat tabular datasets back into hierarchical XML
Supports:
Nested structures
Attribute handling
Multi-record aggregation

### 3. Recursive XML Parsing Engine
Custom recursive traversal of XML trees
Handles:
- Deep nesting
- Repeated nodes
- Attribute-tagged elements

### 4. Multi-Method Parsing Approaches
Two independent parsing strategies implemented:
- xmltodict-based parsing
  - Converts XML → JSON → structured DataFrame
  - Flexible and easy to manipulate

- ElementTree streaming parser (XMLPullParser)
  - Event-driven parsing for hierarchical traversal
  - Closer to real-world low-level feed handling

## Example Use Case (Trading Context)
This system mirrors real-world workflows such as:
- Parsing FX trade reports from clearinghouses
- Converting exchange data feeds into structured formats
- Preparing inputs for:
  - Pricing models
  - Risk engines
  - Backtesting systems

## Key Insights
### 1. Market Data is a Structural Problem
- The challenge is not access to data - but transforming it into usable structure.

### 2. Schema Awareness is Critical
- Capturing XPath alongside values ensures:
  - Traceability
  - Data integrity
  - Compatibility with downstream systems

### 3. Multiple Parsing Approaches Matter
- Different use cases require different trade-offs:
  - Flexibility (xmltodict) vs
  - Performance/control (streaming parser)

### 4. Foundation for Trading Systems
- This pipeline represents the data ingestion layer in a broader trading architecture:

```
Data → Parsing → Structuring → Strategy / Risk / Execution
```
## System Design
The pipeline follows a structured transformation flow:
```
XML Dataset
→ Parsing Layer (recursive / streaming)
→ Structured Representation (DataFrames)
→ Normalization (values + XPath mapping)
→ Output (CSV / XLSX)
```

## Technologies
- Python
- Pandas
- xmltodict
- ElementTree (XMLPullParser)
- Recursive parsing logic

## Summary
This project demonstrates how to build a data transformation layer for structured financial datasets, bridging the gap between raw exchange-style XML feeds and structured inputs required for quantitative trading systems.



# XML-CSV-File-Conversion
Convert files between XML to CSV / XLSX format

<br/>In this notebook, I will be demonstrating how to convert files between CSV / XLSX and XML formats. The Python notebook used is:

1. XML.ipynb

<br/>Firstly, I will be converting a multi-sheet XLSX document into a hierarchical XML file.

<br/>Attached are the relevant files:

1. XML_test_flat.xlsx
2. Output1.xml
3. Output2.xml

<br/>In the XML_test_flat.xlsx document, it is a multi-sheet document consisting of rows of financial data values in sheet 0 - "Output_Values", and the corresponding XPaths to guide each data value in sheet 1 - "Output_XML_Path". Firstly, I converted the 2 sheets into Pandas DataFrames. Next, I sorted the DataFrames in order according to their column names, and did some manipulation to check for any attribute tags within the XPaths. Then, I converted these DataFrames into Python dictionaries from json formatted strings. I then created a recursive function create_root using the ElementTree library. The function creates a new child node for each unexplored nested XPath, or follows the existing tree for existing XPaths, and directs each value to the end node, and if there are any attribute tags, it will add accordingly.

<br/>For Output1, each new row entry within the xlsx document is added under a new "Rpt" XPath.

<br/>For Output2, each all row entries within the xlsx document are added under the same XPath.

<br/>The XML trees are then converted into the an XML file format "Output1.xml" and "Output2.xml" respectively.

<br/>Next, I will be converting a hierarchical XML file into a multi-sheet XLSX document. There will be 2 methods that I will be converting the XML file, which will be described below.

<br/>Attached are the relevant files:

1. FX_MAS_NEWT_OTHR.xml
2. FX_MAS_NEWT_OTHR.xlsx
3. FX_MAS_NEWT_OTHR_ET.xlsx

<br/>In the FX_MAS_NEWT.OTHR.xml file, it is a hierarchical XML document consisting of nested rows of FX trading data. 

<br/>For the first method, I converted the XML file into a json format via the xmltodict library. Then, I found the XPath key that houses the relevant trade data, 'TradData'. I then created a recursive function to obtain the 'TradData' key that houses the dictionary of values of the FX data. I then created DataFrames for both the FX values and corresponding XPath addresses, then compiling the 2 DataFrames into a single XLSX file "FX_MAS_NEWT_OTHR.xlsx".

<br/>For the second method, I parsed the XML file using the XMLPullParser from the ElementTree library, then iterated through the nested XPaths via XMLPullParser's event function, which allows each XPath nested addresses and values to be parsed and entered into separate DataFrames of FX values and XPath addresses. I then similarly compiled these 2 DataFrames into a single XLSX file "FX_MAS_NEWT_OTHR_ET.xlsx".

</details>
</br></br>

</details>
</br></br>
