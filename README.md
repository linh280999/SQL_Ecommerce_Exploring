# SQL_Ecommerce_Exploring
Using SQL in Big Query to collect, organize and connect data from separate worksheets to calculate percisely information for differenct reports
## Table of content:
1. Introduction
2. Import raw data
3. Discription Table
4. Exploring Dataset
## Introduction
The eCommerce dataset is stored in a public Google BigQuery dataset and contains information about user sessions on a website collected from Google Analytics in 2017.

Based on the eCommerce dataset, the author perform queries to analyze following content:
- Calculate total/average visit, pageview, transaction
- Bounce rate analysis
- Revenue analysis
- Products analysis
- Cohort Analysis
- YoY/MoM growth rate, ratio of stock, retention rate
## Import raw data
The eCommerce dataset is stored in a public Google BigQuery dataset. To access the dataset, follow these steps:
- Log in to your Google Cloud Platform account and create a new project.
- Navigate to the BigQuery console and select your newly created project.
- Select "Add Data" in the navigation panel and then "Search a project".
- Enter the project ID **"bigquery-public-data.google_analytics_sample.ga_sessions"** and click "Enter".
- Click on the **"ga_sessions_"** table to open it.
## Discription Table
| **Field Name**                   | **Data Type** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|----------------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fullVisitorId                    | STRING        | The unique visitor ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| date                             | STRING        | The date of the session in YYYYMMDD format.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| totals                           | RECORD        | This section contains aggregate values across the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| totals.bounces                   | INTEGER       | Total bounces (for convenience). For a bounced session, the value is 1, otherwise it is null.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| totals.hits                      | INTEGER       | Total number of hits within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| totals.pageviews                 | INTEGER       | Total number of pageviews within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| totals.visits                    | INTEGER       | The number of sessions (for convenience). This value is 1 for sessions with interaction events. The value is null if there are no interaction events in the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| trafficSource.source             | STRING        | The source of the traffic source. Could be the name of the search engine, the referring hostname, or a value of the utm_source URL parameter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| hits                             | RECORD        | This row and nested fields are populated for any and all types of hits.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| hits.eCommerceAction             | RECORD        | This section contains all of the ecommerce hits that occurred during the session. This is a repeated field and has an entry for each hit that was collected.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| hits.eCommerceAction.action_type | STRING        | "The action type. Click through of product lists = 1, Product detail views = 2, Add product(s) to cart = 3, Remove product(s) from cart = 4, Check out = 5, Completed purchase = 6, Refund of purchase = 7, Checkout options = 8, Unknown = 0. Usually this action type applies to all the products in a hit, with the following exception: when hits.product.isImpression = TRUE, the corresponding product is a product impression that is seen while the product action is taking place (i.e., a ""product in list view""). Example query to calculate number of products in list views: SELECT COUNT(hits.product.v2ProductName) FROM [foo-160803:123456789.ga_sessions_20170101] WHERE hits.product.isImpression == TRUE Example query to calculate number of products in detailed view: SELECT COUNT(hits.product.v2ProductName), FROM [foo 160803:123456789.ga_sessions_20170101] WHERE hits.ecommerceaction.action_type = '2' AND ( BOOLEAN(hits.product.isImpression) IS NULL OR BOOLEAN(hits.product.isImpression) == FALSE )" |
| hits.product                     | RECORD        | This row and nested fields will be populated for each hit that contains Enhanced Ecommerce PRODUCT data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| hits.product.productQuantity     | INTEGER       | The quantity of the product purchased.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| hits.product.productRevenue      | INTEGER       | The revenue of the product, expressed as the value passed to Analytics multiplied by 10^6 (e.g., 2.40 would be given as 2400000).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| hits.product.productSKU          | STRING        | Product SKU.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| hits.product.v2ProductName       | STRING        | Product Name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| fullVisitorId                    | STRING        | The unique visitor ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
## Exploring dataset
**4.1. Calculate total visit, pageview, transaction for Jan, Feb and March 2017 (order by month)**
```s
SELECT format_datetime('%Y%m', parse_datetime('%Y%m%d', date)) as month
      ,sum(totals.visits) as visits
      ,sum(totals.pageviews) as pageviews
      ,sum(totals.transactions) as transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` 
where _table_suffix between '0101' and '0331'
group by month
order by month;
```
|**month**|**visits**|**pageviews**|**transactions**|**revenue**|
|---------|----------|-------------|----------------|-----------|
|201701	  |64694	   |257708	     |713	            |106248.15  |
|201702	  |62192	   |233373	     |733	            |116111.6   |
|201703	  |69931	   |259522	     |993	            |150224.7   |

**4.2. Bounce rate per traffic source in July 2017 (Bounce_rate = num_bounce/total_visit) (order by total_visit DESC)**
```s
SELECT  trafficSource.source as source
        ,sum(totals.visits) as total_visits
        ,sum(totals.bounces) as total_no_of_bounces
        ,round(sum(totals.bounces)*100/sum(totals.visits),2) as bounce_rate
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
group by source
order by total_visits desc;
```
| **source**                  | **total_visits** | **total_no_of_bounces** | **bounce_rate** |
|-----------------------------|------------------|-------------------------|-----------------|
| google                      | 38400            | 19798                   | 51.56           |
| (direct)                    | 19891            | 8606                    | 43.27           |
| youtube.com                 | 6351             | 4238                    | 66.73           |
| analytics.google.com        | 1972             | 1064                    | 53.96           |
| Partners                    | 1788             | 936                     | 52.35           |
| m.facebook.com              | 669              | 430                     | 64.28           |
| google.com                  | 368              | 183                     | 49.73           |
| dfa                         | 302              | 124                     | 41.06           |
| sites.google.com            | 230              | 97                      | 42.17           |
| facebook.com                | 191              | 102                     | 53.4            |
| reddit.com                  | 189              | 54                      | 28.57           |
| qiita.com                   | 146              | 72                      | 49.32           |
| baidu                       | 140              | 84                      | 60.0            |
| quora.com                   | 140              | 70                      | 50.0            |
| bing                        | 111              | 54                      | 48.65           |
| mail.google.com             | 101              | 25                      | 24.75           |
| yahoo                       | 100              | 41                      | 41.0            |
| blog.golang.org             | 65               | 19                      | 29.23           |
| l.facebook.com              | 51               | 45                      | 88.24           |
| groups.google.com           | 50               | 22                      | 44.0            |
| t.co                        | 38               | 27                      | 71.05           |
| google.co.jp                | 36               | 25                      | 69.44           |
| m.youtube.com               | 34               | 22                      | 64.71           |
| dealspotr.com               | 26               | 12                      | 46.15           |
| productforums.google.com    | 25               | 21                      | 84.0            |
| ask                         | 24               | 16                      | 66.67           |
| support.google.com          | 24               | 16                      | 66.67           |
| int.search.tb.ask.com       | 23               | 17                      | 73.91           |
| optimize.google.com         | 21               | 10                      | 47.62           |
| docs.google.com             | 20               | 8                       | 40.0            |
| lm.facebook.com             | 18               | 9                       | 50.0            |
| l.messenger.com             | 17               | 6                       | 35.29           |
| adwords.google.com          | 16               | 7                       | 43.75           |
| duckduckgo.com              | 16               | 14                      | 87.5            |
| google.co.uk                | 15               | 7                       | 46.67           |
| sashihara.jp                | 14               | 8                       | 57.14           |
| lunametrics.com             | 13               | 8                       | 61.54           |
| search.mysearch.com         | 12               | 11                      | 91.67           |
| outlook.live.com            | 10               | 7                       | 70.0            |
| tw.search.yahoo.com         | 10               | 8                       | 80.0            |
| phandroid.com               | 9                | 7                       | 77.78           |
| plus.google.com             | 8                | 2                       | 25.0            |
| connect.googleforwork.com   | 8                | 5                       | 62.5            |
| m.yz.sm.cn                  | 7                | 5                       | 71.43           |
| search.xfinity.com          | 6                | 6                       | 100.0           |
| google.co.in                | 6                | 3                       | 50.0            |
| google.ru                   | 5                | 1                       | 20.0            |
| s0.2mdn.net                 | 5                | 3                       | 60.0            |
| hangouts.google.com         | 5                | 1                       | 20.0            |
| online-metrics.com          | 5                | 2                       | 40.0            |
| m.sogou.com                 | 4                | 3                       | 75.0            |
| away.vk.com                 | 4                | 3                       | 75.0            |
| googleads.g.doubleclick.net | 4                | 1                       | 25.0            |
| in.search.yahoo.com         | 4                | 2                       | 50.0            |
| getpocket.com               | 3                |                         |                 |
| siliconvalley.about.com     | 3                | 2                       | 66.67           |
| m.baidu.com                 | 3                | 2                       | 66.67           |
| wap.sogou.com               | 2                | 2                       | 100.0           |
| google.it                   | 2                | 1                       | 50.0            |
| google.co.th                | 2                | 1                       | 50.0            |
| msn.com                     | 2                | 1                       | 50.0            |
| calendar.google.com         | 2                | 1                       | 50.0            |
| google.cl                   | 2                | 1                       | 50.0            |
| moodle.aurora.edu           | 2                | 2                       | 100.0           |
| uk.search.yahoo.com         | 2                | 1                       | 50.0            |
| amp.reddit.com              | 2                | 1                       | 50.0            |
| search.1and1.com            | 2                | 2                       | 100.0           |
| au.search.yahoo.com         | 2                | 2                       | 100.0           |
| m.sp.sm.cn                  | 2                | 2                       | 100.0           |
| github.com                  | 2                | 2                       | 100.0           |
| centrum.cz                  | 2                | 2                       | 100.0           |
| myactivity.google.com       | 2                | 1                       | 50.0            |
| plus.url.google.com         | 2                |                         |                 |
| kidrex.org                  | 1                | 1                       | 100.0           |
| gophergala.com              | 1                | 1                       | 100.0           |
| google.ca                   | 1                |                         |                 |
| newclasses.nyu.edu          | 1                |                         |                 |
| earth.google.com            | 1                |                         |                 |
| google.nl                   | 1                |                         |                 |
| aol                         | 1                |                         |                 |
| google.es                   | 1                | 1                       | 100.0           |
| kik.com                     | 1                | 1                       | 100.0           |
| malaysia.search.yahoo.com   | 1                | 1                       | 100.0           |
| suche.t-online.de           | 1                | 1                       | 100.0           |
| google.com.br               | 1                |                         |                 |
| es.search.yahoo.com         | 1                | 1                       | 100.0           |
| online.fullsail.edu         | 1                | 1                       | 100.0           |
| ph.search.yahoo.com         | 1                |                         |                 |
| web.mail.comcast.net        | 1                | 1                       | 100.0           |
| search.tb.ask.com           | 1                |                         |                 |
| google.bg                   | 1                | 1                       | 100.0           |
| news.ycombinator.com        | 1                | 1                       | 100.0           |
| arstechnica.com             | 1                |                         |                 |
| mx.search.yahoo.com         | 1                | 1                       | 100.0           |
| it.pinterest.com            | 1                | 1                       | 100.0           |
| images.google.com.au        | 1                | 1                       | 100.0           |
| web.facebook.com            | 1                | 1                       | 100.0           |

**4.3. Revenue by traffic source by week, by month in June 2017**
```s
SELECT  'Month' as time_type
        ,format_datetime('%Y%m', parse_datetime('%Y%m%d', date)) as time
        ,trafficSource.source as source
        ,sum(productRevenue)/1000000 as revenue
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE product.productRevenue is not null
group by time, source
UNION ALL
SELECT  'Week' as time_type
        ,format_datetime('%Y%W', parse_datetime('%Y%m%d', date)) as time
        ,trafficSource.source as source
        ,sum(productRevenue)/1000000 as revenue
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE product.productRevenue is not null
group by time, source
order by revenue desc;
```
| **time_type** | **time** | **source**        | **revenue**  |
|---------------|----------|-------------------|--------------|
| Month         | 201706   | (direct)          | 97333.619695 |
| Week          | 201724   | (direct)          | 30908.909927 |
| Week          | 201725   | (direct)          | 27295.319924 |
| Month         | 201706   | google            | 18757.17992  |
| Week          | 201723   | (direct)          | 17325.679919 |
| Week          | 201726   | (direct)          | 14914.80995  |
| Week          | 201724   | google            | 9217.169976  |
| Month         | 201706   | dfa               | 8862.229996  |
| Week          | 201722   | (direct)          | 6888.899975  |
| Week          | 201726   | google            | 5330.569964  |
| Week          | 201726   | dfa               | 3704.74      |
| Month         | 201706   | mail.google.com   | 2563.13      |
| Week          | 201724   | mail.google.com   | 2486.86      |
| Week          | 201724   | dfa               | 2341.56      |
| Week          | 201722   | google            | 2119.38999   |
| Week          | 201722   | dfa               | 1670.649998  |
| Week          | 201723   | dfa               | 1145.279998  |
| Week          | 201723   | google            | 1083.949999  |
| Week          | 201725   | google            | 1006.099991  |
| Month         | 201706   | search.myway.com  | 105.939998   |
| Week          | 201723   | search.myway.com  | 105.939998   |
| Month         | 201706   | groups.google.com | 101.96       |
| Week          | 201725   | mail.google.com   | 76.27        |
| Month         | 201706   | chat.google.com   | 74.03        |
| Week          | 201723   | chat.google.com   | 74.03        |
| Month         | 201706   | dealspotr.com     | 72.95        |
| Week          | 201724   | dealspotr.com     | 72.95        |
| Month         | 201706   | mail.aol.com      | 64.849998    |
| Week          | 201725   | mail.aol.com      | 64.849998    |
| Week          | 201726   | groups.google.com | 63.37        |
| Month         | 201706   | phandroid.com     | 52.95        |
| Week          | 201725   | phandroid.com     | 52.95        |
| Month         | 201706   | sites.google.com  | 39.17        |
| Week          | 201725   | groups.google.com | 38.59        |
| Week          | 201725   | sites.google.com  | 25.19        |
| Month         | 201706   | google.com        | 23.99        |
| Week          | 201725   | google.com        | 23.99        |
| Month         | 201706   | yahoo             | 20.39        |
| Week          | 201726   | yahoo             | 20.39        |
| Month         | 201706   | youtube.com       | 16.99        |
| Week          | 201723   | youtube.com       | 16.99        |
| Week          | 201722   | sites.google.com  | 13.98        |
| Month         | 201706   | bing              | 13.98        |
| Week          | 201724   | bing              | 13.98        |
| Month         | 201706   | l.facebook.com    | 12.48        |
| Week          | 201724   | l.facebook.com    | 12.48        |

**4.4. Average number of pageviews by purchaser type (purchasers vs non-purchasers) in June, July 2017.**
```s
with purchase as
(SELECT  format_datetime('%Y%m', parse_datetime('%Y%m%d', date)) as month,
        round(sum(totals.pageviews)/count(distinct fullVisitorId),2) as avg_pageviews_purchase
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE product.productRevenue is not null
      and totals.transactions >=1
      and _table_suffix between '0601' and '0731'
group by month
)
,non_purchase as
(SELECT  format_datetime('%Y%m', parse_datetime('%Y%m%d', date)) as month,
        round(sum(totals.pageviews)/count(distinct fullVisitorId),2) as avg_pageviews_non_purchase
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE product.productRevenue is null
      and totals.transactions is null
      and _table_suffix between '0601' and '0731'
group by month
)

select purchase.month,
        avg_pageviews_purchase,
        avg_pageviews_non_purchase
from purchase left join non_purchase on purchase.month=non_purchase.month
order by month asc;
```
| **month**     | **avg_pageviews_purchase** | **avg_pageviews_non_purchase**|
|---------------|----------------------------|-------------------------------|
| 201706        | 94.02                      | 316.87                        |
| 201707        | 124.24                     | 334.06                        |

**4.5. Average number of transactions per user that made a purchase in July 2017.**
```s
SELECT  format_datetime('%Y%m', parse_datetime('%Y%m%d', date)) as month,
        round(sum(totals.transactions)/count(distinct fullVisitorId),2) as Avg_total_transactions_per_user
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE product.productRevenue is not null
      and totals.transactions >=1
      and _table_suffix between '0701' and '0731'
group by month;
```
| **month**     | **Avg_total_transactions_per_user** |
|---------------|-------------------------------------|
| 201707        | 4.16                                |

**4.6. Average amount of money spent per session. Only include purchaser data in July 2017.**
```s
SELECT  format_datetime('%Y%m', parse_datetime('%Y%m%d', date)) as month,
        round((sum(productRevenue)/sum(totals.visits))/1000000,2) as avg_revenue_by_user_per_visit
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE product.productRevenue is not null
      and totals.transactions is not null
      and _table_suffix between '0701' and '0731'
group by month;
```
| **month**     | **avg_revenue_by_user_per_visit**   |
|---------------|-------------------------------------|
| 201707        | 43.86                               |

**4.7. Other products purchased by customers who purchased product "YouTube Men's Vintage Henley" in July 2017. Output should show product name and the quantity was ordered.**

```s
with ID as
(SELECT  distinct fullVisitorId
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE product.productRevenue is not null
      and _table_suffix between '0701' and '0731'
      and v2ProductName like "YouTube Men's Vintage Henley"
)
SELECT  v2ProductName as other_purchased_products,
        sum(productQuantity) as quantity
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` as n1
,UNNEST (hits) hits
,UNNEST (hits.product) product
inner join ID on n1.fullVisitorId=ID.fullVisitorId
WHERE product.productRevenue is not null
      and _table_suffix between '0701' and '0731'
      and v2ProductName not like "YouTube Men's Vintage Henley"
group by v2ProductName
order by quantity desc;
```
| **other_purchased_products**                              | **quantity** |
|-----------------------------------------------------------|--------------|
| Google Sunglasses                                         | 20           |
| Google Women's Vintage Hero Tee Black                     | 7            |
| SPF-15 Slim & Slender Lip Balm                            | 6            |
| Google Women's Short Sleeve Hero Tee Red Heather          | 4            |
| YouTube Men's Fleece Hoodie Black                         | 3            |
| Google Men's Short Sleeve Badge Tee Charcoal              | 3            |
| Crunch Noise Dog Toy                                      | 2            |
| Android Wool Heather Cap Heather/Black                    | 2            |
| YouTube Twill Cap                                         | 2            |
| Recycled Mouse Pad                                        | 2            |
| Red Shine 15 oz Mug                                       | 2            |
| Google Men's Short Sleeve Hero Tee Charcoal               | 2            |
| Android Women's Fleece Hoodie                             | 2            |
| Google Doodle Decal                                       | 2            |
| Android Men's Vintage Henley                              | 2            |
| 22 oz YouTube Bottle Infuser                              | 2            |
| Android Men's Short Sleeve Hero Tee Heather               | 1            |
| YouTube Women's Short Sleeve Tri-blend Badge Tee Charcoal | 1            |
| 8 pc Android Sticker Sheet                                | 1            |
| YouTube Men's Long & Lean Tee Charcoal                    | 1            |
| Google Women's Long Sleeve Tee Lavender                   | 1            |
| YouTube Hard Cover Journal                                | 1            |
| Android BTTF Moonshot Graphic Tee                         | 1            |
| Google Men's Airflow 1/4 Zip Pullover Black               | 1            |
| Google Men's Performance 1/4 Zip Pullover Heather/Black   | 1            |
| Google Men's Vintage Badge Tee Black                      | 1            |
| Google Men's Bike Short Sleeve Tee Charcoal               | 1            |
| Google 5-Panel Cap                                        | 1            |
| Google Toddler Short Sleeve T-shirt Grey                  | 1            |
| Android Sticker Sheet Ultra Removable                     | 1            |
| Google Twill Cap                                          | 1            |
| Google Men's Long & Lean Tee Grey                         | 1            |
| Google Men's Long & Lean Tee Charcoal                     | 1            |
| YouTube Custom Decals                                     | 1            |
| Four Color Retractable Pen                                | 1            |
| Google Laptop and Cell Phone Stickers                     | 1            |
| Google Men's Long Sleeve Raglan Ocean Blue                | 1            |
| Android Men's Short Sleeve Hero Tee White                 | 1            |
| Android Men's Pep Rally Short Sleeve Tee Navy             | 1            |
| YouTube Men's Short Sleeve Hero Tee Black                 | 1            |
| Google Men's  Zip Hoodie                                  | 1            |
| Google Men's Vintage Badge Tee White                      | 1            |
| Google Men's Pullover Hoodie Grey                         | 1            |
| YouTube Men's Short Sleeve Hero Tee White                 | 1            |
| Google Men's 100% Cotton Short Sleeve Hero Tee Red        | 1            |
| Android Men's Vintage Tank                                | 1            |
| YouTube Women's Short Sleeve Hero Tee Charcoal            | 1            |
| Google Men's Performance Full Zip Jacket Black            | 1            |
| 26 oz Double Wall Insulated Bottle                        | 1            |
| Google Slim Utility Travel Bag                            | 1            |

**4.8. Calculate cohort map from product view to addtocart to purchase in Jan, Feb and March 2017.**

```s
with view as
(SELECT  format_datetime('%Y%m', parse_datetime('%Y%m%d', date)) as month,
        count(v2ProductName) as num_product_view
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE _table_suffix between '0101' and '0331' 
      and eCommerceAction.action_type like '2'
group by month
order by month
)
, addtocart as
(SELECT  format_datetime('%Y%m', parse_datetime('%Y%m%d', date)) as month,
        count(v2ProductName) as num_addtocart
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE _table_suffix between '0101' and '0331' 
      and eCommerceAction.action_type like '3'
group by month
order by month
)
, purchase as
(SELECT  format_datetime('%Y%m', parse_datetime('%Y%m%d', date)) as month,
        count(v2ProductName) as num_purchase
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
,UNNEST (hits) hits
,UNNEST (hits.product) product
WHERE product.productRevenue is not null
      and _table_suffix between '0101' and '0331' 
      and eCommerceAction.action_type like '6'
group by month
order by month
)

select view.month,
        num_product_view,
        num_addtocart,
        num_purchase,
        round(num_addtocart*100/num_product_view,2) as add_to_cart_rate,
        round(num_purchase*100/num_product_view,2) as purchase_rate
from view
left join addtocart on view.month=addtocart.month
left join purchase on view.month=purchase.month
order by view.month;
```
| **month** | **num_product_view** | **num_addtocart** | **num_purchase** | **add_to_cart_rate** | **purchase_rate** |
|-----------|----------------------|-------------------|------------------|----------------------|-------------------|
| 201701    | 25787                | 7342              | 2143             | 28.47                | 8.31              |
| 201702    | 21489                | 7360              | 2060             | 34.25                | 9.59              |
| 201703    | 23549                | 8782              | 2977             | 37.29                | 12.64             |

