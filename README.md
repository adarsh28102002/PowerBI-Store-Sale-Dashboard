Store Sale Dashboard | Power BI Project
An interactive and dynamic Power BI Dashboard designed to analyze retail store performance. This project provides a comprehensive view of sales metrics, customer trends, product performance, and regional analysis, along with a detailed drill-down into salesperson and store data.

<img width="1332" height="751" alt="image" src="https://github.com/user-attachments/assets/2ec79fcb-e4e4-4f28-b826-fec520e18a81" />


🚀 Key Features
1. Executive Summary Dashboard
KPI Cards: Real-time tracking of Total Net Sales (4.34M), Total Customers (1371), Total Returns (372), and Average Discount (7.31%).
Year-over-Year (YoY) Growth: Dynamic comparison with the Previous Year (PY) using conditional formatting. Green arrows (▲) indicate growth, and Red arrows (▼) indicate a decline.
Dynamic Filtering: Slicers on the left panel for Year, Store Location, Region Manager, Customer Type, and Payment Method.

Visualizations:
Total Net by Product: Highlights top-performing products (Printer, Laptop, Tablet, etc.).
Total Net by Month: Area chart showing seasonal sales trends.
Total Net by Region: Column chart comparing North, East, West, Central, and South.
Donut Charts: Breakdown by Customer Type (Retail vs. Wholesale), Store Location, and Payment Method.

2. Detailed Data Tables
Region Manager Performance: Matrix view showing sales by Store (A, B, C, D) for each manager.
Salesperson Performance: Individual sales contribution by store.
Product Analysis: Cross-tabulation of Products vs. Store Locations.
<img width="1322" height="742" alt="image" src="https://github.com/user-attachments/assets/630c9625-79a5-4995-90a2-0fefc3ac6505" />


🧮 DAX Formulas & Data Modeling
This project heavily relies on advanced DAX for dynamic calculations and conditional formatting. Below are the key formulas used:

Date Table Creation
dax
Date = CALENDAR(MIN(Sheet1[Date]), MAX(Sheet1[Date]))
Month = MONTH('Date'[Date])
Year = YEAR('Date'[Date])
Day = DAY('Date'[Date])

<img width="747" height="466" alt="image" src="https://github.com/user-attachments/assets/e88e6450-3dc6-466a-8a52-0b087efab01d" />


Core KPI Measures
dax
Total Net = SUM(Sheet1[Net Total])
Average Discount = AVERAGE(Sheet1[Discount])
Total Customer = DISTINCTCOUNT(Sheet1[CustomerName])
Total Return = SUM(Sheet1[Returned])
<img width="1082" height="142" alt="image" src="https://github.com/user-attachments/assets/e83dd970-56fe-495b-857c-0c80a06d59d0" />


Previous Year Comparison & Growth Arrows
dax
Previous Year = 
VAR PY = CALCULATE([Total Net], SAMEPERIODLASTYEAR('Date'[Date]))
VAR CY = [Total Net]
VAR Pct = IF(PY = 0 || ISBLANK(PY), BLANK(), DIVIDE(CY - PY, PY))
VAR Arrow = IF(CY >= PY, UNICHAR(9650), UNICHAR(9660)) /* Up or Down Arrow */
RETURN
IF(
    ISBLANK(PY) || PY = 0,
    BLANK(),
    FORMAT(Pct, "0.00%") & " " & Arrow
)

Dynamic Arrow Color Formatting
dax
Arrow Color = 
VAR PY = CALCULATE([Total Net], SAMEPERIODLASTYEAR('Date'[Date]))
VAR CY = [Total Net]
RETURN
IF(
    ISBLANK(PY) || PY = 0,
    BLANK(),
    IF(CY >= PY, "#2ECC71", "#E74C3C") /* Green for up, Red for down */
)

Dynamic Color for Bar Charts (Traffic Light Logic)
dax
Product Color Segment = 
VAR CitySales = [Total Net]
VAR MaxSales = MAXX(ALLSELECTED(Sheet1[Product]), [Total Net])
VAR MinSales = MINX(ALLSELECTED(Sheet1[Product]), [Total Net])
RETURN
SWITCH(
    TRUE(),
    CitySales = MaxSales, "#DD2D4A", /* Dark Red for Highest */
    CitySales = MinSales, "#F26A8D", /* Light Pink for Lowest */
    "#F49CBB"                        /* Neutral Pink for Others */
)

(Optional) Dynamic KPI Selection using a disconnected table:

dax
KPI List = 
DATATABLE(
    "KPI Name", STRING,
    "Sort", INTEGER,
    {
        {"Total Net", 1},
        {"Average Discount", 2},
        {"Total Return", 3},
        {"Total Customer", 4}
    }
)

Selected KPI Value = 
SWITCH(
    SELECTEDVALUE('KPI List'[KPI Name]),
    "Total Net", [Total Net],
    "Average Discount", [Average Discount],
    "Total Return", [Total Return],
    "Total Customer", [Total Customer],
    BLANK()
)

🛠️ Tools & Technologies Used
Power BI Desktop (Data Visualization & Reporting)

DAX (Data Analysis Expressions) (Dynamic Measures & Time Intelligence)
Power Query (Data Transformation - assumed)

📂 Repository Contents
Store_Sale_Dashboard.pbix: The main Power BI project file.
Formula_Sale_Store_Dashboard.docx: Documentation of all DAX measures.
README.md: Project documentation.

🚀 How to Use
Clone this repository or download the 
Open the file in Power BI Desktop.
Interact with the slicers on the left panel to filter data by Year, Location, Manager, Customer Type, or Payment Method.
Hover over the charts and KPI cards to see detailed tooltips.
