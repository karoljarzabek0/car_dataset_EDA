# car_dataset_EDA
# Exploratory Data Analysis of car offers from a large Polish marketplace
This repo contains exploratory data analysis augmented with machine learning performed on over 200 000 car offers from a Polish online marketplace.
The dataset was scraped by me in March 2025.

**QUERY:**
```
SELECT DISTINCT COUNT(*) AS total_offers
FROM car_dataset
```

**RESULT:**
|    |   total_offers |
|---:|---------------:|
|  0 |         219808 |

## Structure of the dataset
**QUERY:**

```
SELECT *
FROM car_dataset
LIMIT 5
```

**RESULT:**

|    |         id | title                               |   price | city      | region        |   n_parameters | make       | model   | version                    |   year |   damaged | is_imported   | country_origin   |   mileage |   engine_capacity |   engine_power | fuel_type   | gearbox   | transmission        | urban_consumption   | extra_urban_consumption   | body_type   |   nr_seats | color   |   rhd |   registered |   no_accident |
|---:|-----------:|:------------------------------------|--------:|:----------|:--------------|---------------:|:-----------|:--------|:---------------------------|-------:|----------:|:--------------|:-----------------|----------:|------------------:|---------------:|:------------|:----------|:--------------------|:--------------------|:--------------------------|:------------|-----------:|:--------|------:|-------------:|--------------:|
|  0 | 1840114214 | Nissan Juke                         |   36000 | Kraków    | Małopolskie   |             50 | nissan     | juke    |                            |   2015 |         0 |               | pl               |    114043 |              1197 |            116 | petrol      | manual    | front-wheel         |                     |                           | suv         |          5 | black   |   nan |            1 |             1 |
|  1 | 1840114192 | Volkswagen Touareg                  |   65900 | Suchy Las | Wielkopolskie |             93 | volkswagen | touareg |                            |   2016 |         0 |               | pl               |    388900 |              2967 |            262 | diesel      | automatic | all-wheel-permanent |                     |                           | suv         |          5 | white   |     0 |            1 |             1 |
|  2 | 1840114189 | Opel Corsa                          |   37000 | Kraków    | Małopolskie   |             47 | opel       | corsa   |                            |   2016 |         0 |               | pl               |     46086 |              1398 |             90 | petrol      | manual    | front-wheel         |                     |                           | compact     |          5 | grey    |   nan |            1 |             0 |
|  3 | 1840114185 | Skoda Octavia 2.0 TDI SCR Style DSG |   94900 | Poznań    | Wielkopolskie |            102 | skoda      | octavia | ver-2-0-tdi-scr-style-dsg  |   2021 |         0 |               | d                |    107679 |              1968 |            150 | diesel      | automatic | front-wheel         |                     |                           | combi       |          5 | white   |   nan |            1 |             1 |
|  4 | 1840114104 | Ford Mondeo 2.0 EcoBlue ST-Line X   |   85990 | Sosnowiec | Śląskie       |            107 | ford       | mondeo  | ver-2-0-ecoblue-st--line-x |   2021 |         0 |               |                  |    140000 |              1997 |            150 | diesel      | manual    | front-wheel         |                     |                           | combi       |          5 | black   |   nan |            1 |             1 |


The dataset containes fairly standard parametrs of a car. Apart from that


Columns:
```
id, title, price, city, region, year, country_origin, fuel_type and gearbox
```
are self-explanatory.

Make is the manufacturer of a car.

# Key insights

- Podlaskie voievodiship despite keeping ~2% of market share has the 5th highest prices.


## Top manufacturers (market share)

| make          | volume_market_share |   | make          | value_market_share |
|:-------------|--------------------:|---|:-------------|-------------------:|
| bmw          | 8.27%               |   | bmw          | 12.6%             |
| volkswagen   | 8.11%               |   | mercedes-benz| 12.17%            |
| audi         | 7.95%               |   | audi         | 10.85%            |
| ford         | 7.3%                |   | volkswagen   | 7.03%             |
| opel         | 6.82%               |   | toyota       | 5.09%             |



### Gearbox configuration

| gearbox   | avg_price   | avg_year   | avg_mileage   |   volume_market_share |   value_market_share |
|:----------|:------------|:-----------|:--------------|----------------------:|---------------------:|
| automatic | 132 732     | 2 018      | 107 709       |                    50 |                   78 |
| manual    | 38 277      | 2 013      | 166 899       |                    50 |                   22 |

Other singular offers also included hydrogen, petrol-cng and etanol; 78 offers combined.
### Fuel type

| fuel_type     | avg_mileage   |   volume_market_share |   value_market_share |
|:--------------|:--------------|----------------------:|---------------------:|
| petrol        | 111 487       |                    50% |                   49% |
| diesel        | 185 426       |                    38% |                   33% |
| hybrid        | 54 407        |                     6% |                    9% |
| plugin-hybrid | 52 982        |                     1% |                    4% |
| electric      | 28 056        |                     2% |                    3% |
| petrol-lpg    | 202 731       |                     4% |                    1% |

## Geographic distribution

### Region market share

**QUERY:**

```
SELECT *, ROUND(nr_offers * avg_price,0) AS market_cap ,ROUND(1.0 * nr_offers / SUM(nr_offers) OVER() * 100, 2) as percentage
FROM (
SELECT region, COUNT(*) AS nr_offers, ROUND(AVG(price),0) as avg_price, ROUND(AVG(year),0) as avg_year, ROUND(AVG(mileage),0) as avg_mileage
FROM car_dataset 
GROUP BY region
) t
ORDER BY market_cap DESC 
LIMIT 16;
```