**4.9. Calculate Quantity of items, Sales value & Order quantity by each Subcategory in last 12 month**
```s
SELECT  format_datetime('%b %Y', a.ModifiedDate) as period
        ,c.Name
        ,sum(a.OrderQty) as qty_item
        ,sum(a.LineTotal) as total_sales
        ,count(distinct a.SalesOrderID) as order_cnt
FROM `adventureworks2019.Sales.SalesOrderDetail` as a
left join `adventureworks2019.Production.Product` as b on a.ProductID=b.ProductID
left join `adventureworks2019.Production.ProductSubcategory` as c on cast(b.ProductSubcategoryID as INT)=c.ProductSubcategoryID
group by period, Name
order by name;
```
| **period** | **Name**          | **qty_item** | **total_sales**    | **order_cnt** |
|------------|-------------------|--------------|--------------------|---------------|
| May 2012   | Bib-Shorts        | 322          | 17240.356192000003 | 31            |
| Jun 2012   | Bib-Shorts        | 490          | 26180.538728000007 | 60            |
| Jul 2012   | Bib-Shorts        | 436          | 23143.997158999995 | 44            |
| Aug 2012   | Bib-Shorts        | 198          | 10659.531475999996 | 22            |
| Sep 2012   | Bib-Shorts        | 210          | 11338.74           | 31            |
| Jan 2013   | Bib-Shorts        | 121          | 6533.2740000000013 | 21            |
| Apr 2013   | Bib-Shorts        | 280          | 14833.123691999994 | 34            |
| Oct 2012   | Bib-Shorts        | 247          | 13299.550107999998 | 25            |
| Nov 2012   | Bib-Shorts        | 121          | 6533.2740000000013 | 24            |
| Dec 2012   | Bib-Shorts        | 157          | 8477.0580000000027 | 29            |
| Feb 2013   | Bib-Shorts        | 211          | 11361.453475999999 | 29            |
| Mar 2013   | Bib-Shorts        | 318          | 17138.811475999988 | 40            |
| May 2013   | Bib-Shorts        | 2            | 116.987            | 1             |
| Feb 2014   | Bib-Shorts        | 4            | 233.974            | 2             |
| Jun 2013   | Bib-Shorts        | 2            | 116.987            | 1             |
| Jul 2013   | Bib-Shorts        | 2            | 116.987            | 1             |
| Apr 2014   | Bib-Shorts        | 4            | 233.974            | 1             |
| May 2013   | Bike Racks        | 221          | 14905.272000000003 | 27            |
| Jun 2013   | Bike Racks        | 372          | 25764.000000000004 | 66            |
| Jul 2013   | Bike Racks        | 422          | 29802.3            | 75            |
| Aug 2013   | Bike Racks        | 222          | 17387.183999999997 | 63            |
| Sep 2013   | Bike Racks        | 312          | 22828.512000000002 | 71            |
| Oct 2013   | Bike Racks        | 284          | 21181.2            | 70            |
| Nov 2013   | Bike Racks        | 142          | 11472.0            | 50            |
| Jan 2014   | Bike Racks        | 161          | 12840.0            | 53            |
| Mar 2014   | Bike Racks        | 492          | 35588.711999999992 | 100           |
| May 2014   | Bike Racks        | 284          | 21704.688          | 81            |
| Dec 2013   | Bike Racks        | 162          | 12582.288          | 48            |
| Feb 2014   | Bike Racks        | 27           | 3240.0             | 27            |
| Apr 2014   | Bike Racks        | 45           | 5400.0             | 45            |
| Jun 2014   | Bike Racks        | 20           | 2400.0             | 20            |
| May 2013   | Bike Stands       | 1            | 159.0              | 1             |
| Jun 2013   | Bike Stands       | 5            | 795.0              | 5             |
| Jul 2013   | Bike Stands       | 19           | 3021.0             | 19            |
| Aug 2013   | Bike Stands       | 20           | 3180.0             | 20            |
| Sep 2013   | Bike Stands       | 26           | 4134.0             | 26            |
| Oct 2013   | Bike Stands       | 24           | 3816.0             | 24            |
| Nov 2013   | Bike Stands       | 24           | 3816.0             | 24            |
| Dec 2013   | Bike Stands       | 17           | 2703.0             | 17            |
| Jan 2014   | Bike Stands       | 18           | 2862.0             | 18            |
| Feb 2014   | Bike Stands       | 17           | 2703.0             | 17            |
| Mar 2014   | Bike Stands       | 31           | 4929.0             | 31            |
| Apr 2014   | Bike Stands       | 23           | 3657.0             | 23            |
| May 2014   | Bike Stands       | 13           | 2067.0             | 13            |
| Jun 2014   | Bike Stands       | 11           | 1749.0             | 11            |
| May 2013   | Bottles and Cages | 250          | 756.61837500000036 | 36            |
| Jun 2013   | Bottles and Cages | 581          | 2726.8575999999853 | 186           |
| Jul 2013   | Bottles and Cages | 845          | 4705.5903919999346 | 386           |
| Aug 2013   | Bottles and Cages | 761          | 4525.439507999944  | 377           |
| Sep 2013   | Bottles and Cages | 803          | 4676.56280799994   | 380           |
| Oct 2013   | Bottles and Cages | 845          | 5038.95319199993   | 400           |
| Nov 2013   | Bottles and Cages | 894          | 5883.5719999999092 | 482           |
| Dec 2013   | Bottles and Cages | 793          | 5158.6617919999308 | 431           |
| Jan 2014   | Bottles and Cages | 869          | 5729.7459999999128 | 472           |
| Mar 2014   | Bottles and Cages | 1228         | 6912.5938839998644 | 537           |
| May 2014   | Bottles and Cages | 1043         | 6564.597775999885  | 523           |
| Feb 2014   | Bottles and Cages | 655          | 4663.4499999999434 | 389           |
| Apr 2014   | Bottles and Cages | 734          | 5239.65999999993   | 435           |
| Jun 2014   | Bottles and Cages | 251          | 1692.4900000000018 | 178           |
| May 2013   | Bottom Brackets   | 62           | 3385.4280000000008 | 16            |
| Jun 2013   | Bottom Brackets   | 93           | 5280.6420000000016 | 26            |
| Oct 2013   | Bottom Brackets   | 132          | 7030.0080000000025 | 30            |
| Jul 2013   | Bottom Brackets   | 155          | 8787.570000000007  | 36            |
| Aug 2013   | Bottom Brackets   | 61           | 3191.034000000001  | 11            |
| Sep 2013   | Bottom Brackets   | 60           | 3118.1400000000008 | 19            |
| Mar 2014   | Bottom Brackets   | 118          | 5928.4920000000029 | 29            |
| May 2014   | Bottom Brackets   | 134          | 7378.2960000000048 | 32            |
| Nov 2013   | Bottom Brackets   | 24           | 1749.4560000000001 | 9             |
| Dec 2013   | Bottom Brackets   | 40           | 2915.7599999999998 | 19            |
| Jan 2014   | Bottom Brackets   | 42           | 3061.548           | 21            |
| Jun 2013   | Brakes            | 133          | 8417.9304000000011 | 27            |
| Jul 2013   | Brakes            | 205          | 13099.500000000007 | 42            |
| May 2013   | Brakes            | 80           | 5112.0             | 16            |
| Aug 2013   | Brakes            | 49           | 3131.1000000000008 | 13            |
| Sep 2013   | Brakes            | 100          | 6390.0             | 29            |
| Oct 2013   | Brakes            | 142          | 9036.7806000000037 | 38            |
| Mar 2014   | Brakes            | 107          | 6837.3000000000011 | 39            |
| May 2014   | Brakes            | 92           | 5878.8000000000029 | 39            |
| Nov 2013   | Brakes            | 26           | 1661.3999999999999 | 8             |
| Jan 2014   | Brakes            | 69           | 4409.0999999999985 | 28            |
| Dec 2013   | Brakes            | 32           | 2044.7999999999997 | 16            |
| Jul 2011   | Caps              | 103          | 530.65826399999992 | 27            |
| Oct 2011   | Caps              | 240          | 1238.2038720000012 | 66            |
| Jan 2012   | Caps              | 153          | 790.52960800000062 | 45            |
| May 2012   | Caps              | 215          | 1081.6146220000003 | 33            |
| Jun 2012   | Caps              | 345          | 1731.0467370000008 | 56            |
| Jul 2012   | Caps              | 230          | 1119.7058970000003 | 36            |
| Aug 2012   | Caps              | 154          | 798.721            | 31            |
| Sep 2012   | Caps              | 224          | 1158.4979360000009 | 52            |
| Dec 2012   | Caps              | 135          | 700.17750000000012 | 30            |
| Jan 2013   | Caps              | 124          | 639.84793600000012 | 25            |
| Apr 2013   | Caps              | 182          | 926.12188800000024 | 36            |
| Jul 2013   | Caps              | 526          | 3331.7443439999947 | 214           |
| Aug 2013   | Caps              | 354          | 2450.5076850000019 | 188           |
| Sep 2013   | Caps              | 440          | 2879.4826160000002 | 203           |
| Jan 2014   | Caps              | 365          | 2727.6774759999917 | 243           |
| Mar 2014   | Caps              | 663          | 4219.747775999982  | 277           |
| Jun 2013   | Caps              | 465          | 2611.3396840000009 | 109           |
| Oct 2013   | Caps              | 406          | 2738.8412579999995 | 208           |
| Dec 2013   | Caps              | 301          | 2281.6620000000021 | 210           |
| May 2014   | Caps              | 456          | 3294.2056999999854 | 281           |
| May 2011   | Caps              | 40           | 207.45999999999998 | 14            |
| Aug 2011   | Caps              | 137          | 710.5505000000004  | 40            |
| Dec 2011   | Caps              | 25           | 129.6625           | 12            |
| Feb 2012   | Caps              | 48           | 248.95199999999997 | 18            |
| Mar 2012   | Caps              | 125          | 633.769452         | 34            |
| Oct 2012   | Caps              | 220          | 1103.8528640000002 | 37            |
| Nov 2012   | Caps              | 85           | 440.8525           | 21            |
| Feb 2013   | Caps              | 146          | 754.22410799999989 | 32            |
| Mar 2013   | Caps              | 244          | 1244.1336520000009 | 53            |
| May 2013   | Caps              | 261          | 1295.7682560000005 | 30            |
| Nov 2013   | Caps              | 319          | 2429.2849920000008 | 221           |
| Apr 2012   | Caps              | 114          | 591.26099999999974 | 34            |
| Feb 2014   | Caps              | 180          | 1618.2000000000014 | 180           |
| Apr 2014   | Caps              | 193          | 1735.0700000000015 | 193           |
| Jun 2014   | Caps              | 93           | 836.07000000000062 | 93            |
| May 2013   | Chains            | 40           | 485.7600000000001  | 14            |
| Jun 2013   | Chains            | 91           | 1098.0685760000003 | 23            |
| Oct 2013   | Chains            | 93           | 1122.3565760000004 | 31            |
| Dec 2013   | Chains            | 39           | 473.61600000000016 | 16            |
| Jan 2014   | Chains            | 64           | 777.21600000000024 | 28            |
| Mar 2014   | Chains            | 93           | 1129.3920000000003 | 32            |
| May 2014   | Chains            | 84           | 1020.0960000000003 | 29            |
| Jul 2013   | Chains            | 156          | 1886.7889920000005 | 32            |
| Sep 2013   | Chains            | 62           | 752.92800000000022 | 24            |
| Aug 2013   | Chains            | 28           | 340.03200000000004 | 12            |
| Nov 2013   | Chains            | 24           | 291.456            | 9             |
| Jun 2013   | Cleaners          | 359          | 1705.5667649999998 | 78            |
| May 2013   | Cleaners          | 227          | 1016.99103         | 27            |
| Jul 2013   | Cleaners          | 344          | 1830.316575000003  | 118           |
| Aug 2013   | Cleaners          | 241          | 1340.6275800000033 | 99            |
| Sep 2013   | Cleaners          | 296          | 1611.8307000000029 | 108           |
| Oct 2013   | Cleaners          | 294          | 1612.7560800000033 | 108           |
| Nov 2013   | Cleaners          | 202          | 1230.6600000000039 | 111           |
| Dec 2013   | Cleaners          | 202          | 1262.4600000000041 | 119           |
| Jan 2014   | Cleaners          | 182          | 1110.2365800000027 | 100           |
| Mar 2014   | Cleaners          | 454          | 2349.5628750000024 | 138           |
| May 2014   | Cleaners          | 336          | 1889.0638950000043 | 139           |
| Feb 2014   | Cleaners          | 52           | 413.39999999999964 | 52            |
| Apr 2014   | Cleaners          | 88           | 699.60000000000059 | 88            |
| Jun 2014   | Cleaners          | 42           | 333.89999999999975 | 42            |
| May 2013   | Cranksets         | 88           | 16060.804424       | 15            |
| Jun 2013   | Cranksets         | 149          | 27766.233792       | 23            |
| Jul 2013   | Cranksets         | 191          | 36294.954000000005 | 39            |
| Aug 2013   | Cranksets         | 60           | 10448.64           | 13            |
| Sep 2013   | Cranksets         | 75           | 13955.85           | 20            |
| Oct 2013   | Cranksets         | 118          | 19722.792          | 33            |
| Nov 2013   | Cranksets         | 32           | 6949.608           | 10            |
| Dec 2013   | Cranksets         | 55           | 10610.67           | 17            |
| Mar 2014   | Cranksets         | 131          | 23294.814000000006 | 30            |
| May 2014   | Cranksets         | 123          | 20937.762          | 34            |
| Jan 2014   | Cranksets         | 85           | 17900.490000000005 | 27            |
| Jul 2013   | Derailleurs       | 215          | 12989.021999999997 | 42            |
| Sep 2013   | Derailleurs       | 97           | 5972.069999999997  | 23            |
| Jun 2013   | Derailleurs       | 128          | 7583.5248999999976 | 23            |
| Mar 2014   | Derailleurs       | 167          | 10300.164          | 41            |
| May 2013   | Derailleurs       | 79           | 4786.1759999999995 | 14            |
| Aug 2013   | Derailleurs       | 77           | 4698.5320759999986 | 18            |
| Oct 2013   | Derailleurs       | 135          | 8291.8079999999918 | 35            |
| May 2014   | Derailleurs       | 141          | 8616.6608239999969 | 34            |
| Nov 2013   | Derailleurs       | 35           | 1921.29            | 11            |
| Dec 2013   | Derailleurs       | 38           | 2085.9719999999998 | 17            |
| Jan 2014   | Derailleurs       | 54           | 2964.2759999999989 | 25            |
| May 2013   | Fenders           | 2            | 43.96              | 2             |
| Jun 2013   | Fenders           | 50           | 1099.0000000000005 | 50            |
| Jul 2013   | Fenders           | 153          | 3362.9400000000023 | 153           |
| Aug 2013   | Fenders           | 151          | 3318.9800000000023 | 151           |
| Sep 2013   | Fenders           | 169          | 3714.6200000000026 | 169           |
| Oct 2013   | Fenders           | 170          | 3736.6000000000026 | 170           |
| Nov 2013   | Fenders           | 192          | 4220.1600000000008 | 192           |
| Dec 2013   | Fenders           | 201          | 4417.9799999999968 | 201           |
| Jan 2014   | Fenders           | 163          | 3582.7400000000025 | 163           |
| Feb 2014   | Fenders           | 159          | 3494.8200000000024 | 159           |
| Mar 2014   | Fenders           | 183          | 4022.3400000000029 | 183           |
| Apr 2014   | Fenders           | 217          | 4769.65999999999   | 217           |
| May 2014   | Fenders           | 208          | 4571.8399999999938 | 208           |
| Jun 2014   | Fenders           | 103          | 2263.9400000000014 | 103           |
| Jun 2012   | Forks             | 89           | 11035.715999999999 | 16            |
| Apr 2013   | Forks             | 69           | 8525.6459999999988 | 11            |
| May 2012   | Forks             | 39           | 4638.6359999999995 | 6             |
| Jul 2012   | Forks             | 88           | 10455.161623999998 | 12            |
| Aug 2012   | Forks             | 34           | 4096.452           | 6             |
| Sep 2012   | Forks             | 77           | 9529.6739999999991 | 17            |
| Oct 2012   | Forks             | 83           | 9916.9799999999977 | 13            |
| Feb 2013   | Forks             | 18           | 2185.92            | 6             |
| Mar 2013   | Forks             | 65           | 7633.5359999999991 | 17            |
| Nov 2012   | Forks             | 15           | 2065.41            | 6             |
| Dec 2012   | Forks             | 27           | 3717.7379999999994 | 12            |
| Jan 2013   | Forks             | 30           | 4130.82            | 11            |
| Jun 2013   | Gloves            | 568          | 8460.477034        | 82            |
| Jul 2013   | Gloves            | 620          | 9985.11807599999   | 157           |
| Aug 2013   | Gloves            | 468          | 7931.9485479999848 | 142           |
| Sep 2013   | Gloves            | 444          | 7552.24579199999   | 146           |
| Oct 2013   | Gloves            | 447          | 7678.8126749999883 | 148           |
| Nov 2013   | Gloves            | 265          | 5080.5092759999843 | 137           |
| Mar 2014   | Gloves            | 643          | 10774.551827999981 | 196           |
| May 2014   | Gloves            | 413          | 7539.81466799998   | 189           |
| May 2013   | Gloves            | 401          | 5737.0445430000018 | 31            |
| Jan 2014   | Gloves            | 250          | 5027.4452759999795 | 151           |
| Dec 2013   | Gloves            | 249          | 4834.3259999999837 | 134           |
| Feb 2014   | Gloves            | 92           | 2253.0799999999986 | 92            |
| Apr 2014   | Gloves            | 155          | 3797.5779999999868 | 148           |
| Jun 2014   | Gloves            | 65           | 1591.8500000000004 | 65            |
| May 2012   | Gloves            | 773          | 14271.987906       | 33            |
| Jun 2012   | Gloves            | 998          | 19545.408113       | 66            |
| Jul 2012   | Gloves            | 887          | 17353.062453000002 | 51            |
| Aug 2012   | Gloves            | 522          | 10190.096582999995 | 43            |
| Sep 2012   | Gloves            | 728          | 14712.496185       | 69            |
| Oct 2012   | Gloves            | 644          | 12723.009299999994 | 54            |
| Nov 2012   | Gloves            | 339          | 6817.938033        | 35            |
| Dec 2012   | Gloves            | 551          | 11230.988866999993 | 54            |
| Feb 2013   | Gloves            | 579          | 11105.353199000005 | 37            |
| Mar 2013   | Gloves            | 885          | 16829.597149999998 | 68            |
| Apr 2013   | Gloves            | 673          | 13155.074388999996 | 50            |
| Jan 2013   | Gloves            | 353          | 7332.1706730000005 | 47            |
| May 2012   | Handlebars        | 204          | 8670.3255          | 37            |
| Jun 2012   | Handlebars        | 353          | 14312.218000000006 | 73            |
| Jul 2012   | Handlebars        | 313          | 12358.542800000001 | 58            |
| Aug 2012   | Handlebars        | 117          | 5150.5524120000009 | 26            |
| Sep 2012   | Handlebars        | 166          | 7304.9396000000015 | 42            |
| Oct 2012   | Handlebars        | 128          | 5846.1175000000012 | 37            |
| Jan 2013   | Handlebars        | 80           | 2398.5999999999995 | 21            |
| Feb 2013   | Handlebars        | 100          | 4533.3947000000007 | 26            |
| Mar 2013   | Handlebars        | 138          | 5881.8441          | 39            |
| Apr 2013   | Handlebars        | 156          | 6771.1401120000019 | 41            |
| Nov 2012   | Handlebars        | 51           | 1608.7394999999997 | 17            |
| Dec 2012   | Handlebars        | 75           | 2267.6474999999996 | 22            |
| Jul 2013   | Handlebars        | 386          | 16878.18226400001  | 86            |
| Jun 2013   | Handlebars        | 381          | 16508.978992000018 | 93            |
| May 2013   | Handlebars        | 215          | 10186.782          | 50            |
| Sep 2013   | Handlebars        | 131          | 6486.5880000000016 | 47            |
| Oct 2013   | Handlebars        | 188          | 9196.1039999999975 | 56            |
| Nov 2013   | Handlebars        | 40           | 1340.088           | 12            |
| Dec 2013   | Handlebars        | 76           | 2521.1400000000008 | 21            |
| Jan 2014   | Handlebars        | 75           | 2483.9880000000012 | 24            |
| Mar 2014   | Handlebars        | 279          | 13237.409999999998 | 86            |
| Aug 2013   | Handlebars        | 120          | 5992.992           | 34            |
| May 2014   | Handlebars        | 178          | 8655.006           | 62            |
| May 2012   | Headsets          | 46           | 2550.9076320000004 | 6             |
| Jun 2012   | Headsets          | 160          | 8814.5914499999981 | 15            |
| Jul 2012   | Headsets          | 148          | 8216.281871000001  | 13            |
| Oct 2012   | Headsets          | 100          | 6241.4160000000011 | 14            |
| Aug 2012   | Headsets          | 71           | 4349.678172        | 6             |
| Sep 2012   | Headsets          | 99           | 5758.9620000000014 | 16            |
| Dec 2012   | Headsets          | 61           | 4040.0220000000008 | 17            |
| Nov 2012   | Headsets          | 29           | 1784.681996        | 5             |
| Jan 2013   | Headsets          | 80           | 5111.8800000000019 | 12            |
| Mar 2013   | Headsets          | 80           | 5248.1236320000016 | 15            |
| Apr 2013   | Headsets          | 92           | 5984.6116319999992 | 12            |
| Feb 2013   | Headsets          | 43           | 2841.042           | 6             |
| Feb 2013   | Helmets           | 391          | 7613.904016        | 28            |
| Jun 2013   | Helmets           | 956          | 21801.695164000033 | 202           |
| Jul 2013   | Helmets           | 1282         | 32921.891557000279 | 507           |
| Aug 2013   | Helmets           | 937          | 26093.35862400016  | 503           |
| Oct 2013   | Helmets           | 1127         | 30733.9913500002   | 557           |
| Nov 2013   | Helmets           | 850          | 26270.49200000022  | 627           |
| Dec 2013   | Helmets           | 781          | 23590.258000000191 | 540           |
| Jan 2014   | Helmets           | 840          | 25318.76400000021  | 585           |
| Mar 2014   | Helmets           | 1695         | 43928.251484000095 | 692           |
| May 2014   | Helmets           | 1241         | 34956.227652000423 | 697           |
| Sep 2013   | Helmets           | 1016         | 27354.835599000184 | 480           |
| May 2013   | Helmets           | 691          | 9485.7890000000025 | 41            |
| Feb 2014   | Helmets           | 517          | 18061.837999999989 | 516           |
| Apr 2014   | Helmets           | 654          | 22855.468000000208 | 653           |
| Jun 2014   | Helmets           | 262          | 9167.3799999999574 | 262           |
| May 2012   | Helmets           | 492          | 7448.8258800000012 | 36            |
| May 2011   | Helmets           | 84           | 1695.6660000000006 | 15            |
| Jul 2011   | Helmets           | 178          | 3593.1969999999992 | 27            |
| Aug 2011   | Helmets           | 257          | 5187.9304999999968 | 40            |
| Oct 2011   | Helmets           | 484          | 9758.571107999991  | 53            |
| Dec 2011   | Helmets           | 29           | 585.40850000000012 | 6             |
| Jan 2012   | Helmets           | 216          | 4360.2839999999978 | 39            |
| Feb 2012   | Helmets           | 88           | 1776.4120000000007 | 13            |
| Apr 2012   | Helmets           | 212          | 4279.5380000000005 | 23            |
| Jun 2012   | Helmets           | 695          | 13974.332555999972 | 58            |
| Jul 2012   | Helmets           | 582          | 11488.590374999996 | 45            |
| Aug 2012   | Helmets           | 370          | 7421.1622599999982 | 26            |
| Sep 2012   | Helmets           | 478          | 9594.9252280000055 | 35            |
| Oct 2012   | Helmets           | 391          | 7774.0723749999943 | 32            |
| Nov 2012   | Helmets           | 167          | 3371.1455000000005 | 23            |
| Dec 2012   | Helmets           | 216          | 4360.284           | 32            |
| Jan 2013   | Helmets           | 195          | 3923.609436        | 25            |
| Mar 2013   | Helmets           | 469          | 9373.9093639999955 | 37            |
| Apr 2013   | Helmets           | 421          | 8348.681485        | 28            |
| Mar 2012   | Helmets           | 277          | 5577.839264000002  | 29            |
| May 2013   | Hydration Packs   | 168          | 5367.397932        | 28            |
| Jun 2013   | Hydration Packs   | 338          | 11396.589515999987 | 77            |
| Jul 2013   | Hydration Packs   | 318          | 11419.773299999984 | 96            |
| Aug 2013   | Hydration Packs   | 222          | 8536.9555439999913 | 85            |
| Sep 2013   | Hydration Packs   | 249          | 9186.1619849999934 | 87            |
| Oct 2013   | Hydration Packs   | 243          | 9207.6135839999934 | 86            |
| Nov 2013   | Hydration Packs   | 125          | 5400.0179999999918 | 74            |
| Dec 2013   | Hydration Packs   | 139          | 5974.7954759999911 | 78            |
| Jan 2014   | Hydration Packs   | 141          | 6191.873999999988  | 90            |
| Mar 2014   | Hydration Packs   | 368          | 13078.799603999985 | 101           |
| May 2014   | Hydration Packs   | 285          | 10993.089392999987 | 107           |
| Feb 2014   | Hydration Packs   | 64           | 3519.3599999999942 | 64            |
| Apr 2014   | Hydration Packs   | 75           | 4124.2499999999918 | 75            |
| Jun 2014   | Hydration Packs   | 26           | 1429.74            | 26            |
| Jul 2013   | Jerseys           | 1890         | 61736.062710999868 | 283           |
| Jun 2013   | Jerseys           | 1863         | 58337.07989000003  | 155           |
| Aug 2013   | Jerseys           | 1172         | 41362.735830000085 | 300           |
| Sep 2013   | Jerseys           | 1402         | 48318.089182       | 304           |
| Oct 2013   | Jerseys           | 1372         | 47761.25915300005  | 324           |
| Nov 2013   | Jerseys           | 912          | 34516.6878180002   | 333           |
| Dec 2013   | Jerseys           | 915          | 34891.73507600015  | 343           |
| Jan 2014   | Jerseys           | 921          | 34590.931022000157 | 325           |
| Feb 2014   | Jerseys           | 258          | 13345.419999999953 | 258           |
| Mar 2014   | Jerseys           | 2241         | 75862.040340000152 | 423           |
| Apr 2014   | Jerseys           | 296          | 15357.039999999944 | 296           |
| May 2014   | Jerseys           | 1455         | 52521.528226999835 | 426           |
| Jun 2014   | Jerseys           | 146          | 7526.5399999999763 | 146           |
| May 2013   | Jerseys           | 1281         | 38180.12984500001  | 44            |
| May 2012   | Jerseys           | 486          | 13762.868869999998 | 41            |
| Jun 2012   | Jerseys           | 707          | 19926.250469999995 | 56            |
| Aug 2012   | Jerseys           | 332          | 9515.7725800000062 | 30            |
| Sep 2012   | Jerseys           | 475          | 13699.189999999995 | 57            |
| Oct 2012   | Jerseys           | 429          | 12193.759869999998 | 39            |
| Dec 2012   | Jerseys           | 260          | 7498.5040000000072 | 32            |
| Mar 2013   | Jerseys           | 437          | 12530.343760000007 | 57            |
| Apr 2013   | Jerseys           | 329          | 9453.55506         | 42            |
| May 2011   | Jerseys           | 84           | 2422.5936000000011 | 19            |
| Jul 2011   | Jerseys           | 199          | 5722.5308200000045 | 35            |
| Aug 2011   | Jerseys           | 243          | 7008.2172000000073 | 46            |
| Oct 2011   | Jerseys           | 457          | 13102.231039999991 | 76            |
| Jan 2012   | Jerseys           | 286          | 8248.3544000000074 | 49            |
| Feb 2012   | Jerseys           | 102          | 2941.7208000000019 | 21            |
| Mar 2012   | Jerseys           | 281          | 8017.3511600000056 | 44            |
| Apr 2012   | Jerseys           | 206          | 5941.1224000000057 | 36            |
| Jul 2012   | Jerseys           | 533          | 14728.401059999997 | 45            |
| Nov 2012   | Jerseys           | 166          | 4787.5064000000029 | 22            |
| Jan 2013   | Jerseys           | 206          | 5906.1858600000032 | 24            |
| Feb 2013   | Jerseys           | 325          | 9276.6719900000026 | 38            |
| Dec 2011   | Jerseys           | 44           | 1268.9776          | 12            |
| Jun 2012   | Locks             | 168          | 2511.31            | 33            |
| Aug 2012   | Locks             | 64           | 960.0              | 19            |
| Sep 2012   | Locks             | 108          | 1620.0             | 29            |
| Oct 2012   | Locks             | 80           | 1200.0             | 22            |
| Dec 2012   | Locks             | 49           | 735.0              | 14            |
| Jan 2013   | Locks             | 50           | 750.0              | 12            |
| Feb 2013   | Locks             | 70           | 1050.0             | 21            |
| Mar 2013   | Locks             | 101          | 1515.0             | 31            |
| Apr 2013   | Locks             | 92           | 1370.52            | 22            |
| Sep 2013   | Locks             | 1            | 15.0               | 1             |
| May 2012   | Locks             | 107          | 1587.62            | 21            |
| Jul 2012   | Locks             | 149          | 2205.77            | 24            |
| Nov 2012   | Locks             | 48           | 720.0              | 11            |
| Jun 2011   | Mountain Bikes    | 28           | 94874.720000000016 | 28            |
| Jul 2011   | Mountain Bikes    | 519          | 1092336.8843239988 | 64            |
| Aug 2011   | Mountain Bikes    | 570          | 1176243.3774759991 | 54            |
| Sep 2011   | Mountain Bikes    | 26           | 88024.74           | 26            |
| Oct 2011   | Mountain Bikes    | 1114         | 2303616.168851994  | 90            |
| Nov 2011   | Mountain Bikes    | 38           | 128549.62000000007 | 38            |
| Dec 2011   | Mountain Bikes    | 206          | 467503.62000000011 | 49            |
| Jan 2012   | Mountain Bikes    | 925          | 1908089.3659999925 | 80            |
| Feb 2012   | Mountain Bikes    | 229          | 514188.48200000031 | 48            |
| Mar 2012   | Mountain Bikes    | 631          | 1336661.2537919977 | 73            |
| Apr 2012   | Mountain Bikes    | 501          | 403252.24649999966 | 69            |
| May 2012   | Mountain Bikes    | 744          | 868578.56992399972 | 72            |
| May 2011   | Mountain Bikes    | 120          | 247664.26800000013 | 15            |
| Jun 2013   | Mountain Bikes    | 1328         | 1355023.3886889936 | 241           |
| Jul 2013   | Mountain Bikes    | 1295         | 1281345.1203669917 | 214           |
| Aug 2013   | Mountain Bikes    | 880          | 1003019.7319999965 | 246           |
| Sep 2013   | Mountain Bikes    | 1194         | 1244716.8356249924 | 263           |
| May 2013   | Mountain Bikes    | 824          | 862787.97722399945 | 165           |
| Oct 2013   | Mountain Bikes    | 1231         | 1338738.67399999   | 305           |
| Nov 2013   | Mountain Bikes    | 825          | 1042572.3512679967 | 374           |
| Dec 2013   | Mountain Bikes    | 1072         | 1258255.4668669929 | 340           |
| Jan 2014   | Mountain Bikes    | 1032         | 1257649.6974759907 | 360           |
| Feb 2014   | Mountain Bikes    | 292          | 519976.07999999792 | 292           |
| Mar 2014   | Mountain Bikes    | 1868         | 1922495.7967999843 | 418           |
| Apr 2014   | Mountain Bikes    | 404          | 745380.45999999694 | 404           |
| May 2014   | Mountain Bikes    | 1336         | 1578293.9871999912 | 483           |
| Jul 2012   | Mountain Bikes    | 1027         | 1144878.4680440016 | 101           |
| Aug 2012   | Mountain Bikes    | 619          | 673854.35371999978 | 71            |
| Feb 2013   | Mountain Bikes    | 733          | 847502.60287600034 | 119           |
| Mar 2013   | Mountain Bikes    | 922          | 1077264.5328840003 | 170           |
| Jun 2012   | Mountain Bikes    | 1077         | 1191170.7412840012 | 105           |
| Sep 2012   | Mountain Bikes    | 990          | 1117157.4338839995 | 116           |
| Oct 2012   | Mountain Bikes    | 817          | 903581.82729999966 | 92            |
| Dec 2012   | Mountain Bikes    | 876          | 1004535.0871519996 | 147           |
| Jan 2013   | Mountain Bikes    | 688          | 816655.51180000056 | 154           |
| Apr 2013   | Mountain Bikes    | 742          | 902992.51950000052 | 163           |
| Nov 2012   | Mountain Bikes    | 598          | 726011.97455199994 | 136           |
| May 2012   | Mountain Frames   | 345          | 153335.806876      | 26            |
| Oct 2012   | Mountain Frames   | 338          | 161590.58929999996 | 28            |
| Nov 2012   | Mountain Frames   | 114          | 44023.2746         | 19            |
| Feb 2013   | Mountain Frames   | 255          | 110060.32990800003 | 17            |
| Mar 2013   | Mountain Frames   | 395          | 176103.86560000002 | 29            |
| Apr 2013   | Mountain Frames   | 335          | 154048.24019999994 | 27            |
| Jun 2012   | Mountain Frames   | 544          | 243623.50920000026 | 44            |
| Jul 2012   | Mountain Frames   | 479          | 211336.92070000022 | 38            |
| Aug 2012   | Mountain Frames   | 251          | 103356.716576      | 18            |
| Sep 2012   | Mountain Frames   | 404          | 178595.3311        | 31            |
| Dec 2012   | Mountain Frames   | 244          | 108360.76280000001 | 37            |
| Jan 2013   | Mountain Frames   | 179          | 83686.316099999967 | 26            |
| Jul 2013   | Mountain Frames   | 968          | 312020.4731280001  | 43            |
| Jun 2013   | Mountain Frames   | 973          | 305923.499332      | 51            |
| Aug 2013   | Mountain Frames   | 357          | 118989.87307200007 | 22            |
| May 2013   | Mountain Frames   | 636          | 208815.67032       | 25            |
| Sep 2013   | Mountain Frames   | 662          | 224303.9760000002  | 34            |
| Oct 2013   | Mountain Frames   | 632          | 221830.44582800008 | 32            |
| Nov 2013   | Mountain Frames   | 204          | 61012.98599999999  | 17            |
| Dec 2013   | Mountain Frames   | 380          | 133706.64600000004 | 31            |
| Jan 2014   | Mountain Frames   | 375          | 115170.70800000006 | 31            |
| Mar 2014   | Mountain Frames   | 979          | 343024.626         | 52            |
| May 2014   | Mountain Frames   | 613          | 220929.06000000017 | 33            |
| Oct 2011   | Mountain Frames   | 249          | 187019.05049999984 | 38            |
| Feb 2012   | Mountain Frames   | 35           | 26304.695499999998 | 9             |
| Mar 2012   | Mountain Frames   | 133          | 99999.969899999924 | 22            |
| Apr 2012   | Mountain Frames   | 126          | 94630.499399999942 | 19            |
| Aug 2011   | Mountain Frames   | 138          | 103500.00869999995 | 19            |
| Jul 2011   | Mountain Frames   | 95           | 71435.07759999999  | 12            |
| Dec 2011   | Mountain Frames   | 15           | 11375.051700000002 | 3             |
| Jan 2012   | Mountain Frames   | 155          | 116182.59329999993 | 25            |
| May 2011   | Mountain Frames   | 13           | 9633.656           | 3             |
| May 2013   | Pedals            | 349          | 12733.475307999985 | 56            |
| Jun 2013   | Pedals            | 582          | 21697.838023999993 | 99            |
| Jul 2013   | Pedals            | 513          | 19728.521999999979 | 94            |
| Aug 2013   | Pedals            | 253          | 9274.601999999999  | 47            |
| Sep 2013   | Pedals            | 438          | 16565.111999999986 | 76            |
| Oct 2013   | Pedals            | 374          | 14060.975999999986 | 72            |
| Mar 2014   | Pedals            | 645          | 23910.638591999974 | 118           |
| May 2014   | Pedals            | 367          | 13728.305875999989 | 67            |
| Nov 2013   | Pedals            | 97           | 3599.0580000000014 | 26            |
| Dec 2013   | Pedals            | 168          | 6524.3520000000017 | 55            |
| Jan 2014   | Pedals            | 145          | 5661.0300000000007 | 46            |
| May 2012   | Pumps             | 122          | 1441.1590600000006 | 23            |
| Jun 2012   | Pumps             | 173          | 2066.7501080000011 | 30            |
| Jul 2012   | Pumps             | 137          | 1634.966108        | 25            |
| Aug 2012   | Pumps             | 65           | 779.61000000000047 | 21            |
| Sep 2012   | Pumps             | 114          | 1367.3160000000007 | 31            |
| Oct 2012   | Pumps             | 90           | 1079.46            | 22            |
| Nov 2012   | Pumps             | 53           | 635.682            | 12            |
| Dec 2012   | Pumps             | 39           | 467.76599999999996 | 13            |
| Jan 2013   | Pumps             | 55           | 659.67000000000019 | 12            |
| Feb 2013   | Pumps             | 81           | 971.51400000000035 | 24            |
| Mar 2013   | Pumps             | 125          | 1499.2500000000009 | 33            |
| Apr 2013   | Pumps             | 76           | 911.54400000000032 | 21            |
| May 2011   | Road Bikes        | 340          | 220044.86889999965 | 27            |
| Jul 2011   | Road Bikes        | 764          | 827108.32718000107 | 164           |
| Aug 2011   | Road Bikes        | 1111         | 1141346.2182199985 | 191           |
| Oct 2011   | Road Bikes        | 2073         | 1954537.6782079891 | 225           |
| Dec 2011   | Road Bikes        | 618          | 816421.02084000187 | 179           |
| Jan 2012   | Road Bikes        | 1882         | 1896783.7355999921 | 250           |
| Feb 2012   | Road Bikes        | 718          | 892185.75008000084 | 169           |
| Mar 2012   | Road Bikes        | 1291         | 1452696.3564599943 | 225           |
| Apr 2012   | Road Bikes        | 932          | 1078663.6281319999 | 193           |
| May 2012   | Road Bikes        | 1840         | 1614262.3530259982 | 206           |
| Aug 2012   | Road Bikes        | 1478         | 1165422.1286899962 | 197           |
| Jul 2012   | Road Bikes        | 1836         | 1544160.8480729968 | 252           |
| Oct 2012   | Road Bikes        | 1497         | 1189205.4558159963 | 202           |
| Nov 2012   | Road Bikes        | 1232         | 1000859.1190249964 | 240           |
| Dec 2012   | Road Bikes        | 2014         | 1594391.5006239938 | 209           |
| Jan 2013   | Road Bikes        | 1300         | 1067159.9054159964 | 223           |
| Feb 2013   | Road Bikes        | 1424         | 1114024.3853789961 | 189           |
| Mar 2013   | Road Bikes        | 2270         | 1800833.2717669935 | 244           |
| Jun 2012   | Road Bikes        | 2507         | 1986914.1759929962 | 257           |
| Sep 2012   | Road Bikes        | 2233         | 1778010.989023993  | 211           |
| Apr 2013   | Road Bikes        | 1501         | 1183349.0160419971 | 243           |
| May 2013   | Road Bikes        | 1175         | 1088422.8727059986 | 219           |
| Jun 2011   | Road Bikes        | 113          | 364036.1048000002  | 113           |
| Nov 2011   | Road Bikes        | 192          | 609290.20140000153 | 192           |
| Nov 2013   | Road Bikes        | 1070         | 1066177.6289079946 | 452           |
| Sep 2013   | Road Bikes        | 1398         | 1318888.686874995  | 319           |
| Jun 2013   | Road Bikes        | 1558         | 1458572.0982679962 | 283           |
| Dec 2013   | Road Bikes        | 1344         | 1284311.5583999937 | 371           |
| Jul 2013   | Road Bikes        | 1278         | 1188331.4060279964 | 274           |
| Aug 2013   | Road Bikes        | 1092         | 1031456.984359997  | 309           |
| Oct 2013   | Road Bikes        | 1288         | 1217739.1194059956 | 363           |
| Jan 2014   | Road Bikes        | 1185         | 1161599.4502549949 | 416           |
| Feb 2014   | Road Bikes        | 318          | 391068.64279999782 | 318           |
| Mar 2014   | Road Bikes        | 2371         | 2148706.2202929887 | 520           |
| Apr 2014   | Road Bikes        | 443          | 527137.54999999679 | 443           |
| May 2014   | Road Bikes        | 1379         | 1321269.1454179946 | 519           |
| Sep 2011   | Road Bikes        | 131          | 414049.10580000054 | 131           |
| Jun 2013   | Road Frames       | 621          | 287171.21639999928 | 62            |
| Jul 2013   | Road Frames       | 560          | 258149.55865599995 | 39            |
| Aug 2013   | Road Frames       | 203          | 77125.034800000038 | 22            |
| Mar 2014   | Road Frames       | 440          | 173866.07080000013 | 56            |
| Oct 2013   | Road Frames       | 247          | 99189.749999999913 | 40            |
| May 2012   | Road Frames       | 808          | 269761.04235199984 | 33            |
| Jun 2012   | Road Frames       | 1169         | 397298.53339999833 | 61            |
| Jul 2012   | Road Frames       | 840          | 290444.98485799966 | 48            |
| Aug 2012   | Road Frames       | 399          | 112326.40149999991 | 24            |
| Sep 2012   | Road Frames       | 684          | 201760.19729999997 | 49            |
| Oct 2012   | Road Frames       | 491          | 144002.82579999988 | 38            |
| Nov 2012   | Road Frames       | 186          | 44494.513100000004 | 12            |
| Dec 2012   | Road Frames       | 178          | 41904.0135         | 19            |
| Jan 2013   | Road Frames       | 183          | 46293.1435         | 24            |
| Feb 2013   | Road Frames       | 424          | 128223.37950000001 | 26            |
| Mar 2013   | Road Frames       | 639          | 187222.58220000009 | 47            |
| Apr 2013   | Road Frames       | 486          | 142199.84912800003 | 36            |
| May 2013   | Road Frames       | 478          | 210125.36645299982 | 26            |
| Sep 2013   | Road Frames       | 264          | 110277.03599999998 | 38            |
| Dec 2013   | Road Frames       | 96           | 29006.963999999996 | 15            |
| Jan 2014   | Road Frames       | 86           | 24665.153999999995 | 14            |
| Nov 2013   | Road Frames       | 76           | 23105.531999999996 | 12            |
| May 2014   | Road Frames       | 240          | 102948.01199999999 | 29            |
| Feb 2014   | Road Frames       | 7            | 2390.9308000000005 | 3             |
| Apr 2014   | Road Frames       | 2            | 713.796            | 1             |
| Aug 2011   | Road Frames       | 262          | 60817.121600000079 | 32            |
| Oct 2011   | Road Frames       | 512          | 118089.82059999996 | 57            |
| Feb 2012   | Road Frames       | 138          | 37318.952500000021 | 16            |
| Mar 2012   | Road Frames       | 297          | 71325.650900000037 | 32            |
| Apr 2012   | Road Frames       | 205          | 46666.802900000075 | 24            |
| May 2011   | Road Frames       | 101          | 21892.304399999986 | 14            |
| Jul 2011   | Road Frames       | 204          | 43087.939400000076 | 30            |
| Jan 2012   | Road Frames       | 169          | 35163.1104         | 29            |
| Dec 2011   | Road Frames       | 58           | 12323.009999999997 | 8             |
| May 2013   | Saddles           | 215          | 5298.9861119999978 | 42            |
| Jun 2013   | Saddles           | 379          | 10038.012000000004 | 88            |
| Jul 2013   | Saddles           | 375          | 9565.8239999999932 | 92            |
| Aug 2013   | Saddles           | 104          | 2687.7149359999989 | 23            |
| Sep 2013   | Saddles           | 170          | 4250.052           | 39            |
| Oct 2013   | Saddles           | 226          | 5991.3741119999977 | 56            |
| Mar 2014   | Saddles           | 291          | 7581.4010880000005 | 74            |
| May 2014   | Saddles           | 208          | 5416.9559999999965 | 58            |
| Dec 2013   | Saddles           | 61           | 1716.024           | 22            |
| Nov 2013   | Saddles           | 41           | 1157.244           | 11            |
| Jan 2014   | Saddles           | 75           | 2125.7999999999997 | 21            |
| May 2012   | Shorts            | 234          | 8337.2902199999971 | 30            |
| Jun 2012   | Shorts            | 244          | 8757.8921079999982 | 32            |
| Jul 2012   | Shorts            | 254          | 9032.250374        | 27            |
| Aug 2012   | Shorts            | 188          | 6766.8720000000012 | 22            |
| Sep 2012   | Shorts            | 241          | 8674.5539999999964 | 36            |
| Oct 2012   | Shorts            | 217          | 7692.7096680000013 | 25            |
| Nov 2012   | Shorts            | 99           | 3563.4059999999995 | 19            |
| Dec 2012   | Shorts            | 109          | 3923.3460000000005 | 23            |
| Jan 2013   | Shorts            | 104          | 3743.3760000000011 | 18            |
| Feb 2013   | Shorts            | 175          | 6253.453583999998  | 24            |
| Mar 2013   | Shorts            | 232          | 8350.608           | 34            |
| Apr 2013   | Shorts            | 189          | 6802.8659999999991 | 25            |
| Jul 2013   | Shorts            | 930          | 38209.574851999976 | 125           |
| May 2013   | Shorts            | 440          | 16369.422177000004 | 21            |
| Jun 2013   | Shorts            | 872          | 33721.706925000006 | 52            |
| Aug 2013   | Shorts            | 548          | 23835.990372000029 | 116           |
| Sep 2013   | Shorts            | 713          | 30568.71341700001  | 130           |
| Oct 2013   | Shorts            | 661          | 28533.348225000045 | 122           |
| Mar 2014   | Shorts            | 1187         | 49009.573632       | 167           |
| May 2014   | Shorts            | 680          | 30612.359181000029 | 145           |
| Dec 2013   | Shorts            | 512          | 23252.371758000027 | 131           |
| Jan 2014   | Shorts            | 536          | 24469.679832000042 | 125           |
| Nov 2013   | Shorts            | 385          | 17931.31901699998  | 112           |
| Feb 2014   | Shorts            | 77           | 5389.22999999999   | 77            |
| Apr 2014   | Shorts            | 81           | 5669.1899999999887 | 81            |
| Jun 2014   | Shorts            | 59           | 4129.4099999999935 | 59            |
| Jun 2013   | Socks             | 528          | 2767.154364        | 50            |
| Jul 2013   | Socks             | 412          | 2315.7736560000012 | 78            |
| Aug 2013   | Socks             | 274          | 1600.0473920000011 | 72            |
| Mar 2014   | Socks             | 594          | 3262.1473599999963 | 109           |
| May 2013   | Socks             | 266          | 1393.5560820000007 | 25            |
| Sep 2013   | Socks             | 430          | 2438.9627270000005 | 88            |
| Oct 2013   | Socks             | 358          | 2076.2611770000017 | 86            |
| Nov 2013   | Socks             | 174          | 1104.0619000000006 | 67            |
| Dec 2013   | Socks             | 282          | 1708.5710760000013 | 87            |
| Jan 2014   | Socks             | 236          | 1430.1130180000011 | 79            |
| May 2014   | Socks             | 353          | 2059.2206320000014 | 88            |
| Feb 2014   | Socks             | 38           | 341.62000000000018 | 38            |
| Apr 2014   | Socks             | 50           | 449.50000000000028 | 50            |
| Jun 2014   | Socks             | 25           | 224.75000000000006 | 25            |
| Jul 2011   | Socks             | 147          | 785.38874999999985 | 21            |
| Aug 2011   | Socks             | 186          | 1003.3092500000004 | 25            |
| Oct 2011   | Socks             | 253          | 1400.0919500000011 | 46            |
| Dec 2011   | Socks             | 45           | 256.50000000000006 | 10            |
| Jan 2012   | Socks             | 181          | 1009.3056499999996 | 36            |
| Feb 2012   | Socks             | 84           | 461.94509999999991 | 10            |
| Mar 2012   | Socks             | 149          | 836.04750000000035 | 27            |
| Apr 2012   | Socks             | 109          | 575.69999999999993 | 17            |
| May 2011   | Socks             | 43           | 245.1              | 9             |
| May 2012   | Tights            | 425          | 18477.640986       | 33            |
| Jun 2012   | Tights            | 618          | 27091.412329999996 | 55            |
| Jul 2012   | Tights            | 479          | 20751.420274999997 | 39            |
| Aug 2012   | Tights            | 365          | 16207.168755999994 | 33            |
| Sep 2012   | Tights            | 511          | 22795.250228000004 | 54            |
| Feb 2013   | Tights            | 314          | 14047.546744000001 | 40            |
| Nov 2012   | Tights            | 174          | 7828.9559999999974 | 22            |
| Dec 2012   | Tights            | 219          | 9853.6859999999942 | 33            |
| Jan 2013   | Tights            | 222          | 9859.062783        | 25            |
| Mar 2013   | Tights            | 435          | 19467.778950000022 | 59            |
| Apr 2013   | Tights            | 406          | 17880.045675999998 | 40            |
| Jun 2013   | Tights            | 9            | 438.6915           | 1             |
| Jul 2013   | Tights            | 7            | 341.2045           | 1             |
| Oct 2012   | Tights            | 394          | 17573.036615999998 | 37            |
| Apr 2014   | Tights            | 2            | 97.487             | 1             |
| Sep 2013   | Tights            | 5            | 243.71749999999997 | 1             |
| Mar 2014   | Tights            | 4            | 194.974            | 1             |
| May 2013   | Tires and Tubes   | 97           | 348.01600000000008 | 28            |
| Jun 2013   | Tires and Tubes   | 368          | 4388.5656319999871 | 171           |
| Jul 2013   | Tires and Tubes   | 1424         | 18931.415999999972 | 792           |
| Aug 2013   | Tires and Tubes   | 1451         | 19950.401999999915 | 801           |
| Sep 2013   | Tires and Tubes   | 1420         | 18561.387999999981 | 787           |
| Mar 2014   | Tires and Tubes   | 1755         | 22999.197999999971 | 931           |
| Oct 2013   | Tires and Tubes   | 1531         | 21152.65599999989  | 849           |
| Nov 2013   | Tires and Tubes   | 1479         | 20956.05999999991  | 832           |
| Dec 2013   | Tires and Tubes   | 1516         | 21447.467999999892 | 846           |
| Jan 2014   | Tires and Tubes   | 1492         | 21111.629999999888 | 866           |
| Feb 2014   | Tires and Tubes   | 1291         | 18560.040000000037 | 761           |
| Apr 2014   | Tires and Tubes   | 1553         | 22315.909999999985 | 882           |
| May 2014   | Tires and Tubes   | 1633         | 22039.507999999983 | 911           |
| Jun 2014   | Tires and Tubes   | 996          | 13692.269999999948 | 573           |
| May 2013   | Touring Bikes     | 811          | 550625.84921700018 | 31            |
| Jun 2013   | Touring Bikes     | 1465         | 1132492.6258289998 | 122           |
| Jul 2013   | Touring Bikes     | 1683         | 1210589.8623180005 | 150           |
| Aug 2013   | Touring Bikes     | 803          | 808936.5329939971  | 137           |
| Sep 2013   | Touring Bikes     | 1238         | 1214628.9048989993 | 171           |
| Oct 2013   | Touring Bikes     | 1522         | 1497460.966236     | 221           |
| Nov 2013   | Touring Bikes     | 785          | 902005.31440799474 | 261           |
| Dec 2013   | Touring Bikes     | 1009         | 1099947.658655996  | 229           |
| Jan 2014   | Touring Bikes     | 1449         | 1473593.3032680003 | 262           |
| Mar 2014   | Touring Bikes     | 1952         | 1973165.6295180037 | 305           |
| May 2014   | Touring Bikes     | 1606         | 1665519.4317960022 | 328           |
| Feb 2014   | Touring Bikes     | 189          | 343276.95000000024 | 189           |
| Apr 2014   | Touring Bikes     | 239          | 424048.23000000016 | 239           |
| Jul 2013   | Touring Frames    | 642          | 275512.52597800008 | 44            |
| Jun 2013   | Touring Frames    | 573          | 237300.01451999974 | 33            |
| Aug 2013   | Touring Frames    | 165          | 75302.784884000022 | 9             |
| Sep 2013   | Touring Frames    | 337          | 154334.6393279999  | 21            |
| Oct 2013   | Touring Frames    | 283          | 137878.104         | 28            |
| Mar 2014   | Touring Frames    | 552          | 246806.36999999965 | 36            |
| May 2014   | Touring Frames    | 373          | 181629.5999999998  | 41            |
| May 2013   | Touring Frames    | 342          | 151825.97547499993 | 17            |
| Nov 2013   | Touring Frames    | 110          | 45338.772000000048 | 9             |
| Dec 2013   | Touring Frames    | 199          | 82051.218000000037 | 18            |
| Jan 2014   | Touring Frames    | 149          | 54347.682000000066 | 16            |
| Jun 2013   | Vests             | 818          | 29613.440900000005 | 79            |
| Jul 2013   | Vests             | 769          | 28444.101100000018 | 87            |
| Aug 2013   | Vests             | 475          | 17854.30135        | 57            |
| Sep 2013   | Vests             | 623          | 24100.466150000007 | 102           |
| Oct 2013   | Vests             | 611          | 23255.738350000003 | 93            |
| Nov 2013   | Vests             | 315          | 12937.235999999999 | 75            |
| Dec 2013   | Vests             | 370          | 15209.202500000001 | 99            |
| Jan 2014   | Vests             | 404          | 16415.670749999997 | 102           |
| Feb 2014   | Vests             | 50           | 3175.0             | 50            |
| Mar 2014   | Vests             | 1051         | 40115.274100000017 | 149           |
| Apr 2014   | Vests             | 55           | 3492.5             | 55            |
| May 2014   | Vests             | 610          | 23640.707099999996 | 103           |
| Jun 2014   | Vests             | 31           | 1968.5             | 31            |
| May 2013   | Vests             | 556          | 19266.230199999995 | 27            |
| Jun 2012   | Wheels            | 958          | 124399.93027400006 | 103           |
| Dec 2012   | Wheels            | 195          | 22941.236999999983 | 53            |
| May 2012   | Wheels            | 521          | 69155.808492000026 | 54            |
| Jul 2012   | Wheels            | 724          | 94644.497899999973 | 85            |
| Aug 2012   | Wheels            | 340          | 43781.999999999985 | 45            |
| Sep 2012   | Wheels            | 552          | 71073.44400000009  | 75            |
| Oct 2012   | Wheels            | 383          | 49066.443000000028 | 55            |
| Nov 2012   | Wheels            | 129          | 17155.502999999993 | 28            |
| Jan 2013   | Wheels            | 184          | 23089.088999999993 | 47            |
| Feb 2013   | Wheels            | 362          | 45116.976          | 44            |
| Mar 2013   | Wheels            | 495          | 63931.779000000046 | 74            |
| Apr 2013   | Wheels            | 420          | 54713.357595       | 53            |
| Jun 2013   | Wheels            | 3            | 450.90790000000004 | 1             |
| Jul 2013   | Wheels            | 4            | 698.634            | 1             |
| Sep 2013   | Wheels            | 1            | 83.2981            | 1             |
| May 2013   | Wheels            | 2            | 528.4488           | 1             |

