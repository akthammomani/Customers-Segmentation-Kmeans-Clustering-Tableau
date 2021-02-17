# Project: Optimizing Wholesale Wine business by segmenting customers behavior using Machine Learning - Kmeans powered by Tableau

<p align="center">
  <img width="1000" height="400" src="https://user-images.githubusercontent.com/67468718/108179916-03ce9e80-70bb-11eb-937d-702f5c5151d9.JPG">
</p>

## 1. Introduction

This project is based on a book called [Data Smart](https://www.amazon.com/Data-Smart-Science-Transform-Information/dp/111866146X), written by John Foreman, head of product for MailChimp. This book is an excellent business analytics primer that walks you through a variety of machine learning use cases, complete with sample data sets and detailed instructions on how to set up and solve each case in Excel. 

One of these cases, John describes a fictitious wholesale wine business looking to optimize sales by segmenting customers — restaurant and liquor store owners, into groups based on buying behavior. This is a very common problem faced by all kinds of businesses from large retailers like Walmart and Target to your friendly neighborhood liquor store.

## 2. Dataset

The dataset contains information on marketing newsletters/e-mail campaigns (e-mail offers sent to customers) and transaction level data from customers. The transactional data shows which offer customers responded to, and what the customer ended up buying. The data is presented as an Excel workbook containing two worksheets. Each worksheet contains a different dataset.

We see that the first dataset <code>**WineKMC.xlsx sheet 0**</code>  contains information about each offer such as the month it is in effect and several attributes about the wine that the offer refers to: the variety, minimum quantity, discount, country of origin and whether or not it is past peak. 

The second dataset <code>**WineKMC.xlsx sheet 1**</code> in the second worksheet contains transactional data -- which offer each customer responded to.

## 3. Data Wrangling

We're trying to learn more about how our customers behave, so we can use their behavior (whether or not they purchased something based on an offer) as a way to group similar minded customers together. We can then study those groups to look for patterns and trends which can help us formulate future offers.

The first thing we need is a way to compare customers. To do this, we're going to create a matrix that contains each customer and a 0/1 indicator for whether or not they responded to a given offer.

  * Merge <code>**df_transactions**</code> & <code>**df_offers**</code> on=<code>**offer_id**</code> 
  * Make a pivot table, to show a matrix with customer names in the row index and offer numbers as columns headers and fill NANs with zeros.






