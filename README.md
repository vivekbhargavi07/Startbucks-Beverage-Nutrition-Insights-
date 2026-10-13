# Starbucks Beverage Nutrition Dashboard (Power BI)

An interactive Power BI dashboard that explores the nutrition profile of Starbucks beverages: calories, sugar, protein and caffeine across categories, milk options and sizes.

> Independent portfolio project. Not affiliated with or endorsed by Starbucks. All graphics on the dashboard background were created for this project.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f46b2019-c6df-4b0b-ad0e-ce0d4609217f" />


## Objective

Answer practical questions a customer or analyst might ask:

- Which beverage categories are highest in calories and caffeine?
- How much sugar does the average drink contain?
- Which drinks give the most caffeine?
- How do the numbers change when filtering by protein range or beverage preparation (size and milk type)?

## Dataset

| Item | Detail |
|---|---|
| File | `starbucks.csv` |
| Rows | 242 beverage and preparation combinations |
| Columns | 18 (category, beverage, preparation, calories, fat, carbohydrates, sugars, protein, sodium, cholesterol, vitamins, calcium, iron, caffeine) |
| Source | _Add the link to where you downloaded the dataset_ |

## Tools

- Power BI Desktop (Power Query, DAX, report design)
- Canva-style custom background designed as an image (1280 x 720 px) with Power BI visuals layered on top

## Data preparation (Power Query)

1. Promoted the first row to headers.
2. Set data types for every column.
3. Replaced error values with `null` (for example, `Total Fat (g)`).
4. Converted `Caffeine (mg)` from text to Decimal Number and replaced the text values "Varies" with `null`.
5. Confirmed the vitamin and mineral columns are stored as percentages.

## DAX measures

```DAX
Total Beverages  = COUNTROWS(starbucks)
Average Calories = AVERAGE(starbucks[Calories])
Avg Sugar        = AVERAGE(starbucks[Sugars (g)])
Average protein  = AVERAGE(starbucks[Protein (g)])
Avg Caffeine     = AVERAGE(starbucks[Caffeine (mg)])
```

**Top 5 caffeine drinks under each cup.** Change `TOPN(1, ...)` to 2, 3, 4 and 5 for the other measures:

```DAX
Top Drink 1 =
VAR T = TOPN(1, VALUES(starbucks[Beverage]), [Avg Caffeine], DESC)
VAR L = TOPN(1, T, [Avg Caffeine], ASC)
RETURN
    MAXX(L, starbucks[Beverage]) & " | " &
    FORMAT(MAXX(L, [Avg Caffeine]), "0") & " mg"
```

These respond to slicers, so the ranking updates when a user changes the protein range or beverage preparation.

## Dashboard contents

| Section | Visual | Fields |
|---|---|---|
| KPI cards | Card x 4 | Total Beverages, Avg Sugar, Average Calories, Average protein |
| Filters | Between slicer, dropdown slicer | `Protein (g)`, `Beverage_prep` |
| Average calories by category | Area chart | `Beverage_category`, Average Calories |
| Calories by beverage | Column chart (Top 15) | `Beverage`, Average Calories |
| Beverage category distribution | Donut chart | `Beverage_category`, Total Beverages |
| Average caffeine by category | Bar chart | `Beverage_category`, Avg Caffeine |
| Top 5 highest caffeine beverages | 5 cards | Top Drink 1 to Top Drink 5 |

## Design approach

- Green, cream and gold palette set through a custom theme (`#00704A`, `#1E3932`, `#CBA258`, `#F7F3EC`).
- Full-page background image holds the panels and artwork. Charts are placed on top with their own background and border turned off.
- Visual positions were set with exact X, Y, width and height values so everything lines up with the background panels.

## Key learnings

- Layering transparent visuals on a designed background image.
- Cleaning mixed text and numeric columns in Power Query.
- Writing ranking logic in DAX with `TOPN`, `VALUES`, `MAXX` and `FORMAT`.
- Controlling cross-filtering and visual interactions.

## How to open

1. Clone or download this repository.
2. Open `Starbucks Dashboard.pbix` in Power BI Desktop.
3. If prompted, point the data source to `data/starbucks.csv` under **Transform data → Data source settings**.

## Repository structure

```
starbucks-dashboard/
├── Starbucks Dashboard.pbix
├── data/
│   └── starbucks.csv
├── images/
│   ├── dashboard.png
│   └── background.png
└── README.md
```

## Author

**Bhargavi Vivek**
Operations and Process Analyst moving into data analytics. Skills: SQL, Python, Power BI, Advanced Excel.