**4.10. Calculate % YoY growth rate by SubCategory & release top 3 cat with highest grow rate.**
```s
with add_prv_qty as
(select *,
      lead(qty_item) over (partition by Name ORDER BY period desc) as Prv_qty
from (SELECT  extract(year from a.ModifiedDate) as period,
        c.Name,
        sum(a.OrderQty) as qty_item
FROM `adventureworks2019.Sales.SalesOrderDetail` as a
left join `adventureworks2019.Production.Product` as b on a.ProductID=b.ProductID
left join `adventureworks2019.Production.ProductSubcategory` as c on cast(b.ProductSubcategoryID as INT)=c.ProductSubcategoryID
group by 	period, Name
order by name)
)

select name,
      qty_item,
      Prv_qty,
      round((qty_item-Prv_qty)/Prv_qty,2) as Qty_diff
from add_prv_qty
where Prv_qty is not null
order by Qty_diff desc
limit 3;
```
| **name**        | **qty_item** | **Prv_qty** | **Qty_diff** |
|-----------------|--------------|-------------|--------------|
| Mountain Frames | 3168         | 510         | 5.21         |
| Socks           | 2724         | 523         | 4.21         |
| Road Frames     | 5564         | 1137        | 3.89         |

**4.11. Ranking Top 3 TeritoryID with biggest Order quantity of every year.**

