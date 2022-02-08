# USE of Count Distinct
## 1
```
SELECT 
    usertype,
    CONCAT (start_station_name, " to ", end_station_name) AS route,
    COUNT(*) as num_trips,
    ROUND(AVG(cast(tripduration as int64)/60),2) AS duration
 FROM `bigquery-public-data.new_york_citibike.citibike_trips` 
 GROUP BY start_station_name, end_station_name, usertype
 ORDER BY num_trips DESC 
 LIMIT 10
```
## 2
```
SELECT 
orders.*,
warehouse.warehouse_alias,
warehouse.state
FROM 
 `course-test-328223.warehouse_orders.orders`AS orders
JOIN 
 `course-test-328223.warehouse_orders.warehouse`warehouse ON orders.warehouse_id = warehouse.warehouse_id
```
## 3
```
 SELECT 
 COUNT(DISTINCT warehouse.state) as num_states
FROM 
 `course-test-328223.warehouse_orders.orders`AS orders
JOIN 
 `course-test-328223.warehouse_orders.warehouse`warehouse ON orders.warehouse_id = warehouse.warehouse_id
```

## 4
```
SELECT 
 warehouse.state as state,
 COUNT(DISTINCT order_id) as num_orders
FROM 
 `course-test-328223.warehouse_orders.orders`AS orders
JOIN 
 `course-test-328223.warehouse_orders.warehouse`warehouse ON orders.warehouse_id = warehouse.warehouse_id
 GROUP BY 
 warehouse.state
```

# Use of Join
## 1
```
SELECT 
    station_id,
    num_bikes_available,
    (SELECT 
        AVG(num_bikes_available)
    FROM bigquery-public-data.new_york_citibike.citibike_stations) AS avg_num_bikes_available
FROM 
 bigquery-public-data.new_york_citibike.citibike_stations
```

## 2
```
SELECT
    station_id,
    name,
    number_of_rides AS number_of_rides_starting_at_station
FROM
    (
    SELECT 
        start_station_id,
        COUNT(*) number_of_rides
    FROM 
        `bigquery-public-data.new_york_citibike.citibike_trips`
        GROUP BY 
            start_station_id
    )
AS station_num_trips
INNER JOIN 

`bigquery-public-data.new_york_citibike.citibike_stations` ON station_id = start_station_id
ORDER BY 
    number_of_rides DESC
```

## 3
```
SELECT 
    station_id,
    name
FROM 

`bigquery-public-data.new_york_citibike.citibike_stations`
WHERE 
    station_id IN

(
    SELECT 
        start_station_id
    FROM

    `bigquery-public-data.new_york_citibike.citibike_trips`

    WHERE
    usertype= 'Subscriber'
)
```

## 4
```
SELECT 
    Warehouse.warehouse_id,
    CONCAT(Warehouse.state, ':', Warehouse.warehouse_alias) AS warehouse_name, 
    COUNT(Orders.order_id) AS number_of_orders,
    (SELECT 
        COUNT(*)
    FROM `course-test-328223.warehouse_orders.orders` AS Orders)
    AS total_orders,
    CASE 
        WHEN COUNT (Orders.order_id)/(SELECT COUNT (*) FROM  `course-test-328223.warehouse_orders.orders` AS Orders) <= 0.20
        THEN "fulfilled 0-20% of Orders"
        WHEN COUNT (Orders.order_id)/(SELECT COUNT (*) FROM `course-test-328223.warehouse_orders.orders` ) > 0.20
        AND COUNT (Orders.order_id)/(SELECT COUNT (*) FROM `course-test-328223.warehouse_orders.orders`) <= 0.60
        THEN "fulfilled 21-60% of Orders"
    ELSE "fulfilled more than 60% of Orders"
    END AS fulfillment_summary
FROM `course-test-328223.warehouse_orders.warehouse` AS Warehouse
LEFT JOIN `course-test-328223.warehouse_orders.orders` AS Orders
    ON Orders.warehouse_id = Warehouse.warehouse_id
GROUP BY 
    Warehouse.warehouse_id,
    warehouse_name
HAVING 
    COUNT(Orders.order_id)>0
```

# Use of Calculations
# 1
```
SELECT 
    Date,
    Region,
    Small_Bags,
    Large_bags,
    XLarge_bags,
    Total_Bags,
    Small_Bags+ Large_Bags+ XLarge_Bags AS Total_Bags_Cal
FROM `course-test-328223.Avacado.avacado_price`
```

# 2
```
SELECT 
    Date,
    Region,
    Small_Bags,
    Total_Bags,
    (Small_Bags / Total_Bags) *100 AS Small_Bags_Percent
    FROM `course-test-328223.Avacado.avacado_price`
    WHERE 
    Total_Bags <> 0
               !=
```