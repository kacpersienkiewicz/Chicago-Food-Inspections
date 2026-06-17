# Chicago Food Inspections
The City of Chicago offers [Food Inspection Data](https://data.cityofchicago.org/Health-Human-Services/Food-Inspections/4ijn-s7e5/about_data), which helps answer an important question for the food service industry: what are the most common violations and do they vary by ZIP code or business type?

Note the data is from June 13, 2026.

## Dataset Information
* ~300,000 inspection records
* ~1,000,000 violation records after normalization
* January 2010 to June 2026

## Project Overview

This project analyzes over 15 years of Chicago food inspection data to identify the violations most associated with failed inspections and determine whether violation patterns vary by location or facility type.

The analysis was performed using:

* Excel (data quality profiling)
* SQLite (data storage, querying, and cleaning)
* Python/Pandas (cleaning and analysis)
* Tableau (interactive dashboarding)

## Technical Highlights

* Standardized hundreds of facility-type values into 16 business categories
* Normalized denormalized inspection records into separate inspection and violation tables to support relational analysis
* Built SQL queries for SQLite
* Calculated violation-level failure rates
* Compared violation distributions across ZIP codes and facility types
* Developed an interactive Tableau dashboard with cross-filtering and geographic analysis

### Database Schema

Inspections
------------
inspection_id (PK)
dba_name
facility_type
address
zip
inspection_date
inspection_type
results
risk

Violations
------------
violation_id (PK)
inspection_id (FK)
violation_code
violation_description

The original dataset contained a license ID that appeared to represent facilities. However, because the identifier was not consistently unique and included placeholder values (such as 0), a separate facilities table couldn't be made without introducing data integrity issues.

## Key Metrics
* Violation Count
* Violation Count by ZIP Code or Facility Type
* Inspection Failure Rate (by violation or another metric)

## Preparation
There were data quality issues with the dataset including missing values, and inconsistent formatting for some values (such as facility types). I ended up coalescing missing values to "Unknown", and using some Python to consolidate the facility categories from hundreds to 16. Since the focus was on violations by ZIP code or facility type, I only had to fix the categories from facility type, and used the UPPER and TRIM functions to standardize the DBA names, facility types, and addresses. Luckily, there were no inconsistencies among inspection IDs (as well as no duplicates), inspection dates, results, risk and ZIP codes.

![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/blob/main/images/data_quality.png?raw=true)

## Inspection Volume Varies with Time and Dips in the Winter
The volume of inspections per year vary with time, but monthly volume remains steady aside from modest decreases in July and the winter months. Inspection volume increased between 2013 and 2017, before declining to a low in 2021 and picking up again.

![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/blob/main/images/inspections_by_month.png?raw=true)
![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/blob/main/images/inspections_by_year.png?raw=true)

## The Data is Skewed Towards Restaurants
This is expected, but the vast majority of violations, inspections and so on occur at restaurants. Grocery stores, bakeries, and various community buildings like churches, daycares and schools each have a decent share of the dataset as well.

## Violations are Mostly the Same across ZIP Codes
Similarity was measured as the percentage overlap between a ZIP code's top 10 violations and the overall top 10 violations. As a note, only ZIP codes with at least 50 inspections were included in the comparison. On average ZIP codes share 80% of the same violations as the top 10 overall violations, which leads me to believe that the regional differences are fairly minimal. This may be because each ZIP Code is converging towards the same list as restaurants and grocery stores (both of which have the same top 10 violations as the overall) because most inspections would be of those two facility types.

There are differences but facility type is a stronger predictor or violation patterns than the business's ZIP code.

## Violations do Vary by Facility Type
I compared the top 10 violations overall with the top 10 violations for each facility type and calculated the percentage of overlap. With this metric, restaurants, bakeries and grocery stores are typically in-line with the top 10 violations while facilities like healthcare, schools, and gas stations sit around 67% similarity. The most interesting aspect is that Mobile Food businesses share the fewest violations (25%) with the top 10.

Atypical food service facilities like healthcare facilities, schools and gas stations have a problem with violation 10 (inadequate hand-washing stations) and 18 (not providing a pest control log). Restaurants typically handle both of these issues well.

For Mobile Food, in addition to violation 18 (no pest control log), they should pay attention to violation 2 (no food manager present when food is being served), violation 3 (no on-site health policy), and violation 5 (no bodily fluid spill kit). A lot of this is dependent on being prepared for safety-related concerns.

![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/blob/main/images/violations_by_facility_type.png?raw=true)

## Highest-Risk Violations
Several high-frequency violations are also strongly associated with a failed inspection (25-40%) making them important to focus on:

* Violation 55  - facility is not clean
* Violation 34  - poorly kept flooring
* Violation 35  - improper thawing methods
* Violation 38  - rodents/pests/animals present
* Violation 32  - specialized methods used without certification or permission
* Violation 33  - improper cooling methods
* Violation 47  - food surfaces improperly maintained 

There are a few outliers that have a higher association with failure: 10 (inadequate hand-washing stations), 16 (food surfaces improperly cleaned), and 18 (not providing a pest control log). Approximately 95% of inspections with a violation 18 recorded resulted in a failed inspection, while the other two appear in failed inspections around 50-60% of the time.

![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/blob/main/images/top_violations.png?raw=true)

## Recommendations
Based on the prior analysis, regional differences are minimal, but facility differences can be significant. The most important violations to focus on are related to basic infrastructure and safety such as keeping proper logs, and having adequate hand-washing stations. This is especially important for mobile food facilities and non-typical food service locations like schools, as restaurants typically have both of these.

What this means in practice:
* Maintain functional hand-washing stations
* Keep required pest-control logs around and have food safety policies in place
* Properly clean surfaces, especially those that touch both food and non-food items.
* Maintain cleanliness in the facility
* Follow approved methods for food handling, cooling and thawing.

### Limitations
* Facility types were consolidated using keyword-based classification so there may be misclassifications.
* Causation is not established by this analysis.
* Many of the smaller facility categories have substantially fewer inspections which may make their top 10 violations non-representative.

## Dashboard
The Dashboard can be found [here](https://public.tableau.com/app/profile/kacper.sienkiewicz/viz/ChicagoFoodInspection_17814899770390/Overview).

The first pane is an overview of inspections, broken down by month and year as well as breaking down violations by code, colored by failure rate.

The second pane focuses on facility types, showcasing the violations for each type, sorted so the violations with the most occurrences are the closest to the y axis, and the less common violations are further to the right. This also includes the violations colored by fail rate below which can be filtered by facility type.

The final pane showcases a breakdown by ZIP codes and showcases violation count and inspections per year as filtered by ZIP code.

## Key Takeaways
* Violation patterns are largely consistent across Chicago ZIP codes.
* Facility type is a stronger predictor for violation patterns than geographic location.
* Mobile food facilities have very different violation profiles than standard restaurants.
* Businesses can focus on a small number of violations to improve inspection outcomes: maintaining proper handwashing stations, clean food-contact surfaces, keep pest control logs, maintain overall cleanliness, and follow proper food handling, cooling and thawing procedures.