```s
with order_qty as
(select *,
      Dense_rank () over (partition by yr order by order_cnt desc) as rk
from (select extract(year from a.ModifiedDate) as yr,
        TerritoryID,
        sum(OrderQty) as order_cnt,
FROM `adventureworks2019.Sales.SalesOrderDetail` as a
left join `adventureworks2019.Sales.SalesOrderHeader` as b on a.SalesOrderID=b.SalesOrderID
group by yr, TerritoryID)
)

select *
from order_qty
where rk <= 3;
```
| **yr** | **TerritoryID** | **order_cnt** | **rk** |
|--------|-----------------|---------------|--------|
| 2012   | 4               | 17553         | 1      |
| 2012   | 6               | 14412         | 2      |
| 2012   | 1               | 8537          | 3      |
| 2013   | 4               | 26682         | 1      |
| 2013   | 6               | 22553         | 2      |
| 2013   | 1               | 17452         | 3      |
| 2014   | 4               | 11632         | 1      |
| 2014   | 6               | 9711          | 2      |
| 2014   | 1               | 8823          | 3      |
| 2011   | 4               | 3238          | 1      |
| 2011   | 6               | 2705          | 2      |
| 2011   | 1               | 1964          | 3      |

**4.12. Calculate Total Discount Cost belongs to Seasonal Discount for each SubCategory.**

```s
with order_info as
(select  extract(month from ModifiedDate) as mth,
        extract(year from ModifiedDate) as yr,
        CustomerID
from `adventureworks2019.Sales.SalesOrderHeader`
where format_datetime('%Y', ModifiedDate) = '2014' and status = 5
)

, first_order as 
(select distinct mth as mth_join, yr, customerID
from (select *,
        row_number() over (partition by CustomerID order by mth) as rn
from order_info)
where rn = 1
)

select  distinct mth_join,
        mth_diff,
        count(distinct customerID) as customer_cnt
from (
select  mth, a.yr, mth_join, a.CustomerID,
        concat('M',mth-mth_join) as mth_diff
from order_info as a 
left join first_order as b on a.CustomerID=b.CustomerID
order by a.CustomerID
)
group by mth_join, mth_diff
order by mth_join, mth_diff;
```
| **mth_join** | **mth_diff** | **customer_cnt** |
|--------------|--------------|------------------|
| 1            | M0           | 2076             |
| 1            | M1           | 78               |
| 1            | M2           | 89               |
| 1            | M3           | 252              |
| 1            | M4           | 96               |
| 1            | M5           | 61               |
| 1            | M6           | 18               |
| 2            | M0           | 1805             |
| 2            | M1           | 51               |
| 2            | M2           | 61               |
| 2            | M3           | 234              |
| 2            | M4           | 58               |
| 2            | M5           | 8                |
| 3            | M0           | 1918             |
| 3            | M1           | 43               |
| 3            | M2           | 58               |
| 3            | M3           | 44               |
| 3            | M4           | 11               |
| 4            | M0           | 1906             |
| 4            | M1           | 34               |
| 4            | M2           | 44               |
| 4            | M3           | 7                |
| 5            | M0           | 1947             |
| 5            | M1           | 40               |
| 5            | M2           | 7                |
| 6            | M0           | 909              |
| 6            | M1           | 10               |
| 7            | M0           | 148              |

**4.13. Trend of Stock level & MoM diff % by all product in 2011**

