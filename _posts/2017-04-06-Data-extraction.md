---
title: " Data extraction From Google Analytics"
output: github_document
---

Google Analytitcs is widely used in the digital analytics area for analyzing, reporting. The platform itself provides a powerful reporting and analyzing dashboard for users. However, the data from google analytics need extra processing steps on other platforms in order to get more actionable insights. 
Here, I summarized the data extraction process with Google Analytics from three commonly used platfroms: **R, Tableau**

## R 
R has a powerful function regarding to the data visualization, processing, prediction and analytics. There are bunch of open source R packeges helping R work efficiently from different aspects. ["GoogleAnalyticsR" package](http://code.markedmondson.me/googleAnalyticsR/index.html) is an R library especially for working with Google Analytics data.

#### STEP1 : Get the ViewID
I assume that the [R studio set up](https://www.rstudio.com/) is ready here. 

The first step to extract Google Analytics Data is to let R know which Google Analytics view is going to use. Thus, we need to get the [viewID](https://www.youtube.com/watch?v=x1MljgyLeRM) first from Google Analytics.

A very easy way I learnt is to use [Google Analytics Query Explorer](https://ga-dev-tools.appspot.com/query-explorer/), where you can automaticlly get the ViewID. ( I chose my own website view to extract the data)

![Google analytics screenshot]({{ site.url }}/images/Rpic/P1.png "Google Analytics Query Explorer Interface")

#### STEP2 : Install "Google Analytics R" package in R studio
The "GoogleAnalyticsR" package needs to be installed to initiate so that the user can use the GoogleAnalyticsR library.
![Install the package and library]({{ site.url }}/images/Rpic/P2.png "Package Installation")

![Install the package and library]({{ site.url }}/images/Rpic/P2.2.png "Library Installation")

#### STEP3 : Assign the data extraction properties       
Then, the conditions of the extracted data need to be set through an R command, and assigned to an R variable.

![The command screenshot]({{ site.url }}/images/Rpic/P3.png "The command screenshot")  
       

 In this script,         
 【date_range】: Set the date range of the time you want to extract.               
 【metrics】: The name of the metrics you hope to extract. A easy way to do this is to select the metrics you want to extract on the Google Analytics Query Explorer "metrics" row, copy and paste the name after "ga:", which indicates the variable name.       
 【dimensions】: Set the data dimensions, like "date","region" ....     

 Run the command, if everything is right ,you can see the data when you type the view command.

![The command screenshot]({{ site.url }}/images/Rpic/P3-region.png "Extracted Data Screenshot")    

#### STEP4 : Check the data and play

Finally make the data appear in the R studio. Now it is time to do the job you want. I personally just output the extracted dataset as a CSV file

![Output Command]({{ site.url }}/images/Rpic/P6.png "Output Command")    


![Output CSVfile]({{ site.url }}/images/Rpic/P7.png "Output CSVfile")    


           

## Tableau 
Tableau is a widely used drog-click data visualizaiton platform. It helps create effective dashboards and story telling part to make sense of your data.

#### STEP1 : Connect the Google Analytics account 
The first step is to let the Tableau Desktop know which Google Analytics account will be connected with. Start Tableau and click **Connect - To a Server - Google Analytics** Then input the Google analytics account information. 

![CONNECT AND LOGIN SCREEN]({{ site.url }}/images/Rpic/P8.png "Connect") 


![LOGIN READY]({{ site.url }}/images/Rpic/P9.png "Login Ready")    

#### STEP2: Setup the conditions
After connecting the Google analytics account with Tableau, the next step is to set up the conditions for the data that needs to be extracted.

Tableau provides a convenient guidance for users to set up conditions. In this interface, you need to select **Account, Property, and Profile, Filters, dimension and Measures**        
![CONDITION SELECTION INTERFACE]({{ site.url }}/images/Rpic/P10.png "Condition Settings")    

#### STEP3: Play with the data
If all the setting in the last steps are right. It is time to use these data to conduct the analysis though **sheet** at the bottom of Tableau Interface. Here, I present a simple bar chart. For [more [Tableau data viusalizaiton function](http://onlinehelp.tableau.com/current/pro/desktop/en-us/design_and_analyze.html)

![The sheet screenshot]({{ site.url }}/images/Rpic/P11.png  "Condition Settings")



        

#### Reference Resource:           
[From 0 TO R with Google Analytics](http://analyticsdemystified.com/google-analytics/tutorial_pulling_google_analytics_data_with_r/)

[How to Extract and Export your Google Analytics Data](http://www.stateofdigital.com/exporting-google-analytics-data/)