**RESULT:**
| region              | nr_offers   | avg_price   | avg_year   | avg_mileage   | market_cap    |   percentage |
|:--------------------|:------------|:------------|:-----------|:--------------|:--------------|-------------:|
| Mazowieckie         | 44 103      | 99 413      | 2 016      | 130 212       | 4 384 411 539 |           20 |
| Śląskie             | 27 425      | 93 174      | 2 017      | 122 360       | 2 555 296 950 |           12 |
| Wielkopolskie       | 29 003      | 79 357      | 2 016      | 131 945       | 2 301 591 071 |           13 |
| Małopolskie         | 19 305      | 81 484      | 2 016      | 138 653       | 1 573 048 620 |            9 |
| Dolnośląskie        | 16 554      | 78 963      | 2 015      | 147 006       | 1 307 153 502 |            8 |
| Pomorskie           | 13 641      | 92 782      | 2 016      | 141 041       | 1 265 639 262 |            6 |
| Łódzkie             | 12 633      | 88 226      | 2 016      | 131 005       | 1 114 559 058 |            6 |
| Kujawsko-pomorskie  | 9 589       | 82 879      | 2 016      | 142 036       | 794 726 731   |            4 |
| Zachodniopomorskie  | 7 735       | 83 412      | 2 015      | 149 569       | 645 191 820   |            4 |
| Podkarpackie        | 7 395       | 75 296      | 2 015      | 141 195       | 556 813 920   |            3 |
| Lubelskie           | 7 580       | 65 031      | 2 014      | 166 425       | 492 934 980   |            3 |
| Świętokrzyskie      | 6 323       | 68 402      | 2 015      | 155 889       | 432 505 846   |            3 |
| Lubuskie            | 5 447       | 71 214      | 2 015      | 152 921       | 387 902 658   |            2 |
| Podlaskie           | 4 385       | 86 268      | 2 016      | 137 414       | 378 285 180   |            2 |
| Warmińsko-mazurskie | 4 825       | 71 256      | 2 015      | 154 980       | 343 810 200   |            2 |
| Opolskie            | 3 708       | 73 028      | 2 016      | 145 097       | 270 787 824   |            2 |

### TOP 5 cities BY market share

| region        | city     |   nr_offers |   avg_price |   avg_year |   avg_mileage |   volume_market_share |   value_market_share |
|:--------------|:---------|------------:|------------:|-----------:|--------------:|----------------------:|---------------------:|
| Mazowieckie   | Warszawa |       17801 |      134555 |       2018 |         97731 |                  8.1  |                12.73 |
| Małopolskie   | Kraków   |        5864 |      110133 |       2017 |        119097 |                  2.67 |                 3.43 |
| Wielkopolskie | Poznań   |        5415 |      127208 |       2018 |        101571 |                  2.46 |                 3.66 |
| Dolnośląskie  | Wrocław  |        5109 |      105448 |       2017 |        124305 |                  2.32 |                 2.86 |
| Łódzkie       | Łódź     |        4578 |      136420 |       2019 |         85869 |                  2.08 |                 3.32 |
**QUERY:**

```
SELECT *, ROUND(1.0 * nr_offers / SUM(nr_offers) OVER() * 100, 2) as percentage
FROM (
SELECT make, COUNT(*) AS nr_offers, ROUND(AVG(price),0) as avg_price, ROUND(AVG(year),0) as avg_year, ROUND(AVG(mileage),0) as avg_mileage
FROM car_dataset 
GROUP BY make
) t
ORDER BY nr_offers DESC 
LIMIT 20;
```

**RESULT:**

|    | make          |   nr_offers |   avg_price |   avg_year |   avg_mileage |   percentage |
|---:|:--------------|------------:|------------:|-----------:|--------------:|-------------:|
|  0 | bmw           |       18174 |      130449 |       2016 |        145933 |         8.27 |
|  1 | volkswagen    |       17833 |       74211 |       2016 |        152537 |         8.11 |
|  2 | audi          |       17472 |      116820 |       2015 |        155155 |         7.95 |
|  3 | ford          |       16049 |       53473 |       2015 |        158921 |         7.3  |
|  4 | opel          |       14986 |       36486 |       2014 |        161704 |         6.82 |
|  5 | mercedes-benz |       13741 |      166602 |       2016 |        126603 |         6.25 |
|  6 | toyota        |       12507 |       76542 |       2017 |        117795 |         5.69 |
|  7 | skoda         |       10515 |       74952 |       2018 |        132820 |         4.78 |
|  8 | renault       |       10159 |       50205 |       2016 |        135621 |         4.62 |
|  9 | peugeot       |        8919 |       48333 |       2016 |        143064 |         4.06 |
| 10 | kia           |        8561 |       69036 |       2017 |        109680 |         3.89 |
| 11 | hyundai       |        8516 |       63182 |       2017 |        116997 |         3.87 |
| 12 | volvo         |        7531 |      100374 |       2016 |        154284 |         3.43 |
| 13 | citroen       |        6396 |       37695 |       2015 |        151018 |         2.91 |
| 14 | nissan        |        5573 |       52924 |       2015 |        135940 |         2.54 |
| 15 | mazda         |        4108 |       70907 |       2016 |        124343 |         1.87 |
| 16 | fiat          |        3766 |       35267 |       2014 |        135461 |         1.71 |
| 17 | seat          |        3751 |       45271 |       2015 |        152140 |         1.71 |
| 18 | honda         |        2945 |       50399 |       2013 |        164696 |         1.34 |
| 19 | dacia         |        2719 |       48913 |       2019 |         87955 |         1.24 |
