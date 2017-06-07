# Overview of Business Problem

A grocery store chain currently has 85 stores in California and is planning to open 10 new stores at the beginning of the year (2016). Currently, all stores use the same store format for selling their products. Up until now, the company has treated all stores similarly, shipping the same amount of product to each store without any forecast of how much sales to expect. This is beginning to cause problems as stores are suffering from product surpluses in some product categories and shortages in others.

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

# Allocating New Stores to Segments

The grocery store chain is opening 10 new stores at the beginning of the year (2016). To predict which store format each of the new stores should have, a model for predicting categorical variables has to be used. Since there is no sales data for these new stores yet, I had to determine the format using each of the new store’s demographic data. After comparing the Boosted, Forest and Decision Tree models, the Boosted model had the highest overall accuracy based on in-sample (cross) validation.
I built a [boosted] model using the demographic data of existing stores as the predictor (independent) variables and their cluster allocation as the dependent variable. Using this model, I passed the demographic data of the new stores to predict their cluster allocations. All new stores were predicted to be in cluster 3.


# Forecasting Produce Sales for Existing and New Stores

To minimise the perishing of goods in the produce category, I forcasted produce sales for the coming year for both existing and new stores based on past sales data. Since the new stores do not have any past sales data, I improvised, using sales data from similar stores.

For the existing stores, I aggregated the produce sales on a monthly basis from March 2012 to December 2015 to produce a dataset that meets the criteria of a time series dataset. It is continuous over the time range, it is categorized by months, and it contains a sales value for each month, measured in sequence across the time range. The most recent 12 months in the time range (2015 sales) was used as a holdout sample to internally validate the forecasting model(s) to be compared.

For the new stores (with no sales data), since all of them were predicted to fall in segment 3, I filtered out the average produce sales data for all existing stores in that segment for each month in the same time range (March 2012 to December 2015). The average produce sales value was then multiplied by the number of new stores, 10. This dataset, being a subset of the earlier described dataset, also meets the criteria for a time series dataset. I also used the monthly produce sales for the year 2015 to validate the forecasting models.

** time series and decomposition plots
** ACF and PACF plots

Based on the time series and decomposition plots, I decided to use an ETS MNM model to forecast the monthly sales for all the new and existing stores. Looking at the ACF and PACF plots, I decided to difference the time series seasonally to achieve stationarity; however, this was not achieved immediately. I still had to take the seasonal first difference before stationarity was achieved.

** seasonal diff ts plot, seasonal first difference ts plot
** both ACF and PACF plots

The resulting ACF and PACF plots suggested using an ARIMA 011 012 model to forecast the monthly sales for all new and existing stores. Comparing this model to the ETS model showed better in-sample accuracy for the ARIMA model, but better forecast accuracy for the ETS model. Furthermore, the ACF and PACF plots after running the ARIMA model showed that it did not capture all of the correlation in the time series.

** model accuracy comparisons
** ACF and PACF plots

I used the ETS MNM model to forecast the 2016 produce sales for both new and existing stores and displayed them in a [Tableau dashboard](https://public.tableau.com/views/PANDFinalProject/Task3?:embed=y&:display_count=yes).

<div class='tableauPlaceholder' id='viz1495603164584' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;PA&#47;PANDFinalProject&#47;Task3&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='site_root' value='' /><param name='name' value='PANDFinalProject&#47;Task3' /><param name='tabs' value='yes' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;PA&#47;PANDFinalProject&#47;Task3&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /></object></div>
