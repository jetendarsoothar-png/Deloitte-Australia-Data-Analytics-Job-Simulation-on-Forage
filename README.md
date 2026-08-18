# Deloitte Australia – Data Analytics Job Simulation (Forage)

This repository documents my work on the **Deloitte Australia Data Analytics Job Simulation**, hosted on [Forage](https://www.theforage.com/). The simulation is split into two tasks: analyzing machine downtime in **Tableau**, and building a pay-equity classification in **Excel**.

---

## ✅ Task 1: Machine Downtime Analysis (Tableau)

### 🎯 Objective
Daikibo Industrials collects telemetry data from IoT-enabled machines across its factories. The goal was to answer:
1. In which location did machines break down the most?
2. What machine types broke down most often at that location?

### 🧰 Tools Used
- Tableau Desktop / Tableau Public

### 🔧 Process
1. **Import data** — Connected Tableau to `daikibo-telemetry-data.json` (JSON connection), checking all schema levels so device, status, and location fields were available.
2. **Create a calculated field** — `Unhealthy`:
   ```
   IF [Status] = "unhealthy" THEN 10 ELSE 0 END
   ```
   Since each telemetry message represents a 10-minute interval, every "unhealthy" reading counts as 10 minutes of downtime.
3. **Build Chart 1 — "Down Time per Factory"**: `Factory` on Columns, `SUM(Unhealthy)` on Rows, sorted descending.
4. **Build Chart 2 — "Down Time per Device Type"**: `Device Type` on Columns, `SUM(Unhealthy)` on Rows.
5. **Combine into a Dashboard**: dragged both sheets onto one dashboard, then set Chart 1 as a filter (**Use as Filter**) so selecting a factory dynamically updates Chart 2 to show only that location's device breakdown.
6. **Final step**: selected the factory with the highest downtime and captured the dashboard.

### 📊 Findings

**Downtime by Factory**

| Factory | Total Downtime (min) |
|---|---|
| **Daikibo Factory Seiko** | **480** |
| Daikibo Shenzhen | 420 |
| Daikibo Factory Meiyo | 110 |
| Daikibo Berlin | 20 |

**Downtime by Device Type (company-wide)**

| Device Type | Downtime (min) |
|---|---|
| LaserWelder | 480 |
| LaserCutter | 430 |
| HeavyDutyDrill | 70 |
| Furnace | 20 |
| SpotWelder | 10 |
| CNC | 10 |
| ConveyorBelt | 10 |
| MetalPress | 0 |
| AirWrench | 0 |

**Downtime by Device Type — within Seiko specifically**

| Device Type | Downtime (min) |
|---|---|
| LaserWelder | 480 |

### 📌 Conclusion
- **Daikibo Factory Seiko** had the highest downtime (480 min), and within that factory, **100% of the downtime came from LaserWelder units** — no other machine type there registered an unhealthy status.
- **LaserWelder** and **LaserCutter** together account for the large majority of company-wide downtime, making them the top priority for predictive maintenance.
- **MetalPress** and **AirWrench** reported zero downtime across all factories — a benchmark for reliability.
- **Daikibo Berlin** had the least downtime overall (20 min); its maintenance practices could be a model for the other three sites.

---

## ✅ Task 2: Pay Equality Classification (Excel)

### 🎯 Objective
Classify each job role's **Equality Score** (an integer from -100 to +100, where 0 is ideal) into one of three categories, to help identify where gender pay inequality is most severe.

### 🧰 Tools Used
- Microsoft Excel

### 🔧 Process
1. Opened `Equality Table.xlsx`, which contained: `Factory`, `Job Role`, `Equality Score`.
2. Added a 4th column: **`Equality class`**.
3. In cell `D2`, entered the formula and filled it down through all rows:
   ```excel
   =IF(AND(C2>=-10,C2<=10),"Fair",IF(AND(C2>=-20,C2<=20),"Unfair","Highly Discriminative"))
   ```
   (Equivalent single-line version: `=IF(ABS(C2)>20,"Highly Discriminative",IF(ABS(C2)>10,"Unfair","Fair"))`)

### Classification Rule
| Equality Score | Class |
|---|---|
| -10 to +10 (inclusive) | **Fair** |
| -20 to -11, or +11 to +20 | **Unfair** |
| Below -20, or above +20 | **Highly Discriminative** |

### 🧾 Sample Validation
| Factory | Job Role | Equality Score | Equality Class |
|---|---|---|---|
| Daikibo Factory Meiyo | C-Level | -25 | Highly Discriminative |
| Daikibo Factory Seiko | Manager | -21 | Highly Discriminative |
| Daikibo Shenzhen | Engineer | -4 | Fair |

### 📌 Conclusion
- Senior roles (C-Level, VP, Director, Sr. Manager) consistently show the largest equality gaps, particularly at **Daikibo Factory Meiyo**, **Seiko**, and **Shenzhen**.
- **Daikibo Berlin** had the most "Fair" classifications across roles — the most gender pay-equitable location of the four.
- These results support prioritizing pay-equity review for senior/managerial roles company-wide.

---

## 📁 Deliverables
- `Down Time per Factory` and `Down Time per Device Type` Tableau dashboard (screenshot included)
- `Task_5_Equality_Table.xlsx` with completed `Equality class` column

---
*Part of the Deloitte Australia Data Analytics Job Simulation on [Forage](https://www.theforage.com/).*
