# Exploratory Data Analysis
A collection of exploratory data analysis notebooks/scripts done on datasets from various sources.

### 1. Dark Net Marketplace

![dark](imgs/dark_net_data.png)

This dataset was taken from Kaggle. Here are some relevant links:

- [Link to the dataset](https://www.kaggle.com/philipjames11/dark-net-marketplace-drug-data-agora-20142015)
- [Link to the exploratory analysis folder](https://github.com/adityashrm21/exploratory_data_analysis/tree/master/dark_net_marketplace)

#### Data Description

This is a data parse of marketplace data ripped from Agora (a dark/deep web) marketplace from the years 2014 to 2015. It contains drugs, weapons, books, services, and more. Duplicate listings have been removed and prices have been averaged of any duplicates. All of the data is in a csv file and has over 100,000 unique listings.

It is organized by:

- Vendor: The seller
- Category: Where in the marketplace the item falls under
- Item: The title of the listing
- Description: The description of the listing
- Price: Cost of the item (averaged across any duplicate listings between 2014 and 2015)
- Origin: Where the item is listed to have shipped from
- Destination: Where the item is listed to be shipped to (blank means no information was provided, but mostly likely worldwide. I did not enter worldwide for any blanks however as to not make assumptions)
- Rating: The rating of the seller (a rating of [0 deals] or anything else with "deals" in it means there is not concrete rating as the amount of deals is too small for a rating to be displayed)
- Remarks: Only remark options are blank, or "Average price may be skewed outliar > .5 BTC found" which is pretty self explanatory.
