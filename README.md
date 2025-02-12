# Project-2-Carbon-Emission-Analysis
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.
![carbon-footprint-feature](https://github.com/user-attachments/assets/82dcf4b7-9e65-4aaa-89cd-e23290e17261)


## Data Source: Where Our Data Comes From
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
![Database diagram](https://github.com/user-attachments/assets/22430a68-dde2-4b1f-80e7-642e306beada)


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

### Analysis

#### 1. Products contribute the most to carbon emissions
```sql
select product_name, round(avg(carbon_footprint_pcf), 2) as avg_CFP
from product_emissions
Group by product_name
order by avg(carbon_footprint_pcf) DESC
Limit 10
```
Result 
| product_name                                                                                                                       | avg_CFP    | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00 | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00 | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00 | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00  | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00  | 
| TCDE                                                                                                                               | 99075.00   | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00   | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00   | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00   | 


From table above we see. 

Wind Turbine contributes the highest carbon emissions, followed by  Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace then Sheet pile, TCDE and finally by 3 series of Mercedes-Benz. The highest one is Wind Turbine G128 5 Megawats which contributes carbon emissions 50 times as much as Mercedes-Benz SL (SL 350)  as the last one in Top 10 the  products contribute the most to carbon emission.



#### 2. Industry groups of these products

```sql
select product_name, b.industry_group, round(avg(carbon_footprint_pcf), 2) as avg_CFP
from product_emissions a
Join industry_groups b
on a.industry_group_id = b.ID
Group by product_name
order by avg(carbon_footprint_pcf) DESC
Limit 10
```
Or can use this code, It shows industry group and avg_pfp
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

| product_name                                                                                                                       | industry_group                     | avg_CFP    | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | ---------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00 | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00 | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00 | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00  | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00  | 
| TCDE                                                                                                                               | Materials                          | 99075.00   | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00   | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00   | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00   | 

From table above we see. 

Top 3 industries have products contribute the most carbon emission are Electrical Equipment and Machinery, Materials, Automobiles & Components. In thí top 3 induties, 80% carbon emission comes froms Electrical Equipment and Machinery. 

#### 3. The industries with the highest contribution to carbon emissions

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

From table above we see. 

Top 3 industries with the highest contribution to carbon emissions are  Electrical Equipment and Machinery, Automobiles & Components  and "Pharmaceuticals, Biotechnology & Life Sciences. Only Electrical Equipment and Machinery contributes 891050.73 Cacbon emission which are more than 90% in total carbon emssion contributed by Top 10 Indutries

#### 4. The companies with the highest contribution to carbon emissions

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


From table above we see. 

Top 10 companies with the highest contribution to carbon emissions include Gamesa Corporación Tecnológica S.A, Hino Motors Ltd, Arcelor Mittal, Arcelor Mittal,  Weg S/A, Daimler AG, General Motors Company, Volkswagen AG, Waters Corporation, Daikin Industries Ltd and CJ Cheiljedang. Gamesa Corporación Tecnológica, S.A contributes 2444616 carbon emission which is company with the highest contribution.


#### 5. The countries with the highest contribution to carbon emissions

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

From table above we see. 

Spain, Luxembourg and Germany are top 3 countries  with the highest contribution to carbon emissions. Spain contributed 699009.29 which is more than 80% compared with total contribution carbon emission of top 10 countries.

#### 6. The trend of carbon footprints (PCFs) over the years

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

```SQL

select product_name, company_name, industry_group, country_name, 
round(sum(carbon_footprint_pcf),2) as sum_CFP
from product_emissions as T1
Join companies as T2 ON T1.company_id = T2.id
Join countries as T3 ON T1.country_id = T3.id
Join industry_groups as T4 ON T1.industry_group_id = T4.id
Where year = 2015
group by 1, 2, 3, 4
Order by 5 desc
Limit 15

| product_name                                                                                     | company_name                           | industry_group                     | country_name | sum_CFP    | 
| -----------------------------------------------------------------------------------------------: | -------------------------------------: | ---------------------------------: | -----------: | ---------: | 
| Wind Turbine G128 5 Megawats                                                                     | "Gamesa Corporación Tecnológica, S.A." | Electrical Equipment and Machinery | Spain        | 3718044.00 | 
| Wind Turbine G132 5 Megawats                                                                     | "Gamesa Corporación Tecnológica, S.A." | Electrical Equipment and Machinery | Spain        | 3276187.00 | 
| Wind Turbine G114 2 Megawats                                                                     | "Gamesa Corporación Tecnológica, S.A." | Electrical Equipment and Machinery | Spain        | 1532608.00 | 
| Wind Turbine G90 2 Megawats                                                                      | "Gamesa Corporación Tecnológica, S.A." | Electrical Equipment and Machinery | Spain        | 1251625.00 | 
| Mercedes-Benz SL-Class                                                                           | Daimler AG                             | Automobiles & Components           | Germany      | 69000.00   | 
| Mercedes-Benz CLS-Class                                                                          | Daimler AG                             | Automobiles & Components           | Germany      | 57100.00   | 
| Mercedes-Benz S-Class                                                                            | Daimler AG                             | Automobiles & Components           | Germany      | 54000.00   | 
| Mercedes-Benz C-Class                                                                            | Daimler AG                             | Automobiles & Components           | Germany      | 50500.00   | 
| Mercedes-Benz GLK-Class                                                                          | Daimler AG                             | Automobiles & Components           | Germany      | 48800.00   | 
| Mercedes-Benz E-Class Saloon                                                                     | Daimler AG                             | Automobiles & Components           | Germany      | 47200.00   | 
| Mercedes-Benz M-Class - Passenger Car                                                            | Daimler AG                             | Automobiles & Components           | Germany      | 43600.00   | 
| Mercedes-Benz E-Class BlueTEC Hybrid                                                             | Daimler AG                             | Automobiles & Components           | Germany      | 40600.00   | 
| Average of all GM vehicles produced and used in the 10 year life-cycle.                          | General Motors Company                 | Automobiles & Components           | USA          | 39100.00   | 
| Audi A6                                                                                          | Volkswagen AG                          | Automobiles & Components           | Germany      | 37094.00   | 
| Mean value for a vehicle within the Volkswagen Group according to CDP Report 2014 (global scope) | Volkswagen AG                          | Automobiles & Components           | Germany      | 33683.00   | 

From table above we see. 

Carbon emission increased over the year from 2013 to 2017. It was dramatically  in 2015 which 18 times as much as carbon emission in 2013 then reduced sinificantly in 2016 then reduced gradually in 2017 at 4050.
In 2015, the reason of sharp increase is the big investment in wind turbin in Spain.

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
Or use code below 

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


#### Details of Top 10 Products contribute  the most carbon emission 
```SQL
select product_name, b.industry_group, country_name, company_name, round(avg(carbon_footprint_pcf), 2) as avg_CFP
from product_emissions a
Join industry_groups b
on a.industry_group_id = b.ID
Join companies c
On a.company_id=c.id
Join countries d
On a.country_id= d.id
Group by product_name
order by avg(carbon_footprint_pcf) DESC
Limit 10

```

Result
| product_name                                                                                                                       | industry_group                     | country_name | company_name                            | avg_CFP    | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | -----------: | --------------------------------------: | ---------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | Spain        | "Gamesa Corporación Tecnológica, S.A."  | 3718044.00 | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | Spain        | "Gamesa Corporación Tecnológica, S.A."  | 3276187.00 | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | Spain        | "Gamesa Corporación Tecnológica, S.A."  | 1532608.00 | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | Spain        | "Gamesa Corporación Tecnológica, S.A."  | 1251625.00 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | Japan        | "Hino Motors, Ltd."                     | 191687.00  | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | Luxembourg   | Arcelor Mittal                          | 167000.00  | 
| TCDE                                                                                                                               | Materials                          | Japan        | "Mitsubishi Gas Chemical Company, Inc." | 99075.00   | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | Germany      | Daimler AG                              | 91000.00   | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | Germany      | Daimler AG                              | 85000.00   | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | Germany      | Daimler AG                              | 72000.00   | 


#### 7.Contribution of each industries in Top 5 countries over the years


```SQL
select year, product_name, b.industry_group, country_name, round(avg(carbon_footprint_pcf), 2) as avg_CFP
from product_emissions a
Join industry_groups b
on a.industry_group_id = b.ID
Join countries d
On a.country_id= d.id
where country_name in ('Spain' , 'Germany', 'Brazil', 'South Korea', 'Luxembourg ')
Group by industry_group, year
order by country_name, industry_group
```

Result
| year | product_name                                                                                                                | industry_group                            | country_name | avg_CFP    | 
| ---: | --------------------------------------------------------------------------------------------------------------------------: | ----------------------------------------: | -----------: | ---------: | 
| 2013 | Electric Motor                                                                                                              | Capital Goods                             | Brazil       | 53058.00   | 
| 2014 | Electric Motor                                                                                                              | Capital Goods                             | Brazil       | 87589.00   | 
| 2015 | Polyethylene                                                                                                                | Chemicals                                 | Brazil       | 3504.40    | 
| 2015 | Electric Motor                                                                                                              | Electrical Equipment and Machinery        | Brazil       | 1959694.40 | 
| 2013 | "Ethanol. In addition to its use as fuel (in vehicles), ethanol is an input to the food, chemical and cosmetic industries." | Energy                                    | Brazil       | 750.00     | 
| 2013 | grinding media cast.                                                                                                        | Materials                                 | Brazil       | 19656.11   | 
| 2014 | Polyethylene                                                                                                                | Materials                                 | Brazil       | 2560.29    | 
| 2016 | Polyethylene                                                                                                                | Materials                                 | Brazil       | 2665.71    | 
| 2016 | Nicotine                                                                                                                    | "Food, Beverage & Tobacco"                | Germany      | 8652.27    | 
| 2014 | Peppermint tea infusion                                                                                                     | "Food, Beverage & Tobacco"                | Germany      | 0.00       | 
| 2015 | Peppermint tea infusion                                                                                                     | "Food, Beverage & Tobacco"                | Germany      | 0.00       | 
| 2013 | VW Polo V 1.6 TDI BlueMotion Technology                                                                                     | Automobiles & Components                  | Germany      | 25650.25   | 
| 2014 | VW Polo                                                                                                                     | Automobiles & Components                  | Germany      | 27691.14   | 
| 2015 | Volkswagen Polo                                                                                                             | Automobiles & Components                  | Germany      | 37053.67   | 
| 2016 | Volkswagen Polo                                                                                                             | Automobiles & Components                  | Germany      | 36770.06   | 
| 2013 | LG-E970                                                                                                                     | Consumer Durables & Apparel               | South Korea  | 7.00       | 
| 2014 | Air Purifier                                                                                                                | Consumer Durables & Apparel               | South Korea  | 126.00     | 
| 2015 | 4Gb LPDDR2 SDRAM                                                                                                            | Semiconductors & Semiconductors Equipment | South Korea  | 1.00       | 
| 2015 | H000 (Set-top Box)                                                                                                          | Technology Hardware & Equipment           | South Korea  | 79.00      | 
| 2016 | Lineal Alkyl Bencene (LAB)                                                                                                  | Energy                                    | Spain        | 3499.50    | 

```sql

select 
	t2.industry_group, 
	sum(case when year= 2013 then carbon_footprint_pcf end) as total_cfp_2013 , 
	sum(case when year= 2017 then carbon_footprint_pcf end) as total_cfp_2017,
	sum(case when year= 2013 then carbon_footprint_pcf end) - sum(case when year= 2017 then carbon_footprint_pcf end) as difference
from product_emissions as t1
	Join industry_groups as t2 on t2.id= t1.industry_group_id
Group by industry_group
order by 3 desc

Result

| industry_group                                                         | total_cfp_2013 | total_cfp_2017 | difference | 
| ---------------------------------------------------------------------: | -------------: | -------------: | ---------: | 
| Materials                                                              | 200513         | 213137         | -12624     | 
| Capital Goods                                                          | 60190          | 94949          | -34759     | 
| Technology Hardware & Equipment                                        | 61100          | 27592          | 33508      | 
| "Food, Beverage & Tobacco"                                             | 4995           | 3162           | 1833       | 
| Commercial & Professional Services                                     | 1157           | 741            | 416        | 
| Software & Services                                                    | 6              | 690            | -684       | 
| Tires                                                                  | [NULL]         | [NULL]         | [NULL]     | 
| Media                                                                  | 9645           | [NULL]         | [NULL]     | 
| Electrical Equipment and Machinery                                     | [NULL]         | [NULL]         | [NULL]     | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | [NULL]         | [NULL]         | [NULL]     | 
| Gas Utilities                                                          | [NULL]         | [NULL]         | [NULL]     | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | [NULL]         | [NULL]         | [NULL]     | 
| Tobacco                                                                | [NULL]         | [NULL]         | [NULL]     | 
| Retailing                                                              | [NULL]         | [NULL]         | [NULL]     | 
| Energy                                                                 | 750            | [NULL]         | [NULL]     | 
| Automobiles & Components                                               | 130189         | [NULL]         | [NULL]     | 
| Household & Personal Products                                          | 0              | [NULL]         | [NULL]     | 
| Consumer Durables & Apparel                                            | 2867           | [NULL]         | [NULL]     | 
| "Mining - Iron, Aluminum, Other Metals"                                | [NULL]         | [NULL]         | [NULL]     | 
| Trading Companies & Distributors and Commercial Services & Supplies    | [NULL]         | [NULL]         | [NULL]     | 
| Semiconductors & Semiconductor Equipment                               | [NULL]         | [NULL]         | [NULL]     | 
| Food & Beverage Processing                                             | [NULL]         | [NULL]         | [NULL]     | 
| "Consumer Durables, Household and Personal Products"                   | [NULL]         | [NULL]         | [NULL]     | 
| Telecommunication Services                                             | 52             | [NULL]         | [NULL]     | 
| Containers & Packaging                                                 | [NULL]         | [NULL]         | [NULL]     | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 32271          | [NULL]         | [NULL]     | 
| Utilities                                                              | 122            | [NULL]         | [NULL]     | 
| Semiconductors & Semiconductors Equipment                              | [NULL]         | [NULL]         | [NULL]     | 
| Food & Staples Retailing                                               | [NULL]         | [NULL]         | [NULL]     | 
| Chemicals                                                              | [NULL]         | [NULL]         | [NULL]     | 

### Insights

- The products with the highest levels of carbon emissions are typically associated with heavy industry.
- The following car models are leading in carbon emissions during production: Land Cruiser Prado, Mercedes-Benz GLA, Mercedes-Benz S-Class, and Mercedes-Benz SL
- Spain , Germany, Brazil, South Korea are  are dominant players in heavy industry so these countries contributed the most cacbon emission.
- Germany reduced carbon emission in food insdustry but increase amount of carbon emission in heavy industry when they expanded supply chain and targeted to export globally 