```s
with add_prv_qty as
(select *,
      lead(stock_prv) over (partition by Name ORDER BY mth desc) as Prv_qty,
from 
(SELECT  b.name,
        extract(month from a.ModifiedDate) as mth,
        extract(year from a.ModifiedDate) as yr,
        sum(StockedQty) as stock_prv
FROM `adventureworks2019.Production.WorkOrder` as a
left join `adventureworks2019.Production.Product` as b on a.ProductID=b.ProductID
where format_datetime('%Y', a.ModifiedDate) = '2011'
group by b.name, mth, yr)
order by name, mth desc
)
select *,
      case when round((stock_prv-Prv_qty)*100/Prv_qty,1) is null then 0
            else round((stock_prv-Prv_qty)*100/Prv_qty,1) end as diff
from add_prv_qty;
```
| **name**                         | **mth** | **yr** | **stock_prv** | **Prv_qty** | **diff** |
|----------------------------------|---------|--------|---------------|-------------|----------|
| BB Ball Bearing                  | 12      | 2011   | 8475          | 14544       | -41.7    |
| BB Ball Bearing                  | 11      | 2011   | 14544         | 19175       | -24.2    |
| BB Ball Bearing                  | 10      | 2011   | 19175         | 8845        | 116.8    |
| BB Ball Bearing                  | 9       | 2011   | 8845          | 9666        | -8.5     |
| BB Ball Bearing                  | 8       | 2011   | 9666          | 12837       | -24.7    |
| BB Ball Bearing                  | 7       | 2011   | 12837         | 5259        | 144.1    |
| BB Ball Bearing                  | 6       | 2011   | 5259          |             | 0.0      |
| Blade                            | 12      | 2011   | 1842          | 3598        | -48.8    |
| Blade                            | 11      | 2011   | 3598          | 4670        | -23.0    |
| Blade                            | 10      | 2011   | 4670          | 2122        | 120.1    |
| Blade                            | 9       | 2011   | 2122          | 2382        | -10.9    |
| Blade                            | 8       | 2011   | 2382          | 3166        | -24.8    |
| Blade                            | 7       | 2011   | 3166          | 1280        | 147.3    |
| Blade                            | 6       | 2011   | 1280          |             | 0.0      |
| Chain Stays                      | 12      | 2011   | 1842          | 3598        | -48.8    |
| Chain Stays                      | 11      | 2011   | 3598          | 4670        | -23.0    |
| Chain Stays                      | 10      | 2011   | 4670          | 2122        | 120.1    |
| Chain Stays                      | 9       | 2011   | 2122          | 2341        | -9.4     |
| Chain Stays                      | 8       | 2011   | 2341          | 3166        | -26.1    |
| Chain Stays                      | 7       | 2011   | 3166          | 1280        | 147.3    |
| Chain Stays                      | 6       | 2011   | 1280          |             | 0.0      |
| Down Tube                        | 12      | 2011   | 921           | 1799        | -48.8    |
| Down Tube                        | 11      | 2011   | 1799          | 2335        | -23.0    |
| Down Tube                        | 10      | 2011   | 2335          | 1061        | 120.1    |
| Down Tube                        | 9       | 2011   | 1061          | 1191        | -10.9    |
| Down Tube                        | 8       | 2011   | 1191          | 1541        | -22.7    |
| Down Tube                        | 7       | 2011   | 1541          | 640         | 140.8    |
| Down Tube                        | 6       | 2011   | 640           |             | 0.0      |
| Fork Crown                       | 12      | 2011   | 921           | 1799        | -48.8    |
| Fork Crown                       | 11      | 2011   | 1799          | 2335        | -23.0    |
| Fork Crown                       | 10      | 2011   | 2335          | 1061        | 120.1    |
| Fork Crown                       | 9       | 2011   | 1061          | 1191        | -10.9    |
| Fork Crown                       | 8       | 2011   | 1191          | 1583        | -24.8    |
| Fork Crown                       | 7       | 2011   | 1583          | 640         | 147.3    |
| Fork Crown                       | 6       | 2011   | 640           |             | 0.0      |
| Fork End                         | 12      | 2011   | 1842          | 3598        | -48.8    |
| Fork End                         | 11      | 2011   | 3598          | 4670        | -23.0    |
| Fork End                         | 10      | 2011   | 4670          | 2122        | 120.1    |
| Fork End                         | 9       | 2011   | 2122          | 2382        | -10.9    |
| Fork End                         | 8       | 2011   | 2382          | 3166        | -24.8    |
| Fork End                         | 7       | 2011   | 3166          | 1280        | 147.3    |
| Fork End                         | 6       | 2011   | 1280          |             | 0.0      |
| Front Derailleur                 | 12      | 2011   | 861           | 1440        | -40.2    |
| Front Derailleur                 | 11      | 2011   | 1440          | 1918        | -24.9    |
| Front Derailleur                 | 10      | 2011   | 1918          | 874         | 119.5    |
| Front Derailleur                 | 9       | 2011   | 874           | 969         | -9.8     |
| Front Derailleur                 | 8       | 2011   | 969           | 1257        | -22.9    |
| Front Derailleur                 | 7       | 2011   | 1257          | 501         | 150.9    |
| Front Derailleur                 | 6       | 2011   | 501           |             | 0.0      |
| HL Bottom Bracket                | 12      | 2011   | 426           | 719         | -40.8    |
| HL Bottom Bracket                | 11      | 2011   | 719           | 938         | -23.3    |
| HL Bottom Bracket                | 10      | 2011   | 938           | 395         | 137.5    |
| HL Bottom Bracket                | 9       | 2011   | 395           | 534         | -26.0    |
| HL Bottom Bracket                | 8       | 2011   | 534           | 670         | -20.3    |
| HL Bottom Bracket                | 7       | 2011   | 670           | 176         | 280.7    |
| HL Bottom Bracket                | 6       | 2011   | 176           |             | 0.0      |
| HL Crankset                      | 12      | 2011   | 426           | 719         | -40.8    |
| HL Crankset                      | 11      | 2011   | 719           | 938         | -23.3    |
| HL Crankset                      | 10      | 2011   | 938           | 395         | 137.5    |
| HL Crankset                      | 9       | 2011   | 395           | 548         | -27.9    |
| HL Crankset                      | 8       | 2011   | 548           | 670         | -18.2    |
| HL Crankset                      | 7       | 2011   | 670           | 176         | 280.7    |
| HL Crankset                      | 6       | 2011   | 176           |             | 0.0      |
| HL Fork                          | 12      | 2011   | 441           | 856         | -48.5    |
| HL Fork                          | 11      | 2011   | 856           | 1064        | -19.5    |
| HL Fork                          | 10      | 2011   | 1064          | 441         | 141.3    |
| HL Fork                          | 9       | 2011   | 441           | 650         | -32.2    |
| HL Fork                          | 8       | 2011   | 650           | 753         | -13.7    |
| HL Fork                          | 7       | 2011   | 753           | 189         | 298.4    |
| HL Fork                          | 6       | 2011   | 189           |             | 0.0      |
| HL Headset                       | 12      | 2011   | 426           | 719         | -40.8    |
| HL Headset                       | 11      | 2011   | 719           | 938         | -23.3    |
| HL Headset                       | 10      | 2011   | 938           | 395         | 137.5    |
| HL Headset                       | 9       | 2011   | 395           | 548         | -27.9    |
| HL Headset                       | 8       | 2011   | 548           | 670         | -18.2    |
| HL Headset                       | 7       | 2011   | 670           | 176         | 280.7    |
| HL Headset                       | 6       | 2011   | 176           |             | 0.0      |
| HL Hub                           | 12      | 2011   | 1044          | 1886        | -44.6    |
| HL Hub                           | 11      | 2011   | 1886          | 2454        | -23.1    |
| HL Hub                           | 10      | 2011   | 2454          | 1032        | 137.8    |
| HL Hub                           | 9       | 2011   | 1032          | 1346        | -23.3    |
| HL Hub                           | 8       | 2011   | 1346          | 1648        | -18.3    |
| HL Hub                           | 7       | 2011   | 1648          | 594         | 177.4    |
| HL Hub                           | 6       | 2011   | 594           |             | 0.0      |
| "HL Mountain Frame - Black, 38"  | 12      | 2011   | 31            | 90          | -65.6    |
| "HL Mountain Frame - Black, 38"  | 11      | 2011   | 90            | 94          | -4.3     |
| "HL Mountain Frame - Black, 38"  | 10      | 2011   | 94            | 34          | 176.5    |
| "HL Mountain Frame - Black, 38"  | 9       | 2011   | 34            | 69          | -50.7    |
| "HL Mountain Frame - Black, 38"  | 8       | 2011   | 69            | 85          | -18.8    |
| "HL Mountain Frame - Black, 38"  | 7       | 2011   | 85            | 26          | 226.9    |
| "HL Mountain Frame - Black, 38"  | 6       | 2011   | 26            |             | 0.0      |
| "HL Mountain Frame - Black, 42"  | 12      | 2011   | 26            | 90          | -71.1    |
| "HL Mountain Frame - Black, 42"  | 11      | 2011   | 90            | 96          | -6.3     |
| "HL Mountain Frame - Black, 42"  | 10      | 2011   | 96            | 40          | 140.0    |
| "HL Mountain Frame - Black, 42"  | 9       | 2011   | 40            | 64          | -37.5    |
| "HL Mountain Frame - Black, 42"  | 8       | 2011   | 64            | 91          | -29.7    |
| "HL Mountain Frame - Black, 42"  | 7       | 2011   | 91            | 17          | 435.3    |
| "HL Mountain Frame - Black, 42"  | 6       | 2011   | 17            |             | 0.0      |
| "HL Mountain Frame - Black, 44"  | 12      | 2011   | 29            | 72          | -59.7    |
| "HL Mountain Frame - Black, 44"  | 11      | 2011   | 72            | 100         | -28.0    |
| "HL Mountain Frame - Black, 44"  | 10      | 2011   | 100           | 28          | 257.1    |
| "HL Mountain Frame - Black, 44"  | 9       | 2011   | 28            | 60          | -53.3    |
| "HL Mountain Frame - Black, 44"  | 8       | 2011   | 60            | 64          | -6.3     |
| "HL Mountain Frame - Black, 44"  | 7       | 2011   | 64            | 26          | 146.2    |
| "HL Mountain Frame - Black, 44"  | 6       | 2011   | 26            |             | 0.0      |
| "HL Mountain Frame - Black, 48"  | 12      | 2011   | 27            | 76          | -64.5    |
| "HL Mountain Frame - Black, 48"  | 11      | 2011   | 76            | 96          | -20.8    |
| "HL Mountain Frame - Black, 48"  | 10      | 2011   | 96            | 26          | 269.2    |
| "HL Mountain Frame - Black, 48"  | 9       | 2011   | 26            | 61          | -57.4    |
| "HL Mountain Frame - Black, 48"  | 8       | 2011   | 61            | 83          | -26.5    |
| "HL Mountain Frame - Black, 48"  | 7       | 2011   | 83            | 22          | 277.3    |
| "HL Mountain Frame - Black, 48"  | 6       | 2011   | 22            |             | 0.0      |
| "HL Mountain Frame - Silver, 38" | 12      | 2011   | 32            | 77          | -58.4    |
| "HL Mountain Frame - Silver, 38" | 11      | 2011   | 77            | 112         | -31.3    |
| "HL Mountain Frame - Silver, 38" | 10      | 2011   | 112           | 33          | 239.4    |
| "HL Mountain Frame - Silver, 38" | 9       | 2011   | 33            | 73          | -54.8    |
| "HL Mountain Frame - Silver, 38" | 8       | 2011   | 73            | 96          | -24.0    |
| "HL Mountain Frame - Silver, 38" | 7       | 2011   | 96            | 13          | 638.5    |
| "HL Mountain Frame - Silver, 38" | 6       | 2011   | 13            |             | 0.0      |
| "HL Mountain Frame - Silver, 42" | 12      | 2011   | 16            | 52          | -69.2    |
| "HL Mountain Frame - Silver, 42" | 11      | 2011   | 52            | 97          | -46.4    |
| "HL Mountain Frame - Silver, 42" | 10      | 2011   | 97            | 29          | 234.5    |
| "HL Mountain Frame - Silver, 42" | 9       | 2011   | 29            | 49          | -40.8    |
| "HL Mountain Frame - Silver, 42" | 8       | 2011   | 49            | 59          | -16.9    |
| "HL Mountain Frame - Silver, 42" | 7       | 2011   | 59            | 5           | 1080.0   |
| "HL Mountain Frame - Silver, 42" | 6       | 2011   | 5             |             | 0.0      |
| "HL Mountain Frame - Silver, 44" | 12      | 2011   | 33            | 65          | -49.2    |
| "HL Mountain Frame - Silver, 44" | 11      | 2011   | 65            | 71          | -8.5     |
| "HL Mountain Frame - Silver, 44" | 10      | 2011   | 71            | 24          | 195.8    |
| "HL Mountain Frame - Silver, 44" | 9       | 2011   | 24            | 50          | -52.0    |
| "HL Mountain Frame - Silver, 44" | 8       | 2011   | 50            | 59          | -15.3    |
| "HL Mountain Frame - Silver, 44" | 7       | 2011   | 59            | 21          | 181.0    |
| "HL Mountain Frame - Silver, 44" | 6       | 2011   | 21            |             | 0.0      |
| "HL Mountain Frame - Silver, 46" | 12      | 2011   | 3             | 15          | -80.0    |
| "HL Mountain Frame - Silver, 46" | 11      | 2011   | 15            | 17          | -11.8    |
| "HL Mountain Frame - Silver, 46" | 10      | 2011   | 17            | 3           | 466.7    |
| "HL Mountain Frame - Silver, 46" | 9       | 2011   | 3             | 18          | -83.3    |
| "HL Mountain Frame - Silver, 46" | 8       | 2011   | 18            | 15          | 20.0     |
| "HL Mountain Frame - Silver, 46" | 7       | 2011   | 15            | 3           | 400.0    |
| "HL Mountain Frame - Silver, 46" | 6       | 2011   | 3             |             | 0.0      |
| "HL Mountain Frame - Silver, 48" | 12      | 2011   | 27            | 78          | -65.4    |
| "HL Mountain Frame - Silver, 48" | 11      | 2011   | 78            | 90          | -13.3    |
| "HL Mountain Frame - Silver, 48" | 10      | 2011   | 90            | 30          | 200.0    |
| "HL Mountain Frame - Silver, 48" | 9       | 2011   | 30            | 57          | -47.4    |
| "HL Mountain Frame - Silver, 48" | 8       | 2011   | 57            | 58          | -1.7     |
| "HL Mountain Frame - Silver, 48" | 7       | 2011   | 58            | 10          | 480.0    |
| "HL Mountain Frame - Silver, 48" | 6       | 2011   | 10            |             | 0.0      |
| HL Mountain Front Wheel          | 12      | 2011   | 209           | 483         | -56.7    |
| HL Mountain Front Wheel          | 11      | 2011   | 483           | 659         | -26.7    |
| HL Mountain Front Wheel          | 10      | 2011   | 659           | 208         | 216.8    |
| HL Mountain Front Wheel          | 9       | 2011   | 208           | 396         | -47.5    |
| HL Mountain Front Wheel          | 8       | 2011   | 396           | 520         | -23.8    |
| HL Mountain Front Wheel          | 7       | 2011   | 520           | 124         | 319.4    |
| HL Mountain Front Wheel          | 6       | 2011   | 124           |             | 0.0      |
| HL Mountain Handlebars           | 12      | 2011   | 204           | 483         | -57.8    |
| HL Mountain Handlebars           | 11      | 2011   | 483           | 659         | -26.7    |
| HL Mountain Handlebars           | 10      | 2011   | 659           | 203         | 224.6    |
| HL Mountain Handlebars           | 9       | 2011   | 203           | 396         | -48.7    |
| HL Mountain Handlebars           | 8       | 2011   | 396           | 520         | -23.8    |
| HL Mountain Handlebars           | 7       | 2011   | 520           | 121         | 329.8    |
| HL Mountain Handlebars           | 6       | 2011   | 121           |             | 0.0      |
| HL Mountain Rear Wheel           | 12      | 2011   | 209           | 483         | -56.7    |
| HL Mountain Rear Wheel           | 11      | 2011   | 483           | 659         | -26.7    |
| HL Mountain Rear Wheel           | 10      | 2011   | 659           | 208         | 216.8    |
| HL Mountain Rear Wheel           | 9       | 2011   | 208           | 396         | -47.5    |
| HL Mountain Rear Wheel           | 8       | 2011   | 396           | 520         | -23.8    |
| HL Mountain Rear Wheel           | 7       | 2011   | 520           | 124         | 319.4    |
| HL Mountain Rear Wheel           | 6       | 2011   | 124           |             | 0.0      |
| HL Mountain Seat Assembly        | 12      | 2011   | 204           | 483         | -57.8    |
| HL Mountain Seat Assembly        | 11      | 2011   | 483           | 659         | -26.7    |
| HL Mountain Seat Assembly        | 10      | 2011   | 659           | 202         | 226.2    |
| HL Mountain Seat Assembly        | 9       | 2011   | 202           | 396         | -49.0    |
| HL Mountain Seat Assembly        | 8       | 2011   | 396           | 520         | -23.8    |
| HL Mountain Seat Assembly        | 7       | 2011   | 520           | 124         | 319.4    |
| HL Mountain Seat Assembly        | 6       | 2011   | 124           |             | 0.0      |
| "HL Road Frame - Red, 44"        | 12      | 2011   | 33            | 53          | -37.7    |
| "HL Road Frame - Red, 44"        | 11      | 2011   | 53            | 41          | 29.3     |
| "HL Road Frame - Red, 44"        | 10      | 2011   | 41            | 42          | -2.4     |
| "HL Road Frame - Red, 44"        | 9       | 2011   | 42            | 19          | 121.1    |
| "HL Road Frame - Red, 44"        | 8       | 2011   | 19            | 18          | 5.6      |
| "HL Road Frame - Red, 44"        | 7       | 2011   | 18            | 14          | 28.6     |
| "HL Road Frame - Red, 44"        | 6       | 2011   | 14            |             | 0.0      |
| "HL Road Frame - Red, 48"        | 12      | 2011   | 41            | 47          | -12.8    |
| "HL Road Frame - Red, 48"        | 11      | 2011   | 47            | 45          | 4.4      |
| "HL Road Frame - Red, 48"        | 10      | 2011   | 45            | 36          | 25.0     |
| "HL Road Frame - Red, 48"        | 9       | 2011   | 36            | 18          | 100.0    |
| "HL Road Frame - Red, 48"        | 8       | 2011   | 18            | 32          | -43.8    |
| "HL Road Frame - Red, 48"        | 7       | 2011   | 32            | 10          | 220.0    |
| "HL Road Frame - Red, 48"        | 6       | 2011   | 10            |             | 0.0      |
| "HL Road Frame - Red, 52"        | 12      | 2011   | 33            | 43          | -23.3    |
| "HL Road Frame - Red, 52"        | 11      | 2011   | 43            | 54          | -20.4    |
| "HL Road Frame - Red, 52"        | 10      | 2011   | 54            | 28          | 92.9     |
| "HL Road Frame - Red, 52"        | 9       | 2011   | 28            | 26          | 7.7      |
| "HL Road Frame - Red, 52"        | 8       | 2011   | 26            | 15          | 73.3     |
| "HL Road Frame - Red, 52"        | 7       | 2011   | 15            | 7           | 114.3    |
| "HL Road Frame - Red, 52"        | 6       | 2011   | 7             |             | 0.0      |
| "HL Road Frame - Red, 56"        | 12      | 2011   | 44            | 56          | -21.4    |
| "HL Road Frame - Red, 56"        | 11      | 2011   | 56            | 78          | -28.2    |
| "HL Road Frame - Red, 56"        | 10      | 2011   | 78            | 44          | 77.3     |
| "HL Road Frame - Red, 56"        | 9       | 2011   | 44            | 44          | 0.0      |
| "HL Road Frame - Red, 56"        | 8       | 2011   | 44            | 57          | -22.8    |
| "HL Road Frame - Red, 56"        | 7       | 2011   | 57            | 23          | 147.8    |
| "HL Road Frame - Red, 56"        | 6       | 2011   | 23            |             | 0.0      |
| "HL Road Frame - Red, 62"        | 12      | 2011   | 51            | 59          | -13.6    |
| "HL Road Frame - Red, 62"        | 11      | 2011   | 59            | 66          | -10.6    |
| "HL Road Frame - Red, 62"        | 10      | 2011   | 66            | 55          | 20.0     |
| "HL Road Frame - Red, 62"        | 9       | 2011   | 55            | 42          | 31.0     |
| "HL Road Frame - Red, 62"        | 8       | 2011   | 42            | 34          | 23.5     |
| "HL Road Frame - Red, 62"        | 7       | 2011   | 34            | 15          | 126.7    |
| "HL Road Frame - Red, 62"        | 6       | 2011   | 15            |             | 0.0      |
| HL Road Front Wheel              | 12      | 2011   | 217           | 236         | -8.1     |
| HL Road Front Wheel              | 11      | 2011   | 236           | 279         | -15.4    |
| HL Road Front Wheel              | 10      | 2011   | 279           | 187         | 49.2     |
| HL Road Front Wheel              | 9       | 2011   | 187           | 152         | 23.0     |
| HL Road Front Wheel              | 8       | 2011   | 152           | 150         | 1.3      |
| HL Road Front Wheel              | 7       | 2011   | 150           | 52          | 188.5    |
| HL Road Front Wheel              | 6       | 2011   | 52            |             | 0.0      |
| HL Road Handlebars               | 12      | 2011   | 217           | 236         | -8.1     |
| HL Road Handlebars               | 11      | 2011   | 236           | 279         | -15.4    |
| HL Road Handlebars               | 10      | 2011   | 279           | 187         | 49.2     |
| HL Road Handlebars               | 9       | 2011   | 187           | 152         | 23.0     |
| HL Road Handlebars               | 8       | 2011   | 152           | 150         | 1.3      |
| HL Road Handlebars               | 7       | 2011   | 150           | 52          | 188.5    |
| HL Road Handlebars               | 6       | 2011   | 52            |             | 0.0      |
| HL Road Rear Wheel               | 12      | 2011   | 217           | 236         | -8.1     |
| HL Road Rear Wheel               | 11      | 2011   | 236           | 276         | -14.5    |
| HL Road Rear Wheel               | 10      | 2011   | 276           | 187         | 47.6     |
| HL Road Rear Wheel               | 9       | 2011   | 187           | 152         | 23.0     |
| HL Road Rear Wheel               | 8       | 2011   | 152           | 150         | 1.3      |
| HL Road Rear Wheel               | 7       | 2011   | 150           | 52          | 188.5    |
| HL Road Rear Wheel               | 6       | 2011   | 52            |             | 0.0      |
| HL Road Seat Assembly            | 12      | 2011   | 217           | 236         | -8.1     |
| HL Road Seat Assembly            | 11      | 2011   | 236           | 279         | -15.4    |
| HL Road Seat Assembly            | 10      | 2011   | 279           | 187         | 49.2     |
| HL Road Seat Assembly            | 9       | 2011   | 187           | 152         | 23.0     |
| HL Road Seat Assembly            | 8       | 2011   | 152           | 150         | 1.3      |
| HL Road Seat Assembly            | 7       | 2011   | 150           | 52          | 188.5    |
| HL Road Seat Assembly            | 6       | 2011   | 52            |             | 0.0      |
| Handlebar Tube                   | 12      | 2011   | 848           | 1455        | -41.7    |
| Handlebar Tube                   | 11      | 2011   | 1455          | 1918        | -24.1    |
| Handlebar Tube                   | 10      | 2011   | 1918          | 885         | 116.7    |
| Handlebar Tube                   | 9       | 2011   | 885           | 967         | -8.5     |
| Handlebar Tube                   | 8       | 2011   | 967           | 1284        | -24.7    |
| Handlebar Tube                   | 7       | 2011   | 1284          | 526         | 144.1    |
| Handlebar Tube                   | 6       | 2011   | 526           |             | 0.0      |
| Head Tube                        | 12      | 2011   | 921           | 1799        | -48.8    |
| Head Tube                        | 11      | 2011   | 1799          | 2335        | -23.0    |
| Head Tube                        | 10      | 2011   | 2335          | 1061        | 120.1    |
| Head Tube                        | 9       | 2011   | 1061          | 1165        | -8.9     |
| Head Tube                        | 8       | 2011   | 1165          | 1583        | -26.4    |
| Head Tube                        | 7       | 2011   | 1583          | 640         | 147.3    |
| Head Tube                        | 6       | 2011   | 640           |             | 0.0      |
| LL Bottom Bracket                | 12      | 2011   | 316           | 514         | -38.5    |
| LL Bottom Bracket                | 11      | 2011   | 514           | 689         | -25.4    |
| LL Bottom Bracket                | 10      | 2011   | 689           | 362         | 90.3     |
| LL Bottom Bracket                | 9       | 2011   | 362           | 293         | 23.5     |
| LL Bottom Bracket                | 8       | 2011   | 293           | 439         | -33.3    |
| LL Bottom Bracket                | 7       | 2011   | 439           | 223         | 96.9     |
| LL Bottom Bracket                | 6       | 2011   | 223           |             | 0.0      |
| LL Fork                          | 12      | 2011   | 370           | 679         | -45.5    |
| LL Fork                          | 11      | 2011   | 679           | 926         | -26.7    |
| LL Fork                          | 10      | 2011   | 926           | 479         | 93.3     |
| LL Fork                          | 9       | 2011   | 479           | 383         | 25.1     |
| LL Fork                          | 8       | 2011   | 383           | 609         | -37.1    |
| LL Fork                          | 7       | 2011   | 609           | 308         | 97.7     |
| LL Fork                          | 6       | 2011   | 308           |             | 0.0      |
| LL Hub                           | 12      | 2011   | 652           | 1024        | -36.3    |
| LL Hub                           | 11      | 2011   | 1024          | 1382        | -25.9    |
| LL Hub                           | 10      | 2011   | 1382          | 738         | 87.3     |
| LL Hub                           | 9       | 2011   | 738           | 588         | 25.5     |
| LL Hub                           | 8       | 2011   | 588           | 878         | -33.0    |
| LL Hub                           | 7       | 2011   | 878           | 458         | 91.7     |
| LL Hub                           | 6       | 2011   | 458           |             | 0.0      |
| "LL Road Frame - Black, 44"      | 12      | 2011   | 21            | 33          | -36.4    |
| "LL Road Frame - Black, 44"      | 11      | 2011   | 33            | 45          | -26.7    |
| "LL Road Frame - Black, 44"      | 10      | 2011   | 45            | 30          | 50.0     |
| "LL Road Frame - Black, 44"      | 9       | 2011   | 30            | 18          | 66.7     |
| "LL Road Frame - Black, 44"      | 8       | 2011   | 18            | 19          | -5.3     |
| "LL Road Frame - Black, 44"      | 7       | 2011   | 19            | 13          | 46.2     |
| "LL Road Frame - Black, 44"      | 6       | 2011   | 13            |             | 0.0      |
| "LL Road Frame - Black, 48"      | 12      | 2011   | 8             | 15          | -46.7    |
| "LL Road Frame - Black, 48"      | 11      | 2011   | 15            | 30          | -50.0    |
| "LL Road Frame - Black, 48"      | 10      | 2011   | 30            | 19          | 57.9     |
| "LL Road Frame - Black, 48"      | 9       | 2011   | 19            |             | 0.0      |
| "LL Road Frame - Black, 52"      | 12      | 2011   | 64            | 82          | -22.0    |
| "LL Road Frame - Black, 52"      | 11      | 2011   | 82            | 133         | -38.3    |
| "LL Road Frame - Black, 52"      | 10      | 2011   | 133           | 71          | 87.3     |
| "LL Road Frame - Black, 52"      | 9       | 2011   | 71            | 71          | 0.0      |
| "LL Road Frame - Black, 52"      | 8       | 2011   | 71            | 126         | -43.7    |
| "LL Road Frame - Black, 52"      | 7       | 2011   | 126           | 48          | 162.5    |
| "LL Road Frame - Black, 52"      | 6       | 2011   | 48            |             | 0.0      |
| "LL Road Frame - Black, 58"      | 12      | 2011   | 47            | 68          | -30.9    |
| "LL Road Frame - Black, 58"      | 11      | 2011   | 68            | 107         | -36.4    |
| "LL Road Frame - Black, 58"      | 10      | 2011   | 107           | 59          | 81.4     |
| "LL Road Frame - Black, 58"      | 9       | 2011   | 59            | 34          | 73.5     |
| "LL Road Frame - Black, 58"      | 8       | 2011   | 34            | 67          | -49.3    |
| "LL Road Frame - Black, 58"      | 7       | 2011   | 67            | 41          | 63.4     |
| "LL Road Frame - Black, 58"      | 6       | 2011   | 41            |             | 0.0      |
| "LL Road Frame - Black, 60"      | 12      | 2011   | 20            | 25          | -20.0    |
| "LL Road Frame - Black, 60"      | 11      | 2011   | 25            | 46          | -45.7    |
| "LL Road Frame - Black, 60"      | 10      | 2011   | 46            | 31          | 48.4     |
| "LL Road Frame - Black, 60"      | 9       | 2011   | 31            | 19          | 63.2     |
| "LL Road Frame - Black, 60"      | 8       | 2011   | 19            | 16          | 18.8     |
| "LL Road Frame - Black, 60"      | 7       | 2011   | 16            | 11          | 45.5     |
| "LL Road Frame - Black, 60"      | 6       | 2011   | 11            |             | 0.0      |
| "LL Road Frame - Black, 62"      | 12      | 2011   | 9             | 19          | -52.6    |
| "LL Road Frame - Black, 62"      | 11      | 2011   | 19            | 37          | -48.6    |
| "LL Road Frame - Black, 62"      | 10      | 2011   | 37            | 17          | 117.6    |
| "LL Road Frame - Black, 62"      | 9       | 2011   | 17            | 2           | 750.0    |
| "LL Road Frame - Black, 62"      | 8       | 2011   | 2             | 1           | 100.0    |
| "LL Road Frame - Black, 62"      | 7       | 2011   | 1             | 2           | -50.0    |
| "LL Road Frame - Black, 62"      | 6       | 2011   | 2             |             | 0.0      |
| "LL Road Frame - Red, 44"        | 12      | 2011   | 53            | 118         | -55.1    |
| "LL Road Frame - Red, 44"        | 11      | 2011   | 118           | 106         | 11.3     |
| "LL Road Frame - Red, 44"        | 10      | 2011   | 106           | 62          | 71.0     |
| "LL Road Frame - Red, 44"        | 9       | 2011   | 62            | 78          | -20.5    |
| "LL Road Frame - Red, 44"        | 8       | 2011   | 78            | 104         | -25.0    |
| "LL Road Frame - Red, 44"        | 7       | 2011   | 104           | 59          | 76.3     |
| "LL Road Frame - Red, 44"        | 6       | 2011   | 59            |             | 0.0      |
| "LL Road Frame - Red, 48"        | 12      | 2011   | 36            | 78          | -53.8    |
| "LL Road Frame - Red, 48"        | 11      | 2011   | 78            | 107         | -27.1    |
| "LL Road Frame - Red, 48"        | 10      | 2011   | 107           | 53          | 101.9    |
| "LL Road Frame - Red, 48"        | 9       | 2011   | 53            | 41          | 29.3     |
| "LL Road Frame - Red, 48"        | 8       | 2011   | 41            | 73          | -43.8    |
| "LL Road Frame - Red, 48"        | 7       | 2011   | 73            | 29          | 151.7    |
| "LL Road Frame - Red, 48"        | 6       | 2011   | 29            |             | 0.0      |
| "LL Road Frame - Red, 52"        | 12      | 2011   | 23            | 33          | -30.3    |
| "LL Road Frame - Red, 52"        | 11      | 2011   | 33            | 50          | -34.0    |
| "LL Road Frame - Red, 52"        | 10      | 2011   | 50            | 23          | 117.4    |
| "LL Road Frame - Red, 52"        | 9       | 2011   | 23            | 14          | 64.3     |
| "LL Road Frame - Red, 52"        | 8       | 2011   | 14            | 16          | -12.5    |
| "LL Road Frame - Red, 52"        | 7       | 2011   | 16            | 15          | 6.7      |
| "LL Road Frame - Red, 52"        | 6       | 2011   | 15            |             | 0.0      |
| "LL Road Frame - Red, 58"        | 12      | 2011   | 12            | 20          | -40.0    |
| "LL Road Frame - Red, 58"        | 11      | 2011   | 20            | 33          | -39.4    |
| "LL Road Frame - Red, 58"        | 10      | 2011   | 33            | 7           | 371.4    |
| "LL Road Frame - Red, 58"        | 9       | 2011   | 7             | 1           | 600.0    |
| "LL Road Frame - Red, 58"        | 8       | 2011   | 1             |             | 0.0      |
| "LL Road Frame - Red, 60"        | 12      | 2011   | 43            | 98          | -56.1    |
| "LL Road Frame - Red, 60"        | 11      | 2011   | 98            | 126         | -22.2    |
| "LL Road Frame - Red, 60"        | 10      | 2011   | 126           | 58          | 117.2    |
| "LL Road Frame - Red, 60"        | 9       | 2011   | 58            | 72          | -19.4    |
| "LL Road Frame - Red, 60"        | 8       | 2011   | 72            | 112         | -35.7    |
| "LL Road Frame - Red, 60"        | 7       | 2011   | 112           | 59          | 89.8     |
| "LL Road Frame - Red, 60"        | 6       | 2011   | 59            |             | 0.0      |
| "LL Road Frame - Red, 62"        | 12      | 2011   | 38            | 87          | -56.3    |
| "LL Road Frame - Red, 62"        | 11      | 2011   | 87            | 105         | -17.1    |
| "LL Road Frame - Red, 62"        | 10      | 2011   | 105           | 49          | 114.3    |
| "LL Road Frame - Red, 62"        | 9       | 2011   | 49            | 44          | 11.4     |
| "LL Road Frame - Red, 62"        | 8       | 2011   | 44            | 75          | -41.3    |
| "LL Road Frame - Red, 62"        | 7       | 2011   | 75            | 33          | 127.3    |
| "LL Road Frame - Red, 62"        | 6       | 2011   | 33            |             | 0.0      |
| LL Road Front Wheel              | 12      | 2011   | 322           | 514         | -37.4    |
| LL Road Front Wheel              | 11      | 2011   | 514           | 689         | -25.4    |
| LL Road Front Wheel              | 10      | 2011   | 689           | 369         | 86.7     |
| LL Road Front Wheel              | 9       | 2011   | 369           | 293         | 25.9     |
| LL Road Front Wheel              | 8       | 2011   | 293           | 439         | -33.3    |
| LL Road Front Wheel              | 7       | 2011   | 439           | 227         | 93.4     |
| LL Road Front Wheel              | 6       | 2011   | 227           |             | 0.0      |
| LL Road Handlebars               | 12      | 2011   | 322           | 514         | -37.4    |
| LL Road Handlebars               | 11      | 2011   | 514           | 689         | -25.4    |
| LL Road Handlebars               | 10      | 2011   | 689           | 369         | 86.7     |
| LL Road Handlebars               | 9       | 2011   | 369           | 293         | 25.9     |
| LL Road Handlebars               | 8       | 2011   | 293           | 439         | -33.3    |
| LL Road Handlebars               | 7       | 2011   | 439           | 227         | 93.4     |
| LL Road Handlebars               | 6       | 2011   | 227           |             | 0.0      |
| LL Road Rear Wheel               | 12      | 2011   | 322           | 497         | -35.2    |
| LL Road Rear Wheel               | 11      | 2011   | 497           | 689         | -27.9    |
| LL Road Rear Wheel               | 10      | 2011   | 689           | 369         | 86.7     |
| LL Road Rear Wheel               | 9       | 2011   | 369           | 293         | 25.9     |
| LL Road Rear Wheel               | 8       | 2011   | 293           | 439         | -33.3    |
| LL Road Rear Wheel               | 7       | 2011   | 439           | 227         | 93.4     |
| LL Road Rear Wheel               | 6       | 2011   | 227           |             | 0.0      |
| LL Road Seat Assembly            | 12      | 2011   | 322           | 514         | -37.4    |
| LL Road Seat Assembly            | 11      | 2011   | 514           | 689         | -25.4    |
| LL Road Seat Assembly            | 10      | 2011   | 689           | 369         | 86.7     |
| LL Road Seat Assembly            | 9       | 2011   | 369           | 293         | 25.9     |
| LL Road Seat Assembly            | 8       | 2011   | 293           | 427         | -31.4    |
| LL Road Seat Assembly            | 7       | 2011   | 427           | 227         | 88.1     |
| LL Road Seat Assembly            | 6       | 2011   | 227           |             | 0.0      |
| ML Bottom Bracket                | 12      | 2011   | 113           | 207         | -45.4    |
| ML Bottom Bracket                | 11      | 2011   | 207           | 291         | -28.9    |
| ML Bottom Bracket                | 10      | 2011   | 291           | 110         | 164.5    |
| ML Bottom Bracket                | 9       | 2011   | 110           | 128         | -14.1    |
| ML Bottom Bracket                | 8       | 2011   | 128           | 169         | -24.3    |
| ML Bottom Bracket                | 7       | 2011   | 169           | 98          | 72.4     |
| ML Bottom Bracket                | 6       | 2011   | 98            |             | 0.0      |
| ML Crankset                      | 12      | 2011   | 435           | 721         | -39.7    |
| ML Crankset                      | 11      | 2011   | 721           | 980         | -26.4    |
| ML Crankset                      | 10      | 2011   | 980           | 479         | 104.6    |
| ML Crankset                      | 9       | 2011   | 479           | 409         | 17.1     |
| ML Crankset                      | 8       | 2011   | 409           | 608         | -32.7    |
| ML Crankset                      | 7       | 2011   | 608           | 325         | 87.1     |
| ML Crankset                      | 6       | 2011   | 325           |             | 0.0      |
| ML Fork                          | 12      | 2011   | 123           | 249         | -50.6    |
| ML Fork                          | 11      | 2011   | 249           | 345         | -27.8    |
| ML Fork                          | 10      | 2011   | 345           | 130         | 165.4    |
| ML Fork                          | 9       | 2011   | 130           | 150         | -13.3    |
| ML Fork                          | 8       | 2011   | 150           | 203         | -26.1    |
| ML Fork                          | 7       | 2011   | 203           | 118         | 72.0     |
| ML Fork                          | 6       | 2011   | 118           |             | 0.0      |
| ML Headset                       | 12      | 2011   | 435           | 721         | -39.7    |
| ML Headset                       | 11      | 2011   | 721           | 980         | -26.4    |
| ML Headset                       | 10      | 2011   | 980           | 479         | 104.6    |
| ML Headset                       | 9       | 2011   | 479           | 407         | 17.7     |
| ML Headset                       | 8       | 2011   | 407           | 608         | -33.1    |
| ML Headset                       | 7       | 2011   | 608           | 325         | 87.1     |
| ML Headset                       | 6       | 2011   | 325           |             | 0.0      |
| "ML Road Frame - Red, 44"        | 12      | 2011   | 23            | 36          | -36.1    |
| "ML Road Frame - Red, 44"        | 11      | 2011   | 36            | 46          | -21.7    |
| "ML Road Frame - Red, 44"        | 10      | 2011   | 46            | 19          | 142.1    |
| "ML Road Frame - Red, 44"        | 9       | 2011   | 19            | 21          | -9.5     |
| "ML Road Frame - Red, 44"        | 8       | 2011   | 21            | 21          | 0.0      |
| "ML Road Frame - Red, 44"        | 7       | 2011   | 21            | 14          | 50.0     |
| "ML Road Frame - Red, 44"        | 6       | 2011   | 14            |             | 0.0      |
| "ML Road Frame - Red, 48"        | 12      | 2011   | 16            | 45          | -64.4    |
| "ML Road Frame - Red, 48"        | 11      | 2011   | 45            | 68          | -33.8    |
| "ML Road Frame - Red, 48"        | 10      | 2011   | 68            | 20          | 240.0    |
| "ML Road Frame - Red, 48"        | 9       | 2011   | 20            | 14          | 42.9     |
| "ML Road Frame - Red, 48"        | 8       | 2011   | 14            | 26          | -46.2    |
| "ML Road Frame - Red, 48"        | 7       | 2011   | 26            | 16          | 62.5     |
| "ML Road Frame - Red, 48"        | 6       | 2011   | 16            |             | 0.0      |
| "ML Road Frame - Red, 52"        | 12      | 2011   | 37            | 81          | -54.3    |
| "ML Road Frame - Red, 52"        | 11      | 2011   | 81            | 102         | -20.6    |
| "ML Road Frame - Red, 52"        | 10      | 2011   | 102           | 43          | 137.2    |
| "ML Road Frame - Red, 52"        | 9       | 2011   | 43            | 51          | -15.7    |
| "ML Road Frame - Red, 52"        | 8       | 2011   | 51            | 78          | -34.6    |
| "ML Road Frame - Red, 52"        | 7       | 2011   | 78            | 50          | 56.0     |
| "ML Road Frame - Red, 52"        | 6       | 2011   | 50            |             | 0.0      |
| "ML Road Frame - Red, 58"        | 12      | 2011   | 29            | 55          | -47.3    |
| "ML Road Frame - Red, 58"        | 11      | 2011   | 55            | 81          | -32.1    |
| "ML Road Frame - Red, 58"        | 10      | 2011   | 81            | 26          | 211.5    |
| "ML Road Frame - Red, 58"        | 9       | 2011   | 26            | 37          | -29.7    |
| "ML Road Frame - Red, 58"        | 8       | 2011   | 37            | 54          | -31.5    |
| "ML Road Frame - Red, 58"        | 7       | 2011   | 54            | 27          | 100.0    |
| "ML Road Frame - Red, 58"        | 6       | 2011   | 27            |             | 0.0      |
| "ML Road Frame - Red, 60"        | 12      | 2011   | 18            | 32          | -43.8    |
| "ML Road Frame - Red, 60"        | 11      | 2011   | 32            | 47          | -31.9    |
| "ML Road Frame - Red, 60"        | 10      | 2011   | 47            | 22          | 113.6    |
| "ML Road Frame - Red, 60"        | 9       | 2011   | 22            | 27          | -18.5    |
| "ML Road Frame - Red, 60"        | 8       | 2011   | 27            | 22          | 22.7     |
| "ML Road Frame - Red, 60"        | 7       | 2011   | 22            | 11          | 100.0    |
| "ML Road Frame - Red, 60"        | 6       | 2011   | 11            |             | 0.0      |
| ML Road Front Wheel              | 12      | 2011   | 113           | 202         | -44.1    |
| ML Road Front Wheel              | 11      | 2011   | 202           | 291         | -30.6    |
| ML Road Front Wheel              | 10      | 2011   | 291           | 110         | 164.5    |
| ML Road Front Wheel              | 9       | 2011   | 110           | 128         | -14.1    |
| ML Road Front Wheel              | 8       | 2011   | 128           | 169         | -24.3    |
| ML Road Front Wheel              | 7       | 2011   | 169           | 98          | 72.4     |
| ML Road Front Wheel              | 6       | 2011   | 98            |             | 0.0      |
| ML Road Handlebars               | 12      | 2011   | 113           | 207         | -45.4    |
| ML Road Handlebars               | 11      | 2011   | 207           | 291         | -28.9    |
| ML Road Handlebars               | 10      | 2011   | 291           | 110         | 164.5    |
| ML Road Handlebars               | 9       | 2011   | 110           | 125         | -12.0    |
| ML Road Handlebars               | 8       | 2011   | 125           | 169         | -26.0    |
| ML Road Handlebars               | 7       | 2011   | 169           | 98          | 72.4     |
| ML Road Handlebars               | 6       | 2011   | 98            |             | 0.0      |
| ML Road Rear Wheel               | 12      | 2011   | 113           | 207         | -45.4    |
| ML Road Rear Wheel               | 11      | 2011   | 207           | 291         | -28.9    |
| ML Road Rear Wheel               | 10      | 2011   | 291           | 110         | 164.5    |
| ML Road Rear Wheel               | 9       | 2011   | 110           | 128         | -14.1    |
| ML Road Rear Wheel               | 8       | 2011   | 128           | 169         | -24.3    |
| ML Road Rear Wheel               | 7       | 2011   | 169           | 98          | 72.4     |
| ML Road Rear Wheel               | 6       | 2011   | 98            |             | 0.0      |
| ML Road Seat Assembly            | 12      | 2011   | 113           | 207         | -45.4    |
| ML Road Seat Assembly            | 11      | 2011   | 207           | 291         | -28.9    |
| ML Road Seat Assembly            | 10      | 2011   | 291           | 110         | 164.5    |
| ML Road Seat Assembly            | 9       | 2011   | 110           | 128         | -14.1    |
| ML Road Seat Assembly            | 8       | 2011   | 128           | 169         | -24.3    |
| ML Road Seat Assembly            | 7       | 2011   | 169           | 97          | 74.2     |
| ML Road Seat Assembly            | 6       | 2011   | 97            |             | 0.0      |
| Mountain End Caps                | 12      | 2011   | 408           | 974         | -58.1    |
| Mountain End Caps                | 11      | 2011   | 974           | 1314        | -25.9    |
| Mountain End Caps                | 10      | 2011   | 1314          | 415         | 216.6    |
| Mountain End Caps                | 9       | 2011   | 415           | 792         | -47.6    |
| Mountain End Caps                | 8       | 2011   | 792           | 1040        | -23.8    |
| Mountain End Caps                | 7       | 2011   | 1040          | 256         | 306.3    |
| Mountain End Caps                | 6       | 2011   | 256           |             | 0.0      |
| "Mountain-100 Black, 38"         | 12      | 2011   | 28            | 66          | -57.6    |
| "Mountain-100 Black, 38"         | 11      | 2011   | 66            | 82          | -19.5    |
| "Mountain-100 Black, 38"         | 10      | 2011   | 82            | 34          | 141.2    |
| "Mountain-100 Black, 38"         | 9       | 2011   | 34            | 52          | -34.6    |
| "Mountain-100 Black, 38"         | 8       | 2011   | 52            | 71          | -26.8    |
| "Mountain-100 Black, 38"         | 7       | 2011   | 71            | 22          | 222.7    |
| "Mountain-100 Black, 38"         | 6       | 2011   | 22            |             | 0.0      |
| "Mountain-100 Black, 42"         | 12      | 2011   | 22            | 67          | -67.2    |
| "Mountain-100 Black, 42"         | 11      | 2011   | 67            | 76          | -11.8    |
| "Mountain-100 Black, 42"         | 10      | 2011   | 76            | 30          | 153.3    |
| "Mountain-100 Black, 42"         | 9       | 2011   | 30            | 44          | -31.8    |
| "Mountain-100 Black, 42"         | 8       | 2011   | 44            | 77          | -42.9    |
| "Mountain-100 Black, 42"         | 7       | 2011   | 77            | 16          | 381.3    |
| "Mountain-100 Black, 42"         | 6       | 2011   | 16            |             | 0.0      |
| "Mountain-100 Black, 44"         | 12      | 2011   | 28            | 69          | -59.4    |
| "Mountain-100 Black, 44"         | 11      | 2011   | 69            | 96          | -28.1    |
| "Mountain-100 Black, 44"         | 10      | 2011   | 96            | 26          | 269.2    |
| "Mountain-100 Black, 44"         | 9       | 2011   | 26            | 58          | -55.2    |
| "Mountain-100 Black, 44"         | 8       | 2011   | 58            | 68          | -14.7    |
| "Mountain-100 Black, 44"         | 7       | 2011   | 68            | 23          | 195.7    |
| "Mountain-100 Black, 44"         | 6       | 2011   | 23            |             | 0.0      |
| "Mountain-100 Black, 48"         | 12      | 2011   | 27            | 59          | -54.2    |
| "Mountain-100 Black, 48"         | 11      | 2011   | 59            | 79          | -25.3    |
| "Mountain-100 Black, 48"         | 10      | 2011   | 79            | 23          | 243.5    |
| "Mountain-100 Black, 48"         | 9       | 2011   | 23            | 46          | -50.0    |
| "Mountain-100 Black, 48"         | 8       | 2011   | 46            | 63          | -27.0    |
| "Mountain-100 Black, 48"         | 7       | 2011   | 63            | 21          | 200.0    |
| "Mountain-100 Black, 48"         | 6       | 2011   | 21            |             | 0.0      |
| "Mountain-100 Silver, 38"        | 12      | 2011   | 30            | 55          | -45.5    |
| "Mountain-100 Silver, 38"        | 11      | 2011   | 55            | 88          | -37.5    |
| "Mountain-100 Silver, 38"        | 10      | 2011   | 88            | 25          | 252.0    |
| "Mountain-100 Silver, 38"        | 9       | 2011   | 25            | 60          | -58.3    |
| "Mountain-100 Silver, 38"        | 8       | 2011   | 60            | 76          | -21.1    |
| "Mountain-100 Silver, 38"        | 7       | 2011   | 76            | 11          | 590.9    |
| "Mountain-100 Silver, 38"        | 6       | 2011   | 11            |             | 0.0      |
| "Mountain-100 Silver, 42"        | 12      | 2011   | 17            | 48          | -64.6    |
| "Mountain-100 Silver, 42"        | 11      | 2011   | 48            | 96          | -50.0    |
| "Mountain-100 Silver, 42"        | 10      | 2011   | 96            | 20          | 380.0    |
| "Mountain-100 Silver, 42"        | 9       | 2011   | 20            | 49          | -59.2    |
| "Mountain-100 Silver, 42"        | 8       | 2011   | 49            | 59          | -16.9    |
| "Mountain-100 Silver, 42"        | 7       | 2011   | 59            | 5           | 1080.0   |
| "Mountain-100 Silver, 42"        | 6       | 2011   | 5             |             | 0.0      |
| "Mountain-100 Silver, 44"        | 12      | 2011   | 36            | 63          | -42.9    |
| "Mountain-100 Silver, 44"        | 11      | 2011   | 63            | 70          | -10.0    |
| "Mountain-100 Silver, 44"        | 10      | 2011   | 70            | 24          | 191.7    |
| "Mountain-100 Silver, 44"        | 9       | 2011   | 24            | 50          | -52.0    |
| "Mountain-100 Silver, 44"        | 8       | 2011   | 50            | 62          | -19.4    |
| "Mountain-100 Silver, 44"        | 7       | 2011   | 62            | 18          | 244.4    |
| "Mountain-100 Silver, 44"        | 6       | 2011   | 18            |             | 0.0      |
| "Mountain-100 Silver, 48"        | 12      | 2011   | 21            | 55          | -61.8    |
| "Mountain-100 Silver, 48"        | 11      | 2011   | 55            | 68          | -19.1    |
| "Mountain-100 Silver, 48"        | 10      | 2011   | 68            | 26          | 161.5    |
| "Mountain-100 Silver, 48"        | 9       | 2011   | 26            | 37          | -29.7    |
| "Mountain-100 Silver, 48"        | 8       | 2011   | 37            | 44          | -15.9    |
| "Mountain-100 Silver, 48"        | 7       | 2011   | 44            | 8           | 450.0    |
| "Mountain-100 Silver, 48"        | 6       | 2011   | 8             |             | 0.0      |
| Rear Derailleur                  | 12      | 2011   | 861           | 1440        | -40.2    |
| Rear Derailleur                  | 11      | 2011   | 1440          | 1918        | -24.9    |
| Rear Derailleur                  | 10      | 2011   | 1918          | 874         | 119.5    |
| Rear Derailleur                  | 9       | 2011   | 874           | 969         | -9.8     |
| Rear Derailleur                  | 8       | 2011   | 969           | 1278        | -24.2    |
| Rear Derailleur                  | 7       | 2011   | 1278          | 501         | 155.1    |
| Rear Derailleur                  | 6       | 2011   | 501           |             | 0.0      |
| Road End Caps                    | 12      | 2011   | 1282          | 1936        | -33.8    |
| Road End Caps                    | 11      | 2011   | 1936          | 2522        | -23.2    |
| Road End Caps                    | 10      | 2011   | 2522          | 1348        | 87.1     |
| Road End Caps                    | 9       | 2011   | 1348          | 1142        | 18.0     |
| Road End Caps                    | 8       | 2011   | 1142          | 1528        | -25.3    |
| Road End Caps                    | 7       | 2011   | 1528          | 792         | 92.9     |
| Road End Caps                    | 6       | 2011   | 792           |             | 0.0      |
| "Road-150 Red, 44"               | 12      | 2011   | 38            | 45          | -15.6    |
| "Road-150 Red, 44"               | 11      | 2011   | 45            | 40          | 12.5     |
| "Road-150 Red, 44"               | 10      | 2011   | 40            | 35          | 14.3     |
| "Road-150 Red, 44"               | 9       | 2011   | 35            | 20          | 75.0     |
| "Road-150 Red, 44"               | 8       | 2011   | 20            | 17          | 17.6     |
| "Road-150 Red, 44"               | 7       | 2011   | 17            | 10          | 70.0     |
| "Road-150 Red, 44"               | 6       | 2011   | 10            |             | 0.0      |
| "Road-150 Red, 48"               | 12      | 2011   | 47            | 41          | 14.6     |
| "Road-150 Red, 48"               | 11      | 2011   | 41            | 45          | -8.9     |
| "Road-150 Red, 48"               | 10      | 2011   | 45            | 35          | 28.6     |
| "Road-150 Red, 48"               | 9       | 2011   | 35            | 18          | 94.4     |
| "Road-150 Red, 48"               | 8       | 2011   | 18            | 34          | -47.1    |
| "Road-150 Red, 48"               | 7       | 2011   | 34            | 4           | 750.0    |
| "Road-150 Red, 48"               | 6       | 2011   | 4             |             | 0.0      |
| "Road-150 Red, 52"               | 12      | 2011   | 35            | 41          | -14.6    |
| "Road-150 Red, 52"               | 11      | 2011   | 41            | 53          | -22.6    |
| "Road-150 Red, 52"               | 10      | 2011   | 53            | 29          | 82.8     |
| "Road-150 Red, 52"               | 9       | 2011   | 29            | 24          | 20.8     |
| "Road-150 Red, 52"               | 8       | 2011   | 24            | 15          | 60.0     |
| "Road-150 Red, 52"               | 7       | 2011   | 15            | 4           | 275.0    |
| "Road-150 Red, 52"               | 6       | 2011   | 4             |             | 0.0      |
| "Road-150 Red, 56"               | 12      | 2011   | 46            | 55          | -16.4    |
| "Road-150 Red, 56"               | 11      | 2011   | 55            | 74          | -25.7    |
| "Road-150 Red, 56"               | 10      | 2011   | 74            | 44          | 68.2     |
| "Road-150 Red, 56"               | 9       | 2011   | 44            | 46          | -4.3     |
| "Road-150 Red, 56"               | 8       | 2011   | 46            | 55          | -16.4    |
| "Road-150 Red, 56"               | 7       | 2011   | 55            | 21          | 161.9    |
| "Road-150 Red, 56"               | 6       | 2011   | 21            |             | 0.0      |
| "Road-150 Red, 62"               | 12      | 2011   | 51            | 54          | -5.6     |
| "Road-150 Red, 62"               | 11      | 2011   | 54            | 67          | -19.4    |
| "Road-150 Red, 62"               | 10      | 2011   | 67            | 44          | 52.3     |
| "Road-150 Red, 62"               | 9       | 2011   | 44            | 44          | 0.0      |
| "Road-150 Red, 62"               | 8       | 2011   | 44            | 29          | 51.7     |
| "Road-150 Red, 62"               | 7       | 2011   | 29            | 13          | 123.1    |
| "Road-150 Red, 62"               | 6       | 2011   | 13            |             | 0.0      |
| "Road-450 Red, 44"               | 12      | 2011   | 23            | 36          | -36.1    |
| "Road-450 Red, 44"               | 11      | 2011   | 36            | 46          | -21.7    |
| "Road-450 Red, 44"               | 10      | 2011   | 46            | 19          | 142.1    |
| "Road-450 Red, 44"               | 9       | 2011   | 19            | 21          | -9.5     |
| "Road-450 Red, 44"               | 8       | 2011   | 21            | 21          | 0.0      |
| "Road-450 Red, 44"               | 7       | 2011   | 21            | 14          | 50.0     |
| "Road-450 Red, 44"               | 6       | 2011   | 14            |             | 0.0      |
| "Road-450 Red, 48"               | 12      | 2011   | 6             | 14          | -57.1    |
| "Road-450 Red, 48"               | 11      | 2011   | 14            | 29          | -51.7    |
| "Road-450 Red, 48"               | 10      | 2011   | 29            | 9           | 222.2    |
| "Road-450 Red, 48"               | 9       | 2011   | 9             |             | 0.0      |
| "Road-450 Red, 52"               | 12      | 2011   | 37            | 70          | -47.1    |
| "Road-450 Red, 52"               | 11      | 2011   | 70            | 87          | -19.5    |
| "Road-450 Red, 52"               | 10      | 2011   | 87            | 34          | 155.9    |
| "Road-450 Red, 52"               | 9       | 2011   | 34            | 43          | -20.9    |
| "Road-450 Red, 52"               | 8       | 2011   | 43            | 72          | -40.3    |
| "Road-450 Red, 52"               | 7       | 2011   | 72            | 46          | 56.5     |
| "Road-450 Red, 52"               | 6       | 2011   | 46            |             | 0.0      |
| "Road-450 Red, 58"               | 12      | 2011   | 29            | 55          | -47.3    |
| "Road-450 Red, 58"               | 11      | 2011   | 55            | 82          | -32.9    |
| "Road-450 Red, 58"               | 10      | 2011   | 82            | 26          | 215.4    |
| "Road-450 Red, 58"               | 9       | 2011   | 26            | 37          | -29.7    |
| "Road-450 Red, 58"               | 8       | 2011   | 37            | 54          | -31.5    |
| "Road-450 Red, 58"               | 7       | 2011   | 54            | 27          | 100.0    |
| "Road-450 Red, 58"               | 6       | 2011   | 27            |             | 0.0      |
| "Road-450 Red, 60"               | 12      | 2011   | 18            | 32          | -43.8    |
| "Road-450 Red, 60"               | 11      | 2011   | 32            | 47          | -31.9    |
| "Road-450 Red, 60"               | 10      | 2011   | 47            | 22          | 113.6    |
| "Road-450 Red, 60"               | 9       | 2011   | 22            | 27          | -18.5    |
| "Road-450 Red, 60"               | 8       | 2011   | 27            | 22          | 22.7     |
| "Road-450 Red, 60"               | 7       | 2011   | 22            | 11          | 100.0    |
| "Road-450 Red, 60"               | 6       | 2011   | 11            |             | 0.0      |
| "Road-650 Black, 44"             | 12      | 2011   | 21            | 27          | -22.2    |
| "Road-650 Black, 44"             | 11      | 2011   | 27            | 42          | -35.7    |
| "Road-650 Black, 44"             | 10      | 2011   | 42            | 27          | 55.6     |
| "Road-650 Black, 44"             | 9       | 2011   | 27            | 17          | 58.8     |
| "Road-650 Black, 44"             | 8       | 2011   | 17            | 19          | -10.5    |
| "Road-650 Black, 44"             | 7       | 2011   | 19            | 13          | 46.2     |
| "Road-650 Black, 44"             | 6       | 2011   | 13            |             | 0.0      |
| "Road-650 Black, 48"             | 12      | 2011   | 8             | 15          | -46.7    |
| "Road-650 Black, 48"             | 11      | 2011   | 15            | 30          | -50.0    |
| "Road-650 Black, 48"             | 10      | 2011   | 30            | 19          | 57.9     |
| "Road-650 Black, 48"             | 9       | 2011   | 19            |             | 0.0      |
| "Road-650 Black, 52"             | 12      | 2011   | 53            | 56          | -5.4     |
| "Road-650 Black, 52"             | 11      | 2011   | 56            | 89          | -37.1    |
| "Road-650 Black, 52"             | 10      | 2011   | 89            | 51          | 74.5     |
| "Road-650 Black, 52"             | 9       | 2011   | 51            | 48          | 6.3      |
| "Road-650 Black, 52"             | 8       | 2011   | 48            | 86          | -44.2    |
| "Road-650 Black, 52"             | 7       | 2011   | 86            | 29          | 196.6    |
| "Road-650 Black, 52"             | 6       | 2011   | 29            |             | 0.0      |
| "Road-650 Black, 58"             | 12      | 2011   | 43            | 49          | -12.2    |
| "Road-650 Black, 58"             | 11      | 2011   | 49            | 70          | -30.0    |
| "Road-650 Black, 58"             | 10      | 2011   | 70            | 43          | 62.8     |
| "Road-650 Black, 58"             | 9       | 2011   | 43            | 27          | 59.3     |
| "Road-650 Black, 58"             | 8       | 2011   | 27            | 49          | -44.9    |
| "Road-650 Black, 58"             | 7       | 2011   | 49            | 31          | 58.1     |
| "Road-650 Black, 58"             | 6       | 2011   | 31            |             | 0.0      |
| "Road-650 Black, 60"             | 12      | 2011   | 18            | 25          | -28.0    |
| "Road-650 Black, 60"             | 11      | 2011   | 25            | 45          | -44.4    |
| "Road-650 Black, 60"             | 10      | 2011   | 45            | 32          | 40.6     |
| "Road-650 Black, 60"             | 9       | 2011   | 32            | 18          | 77.8     |
| "Road-650 Black, 60"             | 8       | 2011   | 18            | 16          | 12.5     |
| "Road-650 Black, 60"             | 7       | 2011   | 16            | 11          | 45.5     |
| "Road-650 Black, 60"             | 6       | 2011   | 11            |             | 0.0      |
| "Road-650 Black, 62"             | 12      | 2011   | 9             | 21          | -57.1    |
| "Road-650 Black, 62"             | 11      | 2011   | 21            | 35          | -40.0    |
| "Road-650 Black, 62"             | 10      | 2011   | 35            | 16          | 118.8    |
| "Road-650 Black, 62"             | 9       | 2011   | 16            | 2           | 700.0    |
| "Road-650 Black, 62"             | 8       | 2011   | 2             | 1           | 100.0    |
| "Road-650 Black, 62"             | 7       | 2011   | 1             | 2           | -50.0    |
| "Road-650 Black, 62"             | 6       | 2011   | 2             |             | 0.0      |
| "Road-650 Red, 44"               | 12      | 2011   | 40            | 92          | -56.5    |
| "Road-650 Red, 44"               | 11      | 2011   | 92            | 64          | 43.8     |
| "Road-650 Red, 44"               | 10      | 2011   | 64            | 38          | 68.4     |
| "Road-650 Red, 44"               | 9       | 2011   | 38            | 49          | -22.4    |
| "Road-650 Red, 44"               | 8       | 2011   | 49            | 66          | -25.8    |
| "Road-650 Red, 44"               | 7       | 2011   | 66            | 44          | 50.0     |
| "Road-650 Red, 44"               | 6       | 2011   | 44            |             | 0.0      |
| "Road-650 Red, 48"               | 12      | 2011   | 31            | 54          | -42.6    |
| "Road-650 Red, 48"               | 11      | 2011   | 54            | 68          | -20.6    |
| "Road-650 Red, 48"               | 10      | 2011   | 68            | 41          | 65.9     |
| "Road-650 Red, 48"               | 9       | 2011   | 41            | 34          | 20.6     |
| "Road-650 Red, 48"               | 8       | 2011   | 34            | 54          | -37.0    |
| "Road-650 Red, 48"               | 7       | 2011   | 54            | 20          | 170.0    |
| "Road-650 Red, 48"               | 6       | 2011   | 20            |             | 0.0      |
| "Road-650 Red, 52"               | 12      | 2011   | 23            | 30          | -23.3    |
| "Road-650 Red, 52"               | 11      | 2011   | 30            | 50          | -40.0    |
| "Road-650 Red, 52"               | 10      | 2011   | 50            | 21          | 138.1    |
| "Road-650 Red, 52"               | 9       | 2011   | 21            | 14          | 50.0     |
| "Road-650 Red, 52"               | 8       | 2011   | 14            | 16          | -12.5    |
| "Road-650 Red, 52"               | 7       | 2011   | 16            | 15          | 6.7      |
| "Road-650 Red, 52"               | 6       | 2011   | 15            |             | 0.0      |
| "Road-650 Red, 58"               | 12      | 2011   | 12            | 19          | -36.8    |
| "Road-650 Red, 58"               | 11      | 2011   | 19            | 33          | -42.4    |
| "Road-650 Red, 58"               | 10      | 2011   | 33            | 7           | 371.4    |
| "Road-650 Red, 58"               | 9       | 2011   | 7             | 1           | 600.0    |
| "Road-650 Red, 58"               | 8       | 2011   | 1             |             | 0.0      |
| "Road-650 Red, 60"               | 12      | 2011   | 33            | 67          | -50.7    |
| "Road-650 Red, 60"               | 11      | 2011   | 67            | 88          | -23.9    |
| "Road-650 Red, 60"               | 10      | 2011   | 88            | 40          | 120.0    |
| "Road-650 Red, 60"               | 9       | 2011   | 40            | 47          | -14.9    |
| "Road-650 Red, 60"               | 8       | 2011   | 47            | 70          | -32.9    |
| "Road-650 Red, 60"               | 7       | 2011   | 70            | 43          | 62.8     |
| "Road-650 Red, 60"               | 6       | 2011   | 43            |             | 0.0      |
| "Road-650 Red, 62"               | 12      | 2011   | 31            | 56          | -44.6    |
| "Road-650 Red, 62"               | 11      | 2011   | 56            | 73          | -23.3    |
| "Road-650 Red, 62"               | 10      | 2011   | 73            | 34          | 114.7    |
| "Road-650 Red, 62"               | 9       | 2011   | 34            | 36          | -5.6     |
| "Road-650 Red, 62"               | 8       | 2011   | 36            | 62          | -41.9    |
| "Road-650 Red, 62"               | 7       | 2011   | 62            | 19          | 226.3    |
| "Road-650 Red, 62"               | 6       | 2011   | 19            |             | 0.0      |
| Seat Stays                       | 12      | 2011   | 3682          | 7195        | -48.8    |
| Seat Stays                       | 11      | 2011   | 7195          | 9340        | -23.0    |
| Seat Stays                       | 10      | 2011   | 9340          | 4243        | 120.1    |
| Seat Stays                       | 9       | 2011   | 4243          | 4764        | -10.9    |
| Seat Stays                       | 8       | 2011   | 4764          | 6332        | -24.8    |
| Seat Stays                       | 7       | 2011   | 6332          | 2560        | 147.3    |
| Seat Stays                       | 6       | 2011   | 2560          |             | 0.0      |
| Seat Tube                        | 12      | 2011   | 921           | 1799        | -48.8    |
| Seat Tube                        | 11      | 2011   | 1799          | 2335        | -23.0    |
| Seat Tube                        | 10      | 2011   | 2335          | 1061        | 120.1    |
| Seat Tube                        | 9       | 2011   | 1061          | 1191        | -10.9    |
| Seat Tube                        | 8       | 2011   | 1191          | 1583        | -24.8    |
| Seat Tube                        | 7       | 2011   | 1583          | 640         | 147.3    |
| Seat Tube                        | 6       | 2011   | 640           |             | 0.0      |
| Steerer                          | 12      | 2011   | 921           | 1799        | -48.8    |
| Steerer                          | 11      | 2011   | 1799          | 2270        | -20.7    |
| Steerer                          | 10      | 2011   | 2270          | 1061        | 113.9    |
| Steerer                          | 9       | 2011   | 1061          | 1191        | -10.9    |
| Steerer                          | 8       | 2011   | 1191          | 1583        | -24.8    |
| Steerer                          | 7       | 2011   | 1583          | 640         | 147.3    |
| Steerer                          | 6       | 2011   | 640           |             | 0.0      |
| Stem                             | 12      | 2011   | 848           | 1455        | -41.7    |
| Stem                             | 11      | 2011   | 1455          | 1918        | -24.1    |
| Stem                             | 10      | 2011   | 1918          | 885         | 116.7    |
| Stem                             | 9       | 2011   | 885           | 967         | -8.5     |
| Stem                             | 8       | 2011   | 967           | 1284        | -24.7    |
| Stem                             | 7       | 2011   | 1284          | 526         | 144.1    |
| Stem                             | 6       | 2011   | 526           |             | 0.0      |
| Top Tube                         | 12      | 2011   | 921           | 1767        | -47.9    |
| Top Tube                         | 11      | 2011   | 1767          | 2335        | -24.3    |
| Top Tube                         | 10      | 2011   | 2335          | 1061        | 120.1    |
| Top Tube                         | 9       | 2011   | 1061          | 1191        | -10.9    |
| Top Tube                         | 8       | 2011   | 1191          | 1583        | -24.8    |
| Top Tube                         | 7       | 2011   | 1583          | 640         | 147.3    |
| Top Tube                         | 6       | 2011   | 640           |             | 0.0      |

