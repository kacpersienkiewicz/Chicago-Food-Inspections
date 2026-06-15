# Chicago Food Inspections
The City of Chicago offers [Food Inspection Data](https://data.cityofchicago.org/Health-Human-Services/Food-Inspections/4ijn-s7e5/about_data), which helps answer an important question for the food service industry: what are the most common violations and do they vary by ZIP code or business type?

Note the data is from June 13, 2026.

## Key Metrics
* Violation Count
* Violation Count by ZIP Code or Facility Type
* Inspection Failure Rate (by violation or another metric)

## Preparation
There were data quality issues withe dataset including missing values, and inconsistent verbiage for some values (such as facility types). I ended up, coalescing missing values to "Unknown", and using some Python to consolidate the facility categories from hundreds to 16. Since the focus was on violations by ZIP code or facility type, I only had to fix the categories from facility type, and used the UPPER and TRIM functions to standardize the DBA names, facility types, and addresses. Luckily, there were no inconsistencies among inspection IDs (as well as no duplicates), inspection dates, results, risk and ZIP codes.

![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/tree/main/images/data_quality.png?raw=true)

## Inspection Occurrences Oscillate in Three Year Periods, and Dip in the Winter
In 2016, over a thousand inspections occurred, which since 2010, is the maximum amount of inspections. In both 2011, and 2021 inspections dipped to around 500, with the latter being likely due to the COVID-19 Pandemic, and the former being in line with the year before and after. From 2010-2012, There were around 500-600 inspections per year. From 2013 to 2017, there was an increase in inspections per year, which was followed by a decline from 2018 to 2021, when the numbers started increasing.

On a month by month basis, inspections dip during the winter months (December, January, and February) as well as a dip in July. The other months are fairly similar in inspection count. Note that the dips are only a few thousand fewer inspections (~10% fewer), so this may not be as significant as expected.

![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/tree/main/images/inspections_by_month.png?raw=true)
![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/tree/main/images/inspections_by_year.png?raw=true)

## The Data is Skewed Towards Restaurants
This is expected, but the vast majority of violations, inspections and so on occur at restaurants. Grocery stores, bakeries, and various community buildings like churches, daycares and schools each have a decent share of the dataset as well.

## Violations are Mostly the Same across ZIP Codes
I compared the top 10 violations for each ZIP Code which had more than 50 violations, and on average they share 80% of the same violations as the top 10 overall violations, which leads me to believe that the regional differences are fairly minimal. This may be because each ZIP Code is converging towards the same list as restaurants and grocery stores (both of which have the same top 10 violations as the overall) because most inspections would be of those two facility types.

There are differences but it seems that focusing on your specific area is not too important.

## Violations do Vary by Facility Type
I compared the top 10 violations for each facility type, and on average they share ~67% of the same violations as the top 10 overall violations, which is interesting. It's important to note that many of these have far fewer than 25,000 total violations over the course of 15 and a half years, so take the trends with a grain of salt, however they are still valid insights into these businesses. 

The most interesting aspect is that Mobile Food businesses share the fewest violations (25%) with the top 10. Restaurants, bakeries, and grocery stores are typically in line with the top 10 violations, and facilities like healthcare, schools, and gas stations sit around 67% similarity.

An interesting trend is that atypical food service facilities like healthcare facilities, schools and gas stations have a problem with violation 10 (inadequate hand-washing stations) and 18 (not providing a pest control log). Restaurants typically handle both of these issues well.

For Mobile Food, in addition to violation 18 (no pest control log), they should pay attention to violation 2 (no food manager present when food is being served), violation 3 (no on-site health policy), and violation 5 (no bodily fluid spill kit). A lot of this is dependent on being prepare for safety-related concerns.

![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/tree/main/images/violations_by_facility_type.png?raw=true)

## Violations to Prioritize
Overall, violation 55 (facility is not clean), and 34 (poorly kept flooring), 35 (improper thawing methods), 38 (rodents/pests/animals present), 32 (specialized methods used without certification or permission), 33 (improper cooling methods) and 47 (food surfaces improperly maintained) are common and are associated with around a 25 to 40% fail rate. Overall, the most common violations which are somewhat correlated with a failed inspection are about maintenance, cleaning, and proper cooking methodology. A business prioritizing these three aspects are unlikely to face a failed inspection.

There are a few outliers that are much better correlated with failure: 10 (inadequate hand-washing stations), 16 (food surfaces improperly cleaned), and 18 (not providing a pest control log). Violation 18 has a ~95% failure rate correlated with it, while the other two have a 50-60% failure rate correlated with it. 

![image](https://github.com/kacpersienkiewicz/Chicago-Food-Inspections/tree/main/images/top_violations.png?raw=true)

## Conclusion
Based on the prior analysis, regional differences are minimal, but facility differences can be significant. The most important violations to focus on are related to basic infrastructure and safety such as keeping proper logs, and having adequate hand-washing stations. This is especially important for mobile food facilities and non-typical food service locations like schools, as restaurants typically have both of these.

## Dashboard
The Dashboard can be found [here](https://public.tableau.com/app/profile/kacper.sienkiewicz/viz/ChicagoFoodInspection_17814899770390/Overview).

The first pane is an overview of inspections, broken down by month and year as well as breaking down violations by code, colored by failure rate.

The second pane focuses on facility types, showcasing the violations for each type, sorted so the violations with the most occurrences are the closest to the y axis, and the less common violations are further to the right. This also includes the violations colored by fail rate below which can be filtered by facility type.

The final pane showcases a breakdown by ZIP codes and showcases violation count and inspections per year as filtered by ZIP code.
