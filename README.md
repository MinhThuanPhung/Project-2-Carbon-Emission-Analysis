# -Project-2-Carbon-Emission-Analysis
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
## Data structure
### Table product_emissions
```SQL
select *
from product_emissions
limit 10
```
Result

| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 


```sql
select *
from industry_groups
limit 5
```
Result 

| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 


```sql
select *
from companies
limit 5
```
Result 

| id | company_name                  | 
| -: | ----------------------------: | 
| 1  | "Autodesk, Inc."              | 
| 2  | "Casio Computer Co., Ltd."    | 
| 3  | "Cisco Systems, Inc."         | 
| 4  | "CNX Coal Resources, LP"      | 
| 5  | "Coca-Cola Enterprises, Inc." | 


#### Products contribute the most to carbon emissions
```sql
select product_name, round(avg(carbon_footprint_pcf), 2) as avg_carbon_footprint
from product_emissions
Group by product_name
order by avg(carbon_footprint_pcf) DESC
Limit 10
```
Result 
| product_name                                                                                                                       | avg_carbon_footprint | 
| ---------------------------------------------------------------------------------------------------------------------------------: | -------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00           | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00           | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00           | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00           | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00            | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00            | 
| TCDE                                                                                                                               | 99075.00             | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00             | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00             | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00             | 


#### Industry groups of these products
```SQL
select  a.product_name, b.industry_group
from product_emissions a
Join industry_groups b
on a.industry_group_id = b.ID
where product_name in ('Wind Turbine G128 5 Megawats', 'Wind Turbine G132 5 Megawats', 
					   'Wind Turbine G114 2 Megawats', 'Wind Turbine G90 2 Megawats'
					   'Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.',
					   'Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall',
					   'TCDE',
					   'Mercedes-Benz GLE (GLE 500 4MATIC)',
					   'Mercedes-Benz S-Class (S 500)',
					  ' Mercedes-Benz SL (SL 350)' )

```
Result

| product_name                                                                                                                       | industry_group                     | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 
| TCDE                                                                                                                               | Materials                          | 
| TCDE                                                                                                                               | Materials                          | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 


#### The industries with the highest contribution to carbon emissions

```sql
select  b.industry_group, round(avg(carbon_footprint_pcf), 2) as avg_CFP
from product_emissions a
Join industry_groups b
on a.industry_group_id = b.ID
Group by b.industry_group
Order by avg(carbon_footprint_pcf) DESC
Limit 10
```
Result 

| industry_group                                   | avg_CFP   | 
| -----------------------------------------------: | --------: | 
| Electrical Equipment and Machinery               | 891050.73 | 
| Automobiles & Components                         | 35373.48  | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 24162.00  | 
| Capital Goods                                    | 7391.77   | 
| Materials                                        | 3208.86   | 
| "Mining - Iron, Aluminum, Other Metals"          | 2727.00   | 
| Energy                                           | 2154.80   | 
| Chemicals                                        | 1949.03   | 
| Media                                            | 1534.47   | 
| Software & Services                              | 1368.94   | 


#### The companies with the highest contribution to carbon emissions

```sql
select  c.company_name, round(avg(carbon_footprint_pcf),2) as avg_CFP
from product_emissions a
Join companies c
on a.company_id = c.ID
Group by c.company_name
Order by avg(carbon_footprint_pcf) DESC
Limit 10 
```

Result

| company_name                           | avg_CFP    | 
| -------------------------------------: | ---------: | 
| "Gamesa Corporación Tecnológica, S.A." | 2444616.00 | 
| "Hino Motors, Ltd."                    | 191687.00  | 
| Arcelor Mittal                         | 83503.50   | 
| Weg S/A                                | 53551.67   | 
| Daimler AG                             | 43089.19   | 
| General Motors Company                 | 34251.75   | 
| Volkswagen AG                          | 26238.40   | 
| Waters Corporation                     | 24162.00   | 
| "Daikin Industries, Ltd."              | 17600.00   | 
| CJ Cheiljedang                         | 15802.83   | 

#### The countries with the highest contribution to carbon emissions