**4.14. Calculate MoM Ratio of Stock / Sales in 2011 by product name**

```s
with sale_info as
(
select
      extract(month from a.ModifiedDate) as mth
      , extract(year from a.ModifiedDate) as yr
      , a.ProductId
      , b.Name
      , sum(a.OrderQty) as sales
from `adventureworks2019.Sales.SalesOrderDetail` a
left join `adventureworks2019.Production.Product` b on a.ProductID = b.ProductID
where FORMAT_TIMESTAMP("%Y", a.ModifiedDate) = '2011'
group by 1,2,3,4
)
, stock_info as
(
select distinct extract(month from ModifiedDate) as mth
      , extract(year from ModifiedDate) as yr
      , ProductId
      , sum(StockedQty) as stock_cnt
from adventureworks2019.Production.WorkOrder
where FORMAT_TIMESTAMP("%Y", ModifiedDate) = '2011'
group by 1,2,3
)
select distinct a.*
      , coalesce(b.stock_cnt,0) as stock
      , round(coalesce(b.stock_cnt,0) / sales,2) as ratio
from sale_info a
left join stock_info b on a.ProductId = b.ProductId
and a.mth = b.mth
and a.yr = b.yr
order by 1 desc, 7 desc
```

| **mth** | **yr** | **ProductId** | **Name**                         | **sales** | **stock** | **ratio** |
|---------|--------|---------------|----------------------------------|-----------|-----------|-----------|
| 12      | 2011   | 745           | "HL Mountain Frame - Black, 48"  | 1         | 27        | 27.0      |
| 12      | 2011   | 743           | "HL Mountain Frame - Black, 42"  | 1         | 26        | 26.0      |
| 12      | 2011   | 748           | "HL Mountain Frame - Silver, 38" | 2         | 32        | 16.0      |
| 12      | 2011   | 722           | "LL Road Frame - Black, 58"      | 4         | 47        | 11.75     |
| 12      | 2011   | 747           | "HL Mountain Frame - Black, 38"  | 3         | 31        | 10.33     |
| 12      | 2011   | 726           | "LL Road Frame - Red, 48"        | 5         | 36        | 7.2       |
| 12      | 2011   | 738           | "LL Road Frame - Black, 52"      | 10        | 64        | 6.4       |
| 12      | 2011   | 730           | "LL Road Frame - Red, 62"        | 7         | 38        | 5.43      |
| 12      | 2011   | 741           | "HL Mountain Frame - Silver, 48" | 5         | 27        | 5.4       |
| 12      | 2011   | 725           | "LL Road Frame - Red, 44"        | 12        | 53        | 4.42      |
| 12      | 2011   | 729           | "LL Road Frame - Red, 60"        | 10        | 43        | 4.3       |
| 12      | 2011   | 732           | "ML Road Frame - Red, 48"        | 10        | 16        | 1.6       |
| 12      | 2011   | 750           | "Road-150 Red, 44"               | 25        | 38        | 1.52      |
| 12      | 2011   | 751           | "Road-150 Red, 48"               | 32        | 47        | 1.47      |
| 12      | 2011   | 775           | "Mountain-100 Black, 38"         | 23        | 28        | 1.22      |
| 12      | 2011   | 773           | "Mountain-100 Silver, 44"        | 32        | 36        | 1.13      |
| 12      | 2011   | 749           | "Road-150 Red, 62"               | 45        | 51        | 1.13      |
| 12      | 2011   | 768           | "Road-650 Black, 44"             | 19        | 21        | 1.11      |
| 12      | 2011   | 765           | "Road-650 Black, 58"             | 39        | 43        | 1.1       |
| 12      | 2011   | 752           | "Road-150 Red, 52"               | 32        | 35        | 1.09      |
| 12      | 2011   | 778           | "Mountain-100 Black, 48"         | 25        | 27        | 1.08      |
| 12      | 2011   | 760           | "Road-650 Red, 60"               | 31        | 33        | 1.06      |
| 12      | 2011   | 770           | "Road-650 Black, 52"             | 52        | 53        | 1.02      |
| 12      | 2011   | 742           | "HL Mountain Frame - Silver, 46" | 3         | 3         | 1.0       |
| 12      | 2011   | 754           | "Road-450 Red, 58"               | 29        | 29        | 1.0       |
| 12      | 2011   | 755           | "Road-450 Red, 60"               | 18        | 18        | 1.0       |
| 12      | 2011   | 756           | "Road-450 Red, 44"               | 23        | 23        | 1.0       |
| 12      | 2011   | 757           | "Road-450 Red, 48"               | 6         | 6         | 1.0       |
| 12      | 2011   | 758           | "Road-450 Red, 52"               | 37        | 37        | 1.0       |
| 12      | 2011   | 759           | "Road-650 Red, 58"               | 12        | 12        | 1.0       |
| 12      | 2011   | 761           | "Road-650 Red, 62"               | 31        | 31        | 1.0       |
| 12      | 2011   | 764           | "Road-650 Red, 52"               | 23        | 23        | 1.0       |
| 12      | 2011   | 762           | "Road-650 Red, 44"               | 41        | 40        | 0.98      |
| 12      | 2011   | 777           | "Mountain-100 Black, 44"         | 29        | 28        | 0.97      |
| 12      | 2011   | 774           | "Mountain-100 Silver, 48"        | 22        | 21        | 0.95      |
| 12      | 2011   | 772           | "Mountain-100 Silver, 42"        | 18        | 17        | 0.94      |
| 12      | 2011   | 753           | "Road-150 Red, 56"               | 49        | 46        | 0.94      |
| 12      | 2011   | 763           | "Road-650 Red, 48"               | 33        | 31        | 0.94      |
| 12      | 2011   | 776           | "Mountain-100 Black, 42"         | 24        | 22        | 0.92      |
| 12      | 2011   | 771           | "Mountain-100 Silver, 38"        | 33        | 30        | 0.91      |
| 12      | 2011   | 767           | "Road-650 Black, 62"             | 10        | 9         | 0.9       |
| 12      | 2011   | 769           | "Road-650 Black, 48"             | 9         | 8         | 0.89      |
| 12      | 2011   | 766           | "Road-650 Black, 60"             | 22        | 18        | 0.82      |
| 12      | 2011   | 707           | "Sport-100 Helmet, Red"          | 12        | 0         | 0.0       |
| 12      | 2011   | 708           | "Sport-100 Helmet, Black"        | 10        | 0         | 0.0       |
| 12      | 2011   | 709           | "Mountain Bike Socks, M"         | 45        | 0         | 0.0       |
| 12      | 2011   | 711           | "Sport-100 Helmet, Blue"         | 7         | 0         | 0.0       |
| 12      | 2011   | 712           | AWC Logo Cap                     | 25        | 0         | 0.0       |
| 12      | 2011   | 714           | "Long-Sleeve Logo Jersey, M"     | 9         | 0         | 0.0       |
| 12      | 2011   | 715           | "Long-Sleeve Logo Jersey, L"     | 29        | 0         | 0.0       |
| 12      | 2011   | 716           | "Long-Sleeve Logo Jersey, XL"    | 6         | 0         | 0.0       |
| 11      | 2011   | 761           | "Road-650 Red, 62"               | 1         | 56        | 56.0      |
| 11      | 2011   | 764           | "Road-650 Red, 52"               | 1         | 30        | 30.0      |
| 11      | 2011   | 772           | "Mountain-100 Silver, 42"        | 2         | 48        | 24.0      |
| 11      | 2011   | 767           | "Road-650 Black, 62"             | 1         | 21        | 21.0      |
| 11      | 2011   | 763           | "Road-650 Red, 48"               | 3         | 54        | 18.0      |
| 11      | 2011   | 760           | "Road-650 Red, 60"               | 4         | 67        | 16.75     |
| 11      | 2011   | 769           | "Road-650 Black, 48"             | 1         | 15        | 15.0      |
| 11      | 2011   | 770           | "Road-650 Black, 52"             | 4         | 56        | 14.0      |
| 11      | 2011   | 771           | "Mountain-100 Silver, 38"        | 4         | 55        | 13.75     |
| 11      | 2011   | 776           | "Mountain-100 Black, 42"         | 5         | 67        | 13.4      |
| 11      | 2011   | 766           | "Road-650 Black, 60"             | 2         | 25        | 12.5      |
| 11      | 2011   | 765           | "Road-650 Black, 58"             | 4         | 49        | 12.25     |
| 11      | 2011   | 778           | "Mountain-100 Black, 48"         | 5         | 59        | 11.8      |
| 11      | 2011   | 773           | "Mountain-100 Silver, 44"        | 6         | 63        | 10.5      |
| 11      | 2011   | 759           | "Road-650 Red, 58"               | 2         | 19        | 9.5       |
| 11      | 2011   | 777           | "Mountain-100 Black, 44"         | 8         | 69        | 8.63      |
| 11      | 2011   | 775           | "Mountain-100 Black, 38"         | 8         | 66        | 8.25      |
| 11      | 2011   | 768           | "Road-650 Black, 44"             | 4         | 27        | 6.75      |
| 11      | 2011   | 753           | "Road-150 Red, 56"               | 31        | 55        | 1.77      |
| 11      | 2011   | 752           | "Road-150 Red, 52"               | 25        | 41        | 1.64      |
| 11      | 2011   | 749           | "Road-150 Red, 62"               | 35        | 54        | 1.54      |
| 11      | 2011   | 750           | "Road-150 Red, 44"               | 34        | 45        | 1.32      |
| 11      | 2011   | 751           | "Road-150 Red, 48"               | 40        | 41        | 1.02      |
| 10      | 2011   | 723           | "LL Road Frame - Black, 60"      | 1         | 46        | 46.0      |
| 10      | 2011   | 744           | "HL Mountain Frame - Black, 44"  | 6         | 100       | 16.67     |
| 10      | 2011   | 739           | "HL Mountain Frame - Silver, 42" | 7         | 97        | 13.86     |
| 10      | 2011   | 727           | "LL Road Frame - Red, 52"        | 4         | 50        | 12.5      |
| 10      | 2011   | 717           | "HL Road Frame - Red, 62"        | 8         | 66        | 8.25      |
| 10      | 2011   | 718           | "HL Road Frame - Red, 44"        | 6         | 41        | 6.83      |
| 10      | 2011   | 736           | "LL Road Frame - Black, 44"      | 9         | 45        | 5.0       |
| 10      | 2011   | 733           | "ML Road Frame - Red, 52"        | 26        | 102       | 3.92      |
| 10      | 2011   | 745           | "HL Mountain Frame - Black, 48"  | 33        | 96        | 2.91      |
| 10      | 2011   | 747           | "HL Mountain Frame - Black, 38"  | 35        | 94        | 2.69      |
| 10      | 2011   | 748           | "HL Mountain Frame - Silver, 38" | 46        | 112       | 2.43      |
| 10      | 2011   | 743           | "HL Mountain Frame - Black, 42"  | 45        | 96        | 2.13      |
| 10      | 2011   | 741           | "HL Mountain Frame - Silver, 48" | 45        | 90        | 2.0       |
| 10      | 2011   | 738           | "LL Road Frame - Black, 52"      | 69        | 133       | 1.93      |
| 10      | 2011   | 722           | "LL Road Frame - Black, 58"      | 57        | 107       | 1.88      |
| 10      | 2011   | 729           | "LL Road Frame - Red, 60"        | 70        | 126       | 1.8       |
| 10      | 2011   | 726           | "LL Road Frame - Red, 48"        | 63        | 107       | 1.7       |
| 10      | 2011   | 730           | "LL Road Frame - Red, 62"        | 62        | 105       | 1.69      |
| 10      | 2011   | 725           | "LL Road Frame - Red, 44"        | 67        | 106       | 1.58      |
| 10      | 2011   | 732           | "ML Road Frame - Red, 48"        | 70        | 68        | 0.97      |
| 10      | 2011   | 751           | "Road-150 Red, 48"               | 57        | 45        | 0.79      |
| 10      | 2011   | 749           | "Road-150 Red, 62"               | 92        | 67        | 0.73      |
| 10      | 2011   | 752           | "Road-150 Red, 52"               | 75        | 53        | 0.71      |
| 10      | 2011   | 753           | "Road-150 Red, 56"               | 104       | 74        | 0.71      |
| 10      | 2011   | 772           | "Mountain-100 Silver, 42"        | 141       | 96        | 0.68      |
| 10      | 2011   | 766           | "Road-650 Black, 60"             | 66        | 45        | 0.68      |
| 10      | 2011   | 769           | "Road-650 Black, 48"             | 45        | 30        | 0.67      |
| 10      | 2011   | 757           | "Road-450 Red, 48"               | 43        | 29        | 0.67      |
| 10      | 2011   | 759           | "Road-650 Red, 58"               | 49        | 33        | 0.67      |
| 10      | 2011   | 764           | "Road-650 Red, 52"               | 78        | 50        | 0.64      |
| 10      | 2011   | 767           | "Road-650 Black, 62"             | 55        | 35        | 0.64      |
| 10      | 2011   | 768           | "Road-650 Black, 44"             | 68        | 42        | 0.62      |
| 10      | 2011   | 771           | "Mountain-100 Silver, 38"        | 141       | 88        | 0.62      |
| 10      | 2011   | 750           | "Road-150 Red, 44"               | 65        | 40        | 0.62      |
| 10      | 2011   | 770           | "Road-650 Black, 52"             | 145       | 89        | 0.61      |
| 10      | 2011   | 777           | "Mountain-100 Black, 44"         | 158       | 96        | 0.61      |
| 10      | 2011   | 754           | "Road-450 Red, 58"               | 137       | 82        | 0.6       |
| 10      | 2011   | 765           | "Road-650 Black, 58"             | 117       | 70        | 0.6       |
| 10      | 2011   | 778           | "Mountain-100 Black, 48"         | 134       | 79        | 0.59      |
| 10      | 2011   | 755           | "Road-450 Red, 60"               | 79        | 47        | 0.59      |
| 10      | 2011   | 775           | "Mountain-100 Black, 38"         | 145       | 82        | 0.57      |
| 10      | 2011   | 760           | "Road-650 Red, 60"               | 154       | 88        | 0.57      |
| 10      | 2011   | 761           | "Road-650 Red, 62"               | 129       | 73        | 0.57      |
| 10      | 2011   | 763           | "Road-650 Red, 48"               | 120       | 68        | 0.57      |
| 10      | 2011   | 756           | "Road-450 Red, 44"               | 82        | 46        | 0.56      |
| 10      | 2011   | 774           | "Mountain-100 Silver, 48"        | 123       | 68        | 0.55      |
| 10      | 2011   | 758           | "Road-450 Red, 52"               | 157       | 87        | 0.55      |
| 10      | 2011   | 776           | "Mountain-100 Black, 42"         | 140       | 76        | 0.54      |
| 10      | 2011   | 773           | "Mountain-100 Silver, 44"        | 132       | 70        | 0.53      |
| 10      | 2011   | 742           | "HL Mountain Frame - Silver, 46" | 32        | 17        | 0.53      |
| 10      | 2011   | 762           | "Road-650 Red, 44"               | 156       | 64        | 0.41      |
| 10      | 2011   | 707           | "Sport-100 Helmet, Red"          | 141       | 0         | 0.0       |
| 10      | 2011   | 708           | "Sport-100 Helmet, Black"        | 162       | 0         | 0.0       |
| 10      | 2011   | 709           | "Mountain Bike Socks, M"         | 224       | 0         | 0.0       |
| 10      | 2011   | 710           | "Mountain Bike Socks, L"         | 29        | 0         | 0.0       |
| 10      | 2011   | 711           | "Sport-100 Helmet, Blue"         | 181       | 0         | 0.0       |
| 10      | 2011   | 712           | AWC Logo Cap                     | 240       | 0         | 0.0       |
| 10      | 2011   | 714           | "Long-Sleeve Logo Jersey, M"     | 101       | 0         | 0.0       |
| 10      | 2011   | 715           | "Long-Sleeve Logo Jersey, L"     | 239       | 0         | 0.0       |
| 10      | 2011   | 716           | "Long-Sleeve Logo Jersey, XL"    | 117       | 0         | 0.0       |
| 9       | 2011   | 763           | "Road-650 Red, 48"               | 1         | 41        | 41.0      |
| 9       | 2011   | 761           | "Road-650 Red, 62"               | 1         | 34        | 34.0      |
| 9       | 2011   | 771           | "Mountain-100 Silver, 38"        | 1         | 25        | 25.0      |
| 9       | 2011   | 773           | "Mountain-100 Silver, 44"        | 1         | 24        | 24.0      |
| 9       | 2011   | 765           | "Road-650 Black, 58"             | 2         | 43        | 21.5      |
| 9       | 2011   | 760           | "Road-650 Red, 60"               | 2         | 40        | 20.0      |
| 9       | 2011   | 762           | "Road-650 Red, 44"               | 2         | 38        | 19.0      |
| 9       | 2011   | 775           | "Mountain-100 Black, 38"         | 2         | 34        | 17.0      |
| 9       | 2011   | 774           | "Mountain-100 Silver, 48"        | 2         | 26        | 13.0      |
| 9       | 2011   | 766           | "Road-650 Black, 60"             | 3         | 32        | 10.67     |
| 9       | 2011   | 764           | "Road-650 Red, 52"               | 2         | 21        | 10.5      |
| 9       | 2011   | 776           | "Mountain-100 Black, 42"         | 3         | 30        | 10.0      |
| 9       | 2011   | 777           | "Mountain-100 Black, 44"         | 3         | 26        | 8.67      |
| 9       | 2011   | 767           | "Road-650 Black, 62"             | 2         | 16        | 8.0       |
| 9       | 2011   | 778           | "Mountain-100 Black, 48"         | 7         | 23        | 3.29      |
| 9       | 2011   | 772           | "Mountain-100 Silver, 42"        | 7         | 20        | 2.86      |
| 9       | 2011   | 753           | "Road-150 Red, 56"               | 23        | 44        | 1.91      |
| 9       | 2011   | 759           | "Road-650 Red, 58"               | 4         | 7         | 1.75      |
| 9       | 2011   | 750           | "Road-150 Red, 44"               | 21        | 35        | 1.67      |
| 9       | 2011   | 749           | "Road-150 Red, 62"               | 27        | 44        | 1.63      |
| 9       | 2011   | 752           | "Road-150 Red, 52"               | 18        | 29        | 1.61      |
| 9       | 2011   | 751           | "Road-150 Red, 48"               | 23        | 35        | 1.52      |
| 8       | 2011   | 744           | "HL Mountain Frame - Black, 44"  | 3         | 60        | 20.0      |
| 8       | 2011   | 727           | "LL Road Frame - Red, 52"        | 1         | 14        | 14.0      |
| 8       | 2011   | 717           | "HL Road Frame - Red, 62"        | 5         | 42        | 8.4       |
| 8       | 2011   | 739           | "HL Mountain Frame - Silver, 42" | 6         | 49        | 8.17      |
| 8       | 2011   | 736           | "LL Road Frame - Black, 44"      | 4         | 18        | 4.5       |
| 8       | 2011   | 747           | "HL Mountain Frame - Black, 38"  | 17        | 69        | 4.06      |
| 8       | 2011   | 718           | "HL Road Frame - Red, 44"        | 5         | 19        | 3.8       |
| 8       | 2011   | 748           | "HL Mountain Frame - Silver, 38" | 21        | 73        | 3.48      |
| 8       | 2011   | 745           | "HL Mountain Frame - Black, 48"  | 18        | 61        | 3.39      |
| 8       | 2011   | 733           | "ML Road Frame - Red, 52"        | 17        | 51        | 3.0       |
| 8       | 2011   | 741           | "HL Mountain Frame - Silver, 48" | 24        | 57        | 2.38      |
| 8       | 2011   | 743           | "HL Mountain Frame - Black, 42"  | 28        | 64        | 2.29      |
| 8       | 2011   | 726           | "LL Road Frame - Red, 48"        | 18        | 41        | 2.28      |
| 8       | 2011   | 730           | "LL Road Frame - Red, 62"        | 23        | 44        | 1.91      |
| 8       | 2011   | 729           | "LL Road Frame - Red, 60"        | 44        | 72        | 1.64      |
| 8       | 2011   | 738           | "LL Road Frame - Black, 52"      | 44        | 71        | 1.61      |
| 8       | 2011   | 722           | "LL Road Frame - Black, 58"      | 23        | 34        | 1.48      |
| 8       | 2011   | 725           | "LL Road Frame - Red, 44"        | 53        | 78        | 1.47      |
| 8       | 2011   | 742           | "HL Mountain Frame - Silver, 46" | 21        | 18        | 0.86      |
| 8       | 2011   | 749           | "Road-150 Red, 62"               | 55        | 44        | 0.8       |
| 8       | 2011   | 771           | "Mountain-100 Silver, 38"        | 77        | 60        | 0.78      |
| 8       | 2011   | 778           | "Mountain-100 Black, 48"         | 61        | 46        | 0.75      |
| 8       | 2011   | 772           | "Mountain-100 Silver, 42"        | 66        | 49        | 0.74      |
| 8       | 2011   | 752           | "Road-150 Red, 52"               | 33        | 24        | 0.73      |
| 8       | 2011   | 777           | "Mountain-100 Black, 44"         | 80        | 58        | 0.72      |
| 8       | 2011   | 753           | "Road-150 Red, 56"               | 66        | 46        | 0.7       |
| 8       | 2011   | 773           | "Mountain-100 Silver, 44"        | 74        | 50        | 0.68      |
| 8       | 2011   | 776           | "Mountain-100 Black, 42"         | 69        | 44        | 0.64      |
| 8       | 2011   | 775           | "Mountain-100 Black, 38"         | 82        | 52        | 0.63      |
| 8       | 2011   | 774           | "Mountain-100 Silver, 48"        | 61        | 37        | 0.61      |
| 8       | 2011   | 750           | "Road-150 Red, 44"               | 34        | 20        | 0.59      |
| 8       | 2011   | 754           | "Road-450 Red, 58"               | 63        | 37        | 0.59      |
| 8       | 2011   | 762           | "Road-650 Red, 44"               | 86        | 49        | 0.57      |
| 8       | 2011   | 732           | "ML Road Frame - Red, 48"        | 25        | 14        | 0.56      |
| 8       | 2011   | 758           | "Road-450 Red, 52"               | 77        | 43        | 0.56      |
| 8       | 2011   | 755           | "Road-450 Red, 60"               | 49        | 27        | 0.55      |
| 8       | 2011   | 760           | "Road-650 Red, 60"               | 86        | 47        | 0.55      |
| 8       | 2011   | 756           | "Road-450 Red, 44"               | 40        | 21        | 0.53      |
| 8       | 2011   | 761           | "Road-650 Red, 62"               | 69        | 36        | 0.52      |
| 8       | 2011   | 770           | "Road-650 Black, 52"             | 95        | 48        | 0.51      |
| 8       | 2011   | 751           | "Road-150 Red, 48"               | 39        | 18        | 0.46      |
| 8       | 2011   | 763           | "Road-650 Red, 48"               | 74        | 34        | 0.46      |
| 8       | 2011   | 768           | "Road-650 Black, 44"             | 42        | 17        | 0.4       |
| 8       | 2011   | 764           | "Road-650 Red, 52"               | 35        | 14        | 0.4       |
| 8       | 2011   | 765           | "Road-650 Black, 58"             | 70        | 27        | 0.39      |
| 8       | 2011   | 766           | "Road-650 Black, 60"             | 49        | 18        | 0.37      |
| 8       | 2011   | 759           | "Road-650 Red, 58"               | 5         | 1         | 0.2       |
| 8       | 2011   | 767           | "Road-650 Black, 62"             | 16        | 2         | 0.13      |
| 8       | 2011   | 769           | "Road-650 Black, 48"             | 19        | 0         | 0.0       |
| 8       | 2011   | 707           | "Sport-100 Helmet, Red"          | 96        | 0         | 0.0       |
| 8       | 2011   | 708           | "Sport-100 Helmet, Black"        | 86        | 0         | 0.0       |
| 8       | 2011   | 709           | "Mountain Bike Socks, M"         | 167       | 0         | 0.0       |
| 8       | 2011   | 710           | "Mountain Bike Socks, L"         | 19        | 0         | 0.0       |
| 8       | 2011   | 711           | "Sport-100 Helmet, Blue"         | 75        | 0         | 0.0       |
| 8       | 2011   | 712           | AWC Logo Cap                     | 137       | 0         | 0.0       |
| 8       | 2011   | 714           | "Long-Sleeve Logo Jersey, M"     | 65        | 0         | 0.0       |
| 8       | 2011   | 715           | "Long-Sleeve Logo Jersey, L"     | 113       | 0         | 0.0       |
| 8       | 2011   | 716           | "Long-Sleeve Logo Jersey, XL"    | 65        | 0         | 0.0       |
| 8       | 2011   | 757           | "Road-450 Red, 48"               | 9         | 0         | 0.0       |
| 7       | 2011   | 733           | "ML Road Frame - Red, 52"        | 8         | 78        | 9.75      |
| 7       | 2011   | 743           | "HL Mountain Frame - Black, 42"  | 13        | 91        | 7.0       |
| 7       | 2011   | 747           | "HL Mountain Frame - Black, 38"  | 14        | 85        | 6.07      |
| 7       | 2011   | 730           | "LL Road Frame - Red, 62"        | 13        | 75        | 5.77      |
| 7       | 2011   | 748           | "HL Mountain Frame - Silver, 38" | 20        | 96        | 4.8       |
| 7       | 2011   | 745           | "HL Mountain Frame - Black, 48"  | 19        | 83        | 4.37      |
| 7       | 2011   | 741           | "HL Mountain Frame - Silver, 48" | 14        | 58        | 4.14      |
| 7       | 2011   | 726           | "LL Road Frame - Red, 48"        | 19        | 73        | 3.84      |
| 7       | 2011   | 722           | "LL Road Frame - Black, 58"      | 20        | 67        | 3.35      |
| 7       | 2011   | 738           | "LL Road Frame - Black, 52"      | 39        | 126       | 3.23      |
| 7       | 2011   | 725           | "LL Road Frame - Red, 44"        | 38        | 104       | 2.74      |
| 7       | 2011   | 729           | "LL Road Frame - Red, 60"        | 41        | 112       | 2.73      |
| 7       | 2011   | 751           | "Road-150 Red, 48"               | 18        | 34        | 1.89      |
| 7       | 2011   | 750           | "Road-150 Red, 44"               | 15        | 17        | 1.13      |
| 7       | 2011   | 773           | "Mountain-100 Silver, 44"        | 57        | 62        | 1.09      |
| 7       | 2011   | 764           | "Road-650 Red, 52"               | 15        | 16        | 1.07      |
| 7       | 2011   | 774           | "Mountain-100 Silver, 48"        | 42        | 44        | 1.05      |
| 7       | 2011   | 772           | "Mountain-100 Silver, 42"        | 57        | 59        | 1.04      |
| 7       | 2011   | 765           | "Road-650 Black, 58"             | 47        | 49        | 1.04      |
| 7       | 2011   | 777           | "Mountain-100 Black, 44"         | 66        | 68        | 1.03      |
| 7       | 2011   | 762           | "Road-650 Red, 44"               | 64        | 66        | 1.03      |
| 7       | 2011   | 761           | "Road-650 Red, 62"               | 61        | 62        | 1.02      |
| 7       | 2011   | 768           | "Road-650 Black, 44"             | 19        | 19        | 1.0       |
| 7       | 2011   | 775           | "Mountain-100 Black, 38"         | 71        | 71        | 1.0       |
| 7       | 2011   | 732           | "ML Road Frame - Red, 48"        | 26        | 26        | 1.0       |
| 7       | 2011   | 742           | "HL Mountain Frame - Silver, 46" | 15        | 15        | 1.0       |
| 7       | 2011   | 754           | "Road-450 Red, 58"               | 54        | 54        | 1.0       |
| 7       | 2011   | 755           | "Road-450 Red, 60"               | 22        | 22        | 1.0       |
| 7       | 2011   | 756           | "Road-450 Red, 44"               | 21        | 21        | 1.0       |
| 7       | 2011   | 758           | "Road-450 Red, 52"               | 72        | 72        | 1.0       |
| 7       | 2011   | 763           | "Road-650 Red, 48"               | 54        | 54        | 1.0       |
| 7       | 2011   | 760           | "Road-650 Red, 60"               | 71        | 70        | 0.99      |
| 7       | 2011   | 770           | "Road-650 Black, 52"             | 88        | 86        | 0.98      |
| 7       | 2011   | 778           | "Mountain-100 Black, 48"         | 64        | 63        | 0.98      |
| 7       | 2011   | 776           | "Mountain-100 Black, 42"         | 81        | 77        | 0.95      |
| 7       | 2011   | 771           | "Mountain-100 Silver, 38"        | 81        | 76        | 0.94      |
| 7       | 2011   | 766           | "Road-650 Black, 60"             | 17        | 16        | 0.94      |
| 7       | 2011   | 753           | "Road-150 Red, 56"               | 61        | 55        | 0.9       |
| 7       | 2011   | 749           | "Road-150 Red, 62"               | 41        | 29        | 0.71      |
| 7       | 2011   | 752           | "Road-150 Red, 52"               | 21        | 15        | 0.71      |
| 7       | 2011   | 767           | "Road-650 Black, 62"             | 2         | 1         | 0.5       |
| 7       | 2011   | 707           | "Sport-100 Helmet, Red"          | 58        | 0         | 0.0       |
| 7       | 2011   | 708           | "Sport-100 Helmet, Black"        | 56        | 0         | 0.0       |
| 7       | 2011   | 709           | "Mountain Bike Socks, M"         | 134       | 0         | 0.0       |
| 7       | 2011   | 710           | "Mountain Bike Socks, L"         | 13        | 0         | 0.0       |
| 7       | 2011   | 711           | "Sport-100 Helmet, Blue"         | 64        | 0         | 0.0       |
| 7       | 2011   | 712           | AWC Logo Cap                     | 103       | 0         | 0.0       |
| 7       | 2011   | 714           | "Long-Sleeve Logo Jersey, M"     | 37        | 0         | 0.0       |
| 7       | 2011   | 715           | "Long-Sleeve Logo Jersey, L"     | 114       | 0         | 0.0       |
| 7       | 2011   | 716           | "Long-Sleeve Logo Jersey, XL"    | 48        | 0         | 0.0       |
| 7       | 2011   | 759           | "Road-650 Red, 58"               | 1         | 0         | 0.0       |
| 6       | 2011   | 762           | "Road-650 Red, 44"               | 2         | 44        | 22.0      |
| 6       | 2011   | 763           | "Road-650 Red, 48"               | 1         | 20        | 20.0      |
| 6       | 2011   | 761           | "Road-650 Red, 62"               | 1         | 19        | 19.0      |
| 6       | 2011   | 776           | "Mountain-100 Black, 42"         | 1         | 16        | 16.0      |
| 6       | 2011   | 770           | "Road-650 Black, 52"             | 2         | 29        | 14.5      |
| 6       | 2011   | 765           | "Road-650 Black, 58"             | 3         | 31        | 10.33     |
| 6       | 2011   | 764           | "Road-650 Red, 52"               | 2         | 15        | 7.5       |
| 6       | 2011   | 775           | "Mountain-100 Black, 38"         | 3         | 22        | 7.33      |
| 6       | 2011   | 778           | "Mountain-100 Black, 48"         | 3         | 21        | 7.0       |
| 6       | 2011   | 768           | "Road-650 Black, 44"             | 2         | 13        | 6.5       |
| 6       | 2011   | 777           | "Mountain-100 Black, 44"         | 6         | 23        | 3.83      |
| 6       | 2011   | 773           | "Mountain-100 Silver, 44"        | 6         | 18        | 3.0       |
| 6       | 2011   | 771           | "Mountain-100 Silver, 38"        | 4         | 11        | 2.75      |
| 6       | 2011   | 774           | "Mountain-100 Silver, 48"        | 3         | 8         | 2.67      |
| 6       | 2011   | 772           | "Mountain-100 Silver, 42"        | 2         | 5         | 2.5       |
| 6       | 2011   | 767           | "Road-650 Black, 62"             | 1         | 2         | 2.0       |
| 6       | 2011   | 753           | "Road-150 Red, 56"               | 15        | 21        | 1.4       |
| 6       | 2011   | 749           | "Road-150 Red, 62"               | 21        | 13        | 0.62      |
| 6       | 2011   | 750           | "Road-150 Red, 44"               | 23        | 10        | 0.43      |
| 6       | 2011   | 752           | "Road-150 Red, 52"               | 12        | 4         | 0.33      |
| 6       | 2011   | 751           | "Road-150 Red, 48"               | 28        | 4         | 0.14      |
| 5       | 2011   | 768           | "Road-650 Black, 44"             | 13        | 0         | 0.0       |
| 5       | 2011   | 770           | "Road-650 Black, 52"             | 29        | 0         | 0.0       |
| 5       | 2011   | 771           | "Mountain-100 Silver, 38"        | 10        | 0         | 0.0       |
| 5       | 2011   | 772           | "Mountain-100 Silver, 42"        | 5         | 0         | 0.0       |
| 5       | 2011   | 773           | "Mountain-100 Silver, 44"        | 17        | 0         | 0.0       |
| 5       | 2011   | 774           | "Mountain-100 Silver, 48"        | 7         | 0         | 0.0       |
| 5       | 2011   | 775           | "Mountain-100 Black, 38"         | 22        | 0         | 0.0       |
| 5       | 2011   | 776           | "Mountain-100 Black, 42"         | 16        | 0         | 0.0       |
| 5       | 2011   | 777           | "Mountain-100 Black, 44"         | 23        | 0         | 0.0       |
| 5       | 2011   | 778           | "Mountain-100 Black, 48"         | 20        | 0         | 0.0       |
| 5       | 2011   | 707           | "Sport-100 Helmet, Red"          | 24        | 0         | 0.0       |
| 5       | 2011   | 708           | "Sport-100 Helmet, Black"        | 27        | 0         | 0.0       |
| 5       | 2011   | 709           | "Mountain Bike Socks, M"         | 38        | 0         | 0.0       |
| 5       | 2011   | 710           | "Mountain Bike Socks, L"         | 5         | 0         | 0.0       |
| 5       | 2011   | 711           | "Sport-100 Helmet, Blue"         | 33        | 0         | 0.0       |
| 5       | 2011   | 712           | AWC Logo Cap                     | 40        | 0         | 0.0       |
| 5       | 2011   | 714           | "Long-Sleeve Logo Jersey, M"     | 16        | 0         | 0.0       |
| 5       | 2011   | 715           | "Long-Sleeve Logo Jersey, L"     | 49        | 0         | 0.0       |
| 5       | 2011   | 716           | "Long-Sleeve Logo Jersey, XL"    | 19        | 0         | 0.0       |
| 5       | 2011   | 722           | "LL Road Frame - Black, 58"      | 8         | 0         | 0.0       |
| 5       | 2011   | 725           | "LL Road Frame - Red, 44"        | 15        | 0         | 0.0       |
| 5       | 2011   | 726           | "LL Road Frame - Red, 48"        | 9         | 0         | 0.0       |
| 5       | 2011   | 729           | "LL Road Frame - Red, 60"        | 16        | 0         | 0.0       |
| 5       | 2011   | 730           | "LL Road Frame - Red, 62"        | 14        | 0         | 0.0       |
| 5       | 2011   | 732           | "ML Road Frame - Red, 48"        | 16        | 0         | 0.0       |
| 5       | 2011   | 733           | "ML Road Frame - Red, 52"        | 4         | 0         | 0.0       |
| 5       | 2011   | 738           | "LL Road Frame - Black, 52"      | 19        | 0         | 0.0       |
| 5       | 2011   | 741           | "HL Mountain Frame - Silver, 48" | 2         | 0         | 0.0       |
| 5       | 2011   | 742           | "HL Mountain Frame - Silver, 46" | 3         | 0         | 0.0       |
| 5       | 2011   | 743           | "HL Mountain Frame - Black, 42"  | 1         | 0         | 0.0       |
| 5       | 2011   | 745           | "HL Mountain Frame - Black, 48"  | 1         | 0         | 0.0       |
| 5       | 2011   | 747           | "HL Mountain Frame - Black, 38"  | 4         | 0         | 0.0       |
| 5       | 2011   | 748           | "HL Mountain Frame - Silver, 38" | 2         | 0         | 0.0       |
| 5       | 2011   | 749           | "Road-150 Red, 62"               | 4         | 0         | 0.0       |
| 5       | 2011   | 753           | "Road-150 Red, 56"               | 14        | 0         | 0.0       |
| 5       | 2011   | 754           | "Road-450 Red, 58"               | 27        | 0         | 0.0       |
| 5       | 2011   | 755           | "Road-450 Red, 60"               | 11        | 0         | 0.0       |
| 5       | 2011   | 756           | "Road-450 Red, 44"               | 14        | 0         | 0.0       |
| 5       | 2011   | 758           | "Road-450 Red, 52"               | 46        | 0         | 0.0       |
| 5       | 2011   | 760           | "Road-650 Red, 60"               | 43        | 0         | 0.0       |
| 5       | 2011   | 761           | "Road-650 Red, 62"               | 19        | 0         | 0.0       |
| 5       | 2011   | 762           | "Road-650 Red, 44"               | 44        | 0         | 0.0       |
| 5       | 2011   | 763           | "Road-650 Red, 48"               | 20        | 0         | 0.0       |
| 5       | 2011   | 764           | "Road-650 Red, 52"               | 14        | 0         | 0.0       |
| 5       | 2011   | 765           | "Road-650 Black, 58"             | 30        | 0         | 0.0       |
| 5       | 2011   | 766           | "Road-650 Black, 60"             | 11        | 0         | 0.0       |
| 5       | 2011   | 767           | "Road-650 Black, 62"             | 1         | 0         | 0.0       |



