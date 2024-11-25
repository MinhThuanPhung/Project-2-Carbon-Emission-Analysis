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
select  b.industry_group, avg(carbon_footprint_pcf) as avg_CFP
from product_emissions a
Join industry_groups b
on a.industry_group_id = b.ID
Group by b.industry_group
Order by avg(carbon_footprint_pcf) DESC
Limit 10
```
Result 

| industry_group                                   | avg_CFP     | 
| -----------------------------------------------: | ----------: | 
| Electrical Equipment and Machinery               | 891050.7273 | 
| Automobiles & Components                         | 35373.4795  | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 24162.0000  | 
| Capital Goods                                    | 7391.7714   | 
| Materials                                        | 3208.8611   | 
| "Mining - Iron, Aluminum, Other Metals"          | 2727.0000   | 
| Energy                                           | 2154.8000   | 
| Chemicals                                        | 1949.0313   | 
| Media                                            | 1534.4667   | 
| Software & Services                              | 1368.9412   | 


#### The companies with the highest contribution to carbon emissions

```sql
select  c.company_name, avg(carbon_footprint_pcf) as avg_CFP
from product_emissions a
Join companies c
on a.company_id = c.ID
Group by c.company_name
Order by avg(carbon_footprint_pcf) DESC
Limit 10
```

Result

| company_name                           | avg_CFP      | 
| -------------------------------------: | -----------: | 
| "Gamesa Corporación Tecnológica, S.A." | 2444616.0000 | 
| "Hino Motors, Ltd."                    | 191687.0000  | 
| Arcelor Mittal                         | 83503.5000   | 
| Weg S/A                                | 53551.6667   | 
| Daimler AG                             | 43089.1892   | 
| General Motors Company                 | 34251.7500   | 
| Volkswagen AG                          | 26238.4000   | 
| Waters Corporation                     | 24162.0000   | 
| "Daikin Industries, Ltd."              | 17600.0000   | 
| CJ Cheiljedang                         | 15802.8333   | 
