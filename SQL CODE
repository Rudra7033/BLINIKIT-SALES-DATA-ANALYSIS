select * from blinkit_data limit 1000;

select count(*) from blinkit_data;


SET SQL_SAFE_UPDATES = 0;
UPDATE blinkit_data
SET `Item Fat Content` =
    CASE
        WHEN `Item Fat Content` IN ('LF', 'low fat') THEN 'Low Fat'
        WHEN `Item Fat Content` = 'reg' THEN 'Regular'
        ELSE `Item Fat Content`
    END;
SET SQL_SAFE_UPDATES = 1;

SELECT DISTINCT `Item Fat Content` FROM blinkit_data;


-- KPI CALCULATIONS

-- Total sales in millions (2 decimals)
    SELECT
    CAST(SUM(`Total Sales`) / 1000000 AS DECIMAL(10,2)) AS Total_Sales_Million,  

   -- Average sales (whole number)
   CAST(AVG(`Total Sales`) AS DECIMAL(10,0))           AS Avg_Sales,  
   
   -- Number of orders
    COUNT(*)                                          AS No_of_Orders,         -- Number of orders
    
    -- Average rating (1 decimal)
    CAST(AVG(`Rating`) AS DECIMAL(10,1))                AS Avg_Rating            
FROM blinkit_data;

-- GRANULAR REQIREMENTS

-- TOTAL SALES,AVG_SALES,NO OF ITEMS,AVG RATING by Fat Content:

SELECT  `Item Fat Content`, 
    CAST(SUM(`Total Sales`) AS DECIMAL(10,2)) AS Total_Sales
FROM 
    blinkit_data
GROUP BY 
    `Item Fat Content`;
    
    
    
 SELECT  `Item Fat Content`, 
    CAST(SUM(`Total Sales`) AS DECIMAL(10,2)) AS Total_Sales,
    CAST(AVG(`TOTAL SALES`) AS DECIMAL(10,1)) AS AVG_Sales,
    COUNT(*) AS No_of_Items,
    CAST(AVG(Rating) AS DECIMAL(10,1)) AS Avg_Rating
FROM 
    blinkit_data
GROUP BY 
    `Item Fat Content`
    order by total_sales desc;
    
    
    --  TOTAL SALES,AVG_SALES,NO OF ITEMS,AVG RATING by Item Type
    
    SELECT  `Item type`, 
    CAST(SUM(`Total Sales`) AS DECIMAL(10,2)) AS Total_Sales,
    CAST(AVG(`TOTAL SALES`) AS DECIMAL(10,1)) AS AVG_Sales,
    COUNT(*) AS No_of_Items,
    CAST(AVG(Rating) AS DECIMAL(10,1)) AS Avg_Rating

FROM blinkit_data
GROUP BY `Item type`
order by total_sales desc;

-- fat content by outlet for total sales

 SELECT 
    `Outlet Location Type`,
    CAST(SUM(CASE WHEN `Item Fat Content` = 'Low Fat' THEN `Total Sales` ELSE 0 END) AS DECIMAL(10,2)) AS Low_Fat,
    CAST(SUM(CASE WHEN `Item Fat Content` = 'Regular' THEN `Total Sales` ELSE 0 END) AS DECIMAL(10,2)) AS Regular

FROM 
    blinkit_data
GROUP BY 
    `Outlet Location Type`
ORDER BY 
    `Outlet Location Type`;


-- total sales by outlet establishment year
    
SELECT `Outlet Establishment Year`, 
        CAST(SUM(`Total Sales`) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY `Outlet Establishment Year`
ORDER BY `Outlet Establishment Year`;


-- Percentage of Sales by Outlet Size WITH WINDOW FUNCTION OVER()

SELECT 
    `Outlet Size`, 
    CAST(SUM(`Total Sales`) AS DECIMAL(10,2)) AS Total_Sales,
    CAST(
        (SUM(`Total Sales`) * 100.0 / SUM(SUM(`Total Sales`)) OVER()) 
        AS DECIMAL(10,2)
    ) AS Sales_Percentage
FROM blinkit_data
GROUP BY `Outlet Size`
ORDER BY Total_Sales DESC;

    
    SELECT 
    `Outlet Size`, 
    SUM(`Total Sales`) AS Total_Sales
FROM 
    blinkit_data
GROUP BY 
    `Outlet Size`;

-- Percentage of Sales by Outlet Size WITHOUT WINDOW FUNCTION OVER()
    
SELECT 
    final.`Outlet Size`,
    final.Total_Sales,
    ROUND(final.Total_Sales * 100 / final.Total_All, 2) AS Sales_Percentage
FROM (
    SELECT 
        `Outlet Size`,
        SUM(`Total Sales`) AS Total_Sales,
        (SELECT SUM(`Total Sales`) FROM blinkit_data) AS Total_All
    FROM 
        blinkit_data
    GROUP BY 
        `Outlet Size`
) AS final
ORDER BY 
    final.Total_Sales DESC;
    

-- All Metrics by Outlet Type

SELECT 
    `Outlet Type`, 
    CAST(SUM(`Total Sales`) AS DECIMAL(10,2)) AS Total_Sales,
    CAST(AVG(`Total Sales`) AS DECIMAL(10,0)) AS Avg_Sales,
    COUNT(*) AS No_Of_Items,
    CAST(AVG(`Rating`) AS DECIMAL(10,2)) AS Avg_Rating,
    CAST(AVG(`Item Visibility`) AS DECIMAL(10,2)) AS Item_Visibility
FROM blinkit_data
GROUP BY `Outlet Type`
ORDER BY Total_Sales DESC;

