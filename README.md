# Overview of Business Problem

A grocery store chain currently has 85 stores in California and is planning to open 10 new stores at the beginning of the year (2016). Currently, all stores use the same store format for selling their products. Up until now, the company has treated all stores similarly, shipping the same amount of product to each store. This is beginning to cause problems as stores are suffering from product surpluses in some product categories and shortages in others.

Files Available:

[Store Sales Data](https://github.com/omininiab/grocery-store-chain-expansion/blob/master/storesalesdata%20(1).csv) - This file contains sales by product category for all existing stores for 2012, 2013, and 2014.

[Store Information](https://github.com/omininiab/grocery-store-chain-expansion/blob/master/storeinformation%20(1).csv) - This file contains location data for each of the stores.

[Store Demographic Data](https://github.com/omininiab/grocery-store-chain-expansion/blob/master/storedemographicdata%20(3).csv) - This file contains demographic data for the areas surrounding each of the existing stores and locations for new stores.

# Problem Solving

To remedy the product surplus and shortages, I introduced different store formats, such that each format will carry a different product selection in order to better match local demand. To determine the optimal number of store formats, I used segmentation and clustering tools in [Alteryx](http://analytics.alteryx.com/alteryx), segmenting the stores based on sales data for the preceding year (2015). Using aggregated 2015 sales data for existing stores, showing the percent sales per category for each store, I ran a [K-centroids diagnostics tool](https://help.alteryx.com/current/K-Centroids_Diagnostics.htm) with the fields [standardized](http://downloads.alteryx.com/Alteryx/Help/Standardize_z-score.htm) and the clustering method set to K-Means. The minimum number of clusters tested was 2 and the maximum, 10. I used the fields containing the percent sales for each category as my determinant fields (segmentation variables). The [output](https://github.com/omininiab/grocery-store-chain-expansion/blob/master/%23clusters.png) showing the Rand and Calinski-Harabasz (CH) indices for each potential number of clusters was then observed to determine the optimum number of clusters to be 3.

![Rand and CH Indices](https://github.com/omininiab/grocery-store-chain-expansion/blob/master/%23clusters.png)

From the plots, the median Rand and CH indices for 3 clusters is the highest, indicating high stability and compactness within each cluster and high distinctness between different clusters. Knowing this, I then passed the data through a K-Centroids Cluster Analysis tool to assign each store to 1 of 3 clusters based on their standardized percent sales in all categories. Each cluster had 23, 29 and 33 stores, respectively. See [geographic distribution](https://public.tableau.com/views/Task1Visualizations/SegmentationandClustering?:embed=y&:display_count=yes) of the existing stores color coded for cluster and size coded for 2015 sales.

<div class='tableauPlaceholder' id='viz1495603268429' style='position: relative'><noscript><a href='#'><img alt='Segmentation and Clustering ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ta&#47;Task1Visualizations&#47;SegmentationandClustering&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='site_root' value='' /><param name='name' value='Task1Visualizations&#47;SegmentationandClustering' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ta&#47;Task1Visualizations&#47;SegmentationandClustering&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /></object></div>
used multiple analytical (predictive) techniques to provide recommendations on where and how to expand. Furthermore, since products in the fresh produce category have short life spans, leading to increasing costs, I provided accurate monthly sales forecast for all stores (new and existing) for the new year.

Tools used: Alteryx and Tabelau