```sql
select  d.country_name, round(avg(carbon_footprint_pcf), 2) as avg_CFP 
from product_emissions a
Join countries d
on a.country_id = d.ID
Group by d.country_name
Order by avg(carbon_footprint_pcf) DESC
Limit 10
```
Result

| country_name | avg_CFP   | 
| -----------: | --------: | 
| Spain        | 699009.29 | 
| Luxembourg   | 83503.50  | 
| Germany      | 33600.37  | 
| Brazil       | 9407.61   | 
| South Korea  | 5665.61   | 
| Japan        | 4600.26   | 
| Netherlands  | 2011.91   | 
| India        | 1535.88   | 
| USA          | 1332.60   | 
| South Africa | 1119.27   | 

#### The trend of carbon footprints (PCFs) over the years

```sql
select Year, round(avg(carbon_footprint_pcf),2) as Avg_CFP
from product_emissions
group by year
```
Result

| Year | Avg_CFP  | 
| ---: | -------: | 
| 2013 | 2399.32  | 
| 2014 | 2457.58  | 
| 2015 | 43188.90 | 
| 2016 | 6891.52  | 
| 2017 | 4050.85  |  

#### Industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
```SQL
select Year, industry_group, round(avg(carbon_footprint_pcf),2) as Avg_CFP
from product_emissions a
join industry_groups b
on a.industry_group_id = b.id
group by year, industry_group
Order by industry_group, year , avg(carbon_footprint_pcf)
limit 15
```

```SQL
select  industry_group as 'Industry_group', 
round(avg(case when year= 2013 then carbon_footprint_pcf else 0 end) ,2) as  '2013 Emission', 
round(avg(case when year= 2014 then carbon_footprint_pcf else 0 end) ,2) as  '2014 Emission',  
round(avg(case when year= 2015 then carbon_footprint_pcf else 0 end) ,2) as '2015 Emission',
round(avg(case when year= 2016 then carbon_footprint_pcf else 0 end) ,2) as  '2016 Emission',
round(avg(case when year= 2017 then carbon_footprint_pcf else 0 end) ,2) as '2017 Emission'
from product_emissions a
join industry_groups b
on a.industry_group_id = b.id
group by industry_group
Order by industry_group
limit 15

```

| Industry_group                                                         | 2013 Emission | 2014 Emission | 2015 Emission | 2016 Emission | 2017 Emission | 
| ---------------------------------------------------------------------: | ------------: | ------------: | ------------: | ------------: | ------------: | 
| "Consumer Durables, Household and Personal Products"                   | 0.00          | 0.00          | 116.38        | 0.00          | 0.00          | 
| "Food, Beverage & Tobacco"                                             | 39.02         | 20.98         | 0.00          | 783.51        | 24.70         | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 0.00          | 0.00          | 685.31        | 0.00          | 0.00          | 
| "Mining - Iron, Aluminum, Other Metals"                                | 0.00          | 0.00          | 2727.00       | 0.00          | 0.00          | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 10757.00      | 13405.00      | 0.00          | 0.00          | 0.00          | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 0.00          | 0.00          | 14.33         | 0.00          | 0.00          | 
| Automobiles & Components                                               | 1783.41       | 3150.89       | 11194.89      | 19244.29      | 0.00          | 
| Capital Goods                                                          | 1719.71       | 2677.11       | 100.14        | 181.97        | 2712.83       | 
| Chemicals                                                              | 0.00          | 0.00          | 1949.03       | 0.00          | 0.00          | 
| Commercial & Professional Services                                     | 26.30         | 10.84         | 0.00          | 65.68         | 16.84         | 
| Consumer Durables & Apparel                                            | 42.16         | 48.24         | 0.00          | 17.09         | 0.00          | 
| Containers & Packaging                                                 | 0.00          | 0.00          | 373.50        | 0.00          | 0.00          | 
| Electrical Equipment and Machinery                                     | 0.00          | 0.00          | 891050.73     | 0.00          | 0.00          | 
| Energy                                                                 | 150.00        | 0.00          | 0.00          | 2004.80       | 0.00          | 
| Food & Beverage Processing                                             | 0.00          | 0.00          | 7.05          | 0.00          | 0.00          | 

