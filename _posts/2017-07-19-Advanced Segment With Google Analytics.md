---
title: " Advanced Segments With Google Analytics"
output: github_document
---
The advanced segments feature in Google Analytics allows users to slice and dice the historical and real-time data immediately without altering the data permanently. Here, I used the demo account ["Google Analytics - Google Merchandise Store"](https://analytics.google.com/analytics/web/?utm_source=demoaccount&utm_medium=demoaccount&utm_campaign=demoaccount#report/visitors-overview/a54516992w87479473p92320289/)



### Where is Advanced Segments Function?
Let's take the Behavior - Overview report as an example. The advanced segment function is usually under the report title.     
![p1-AdvancedSegmentsClick]({{ site.url }}/images/AdSeg/P1.png "AdvancedSegmentsClick")        

The default interface provides the choices of  built-in segments,custom segments and shared segments.
![p2-AdvancedSegmentsInterface]({{ site.url }}/images/AdSeg/P2.png "AdvancedSegmentsInterface")  

To select the segments, you just need to check the cube in front of each segment. Then click the **"Apply"** at the bottom of the interface. Google analytics allows **at most 4 segments applied to the report at any one time**.

### How to Set Up Custom Segments Function?
When the built-in segments can not satisfy the analysis requirement, a custom segment can help! To create a new custom segment, click the red **"+ New Custom Segment"** button. Then you can see another interface which can guide you to set up your own custom segments.   
![p3-CustomSegment]({{ site.url }}/images/AdSeg/P3.png "AdvancedSegmentsInterface") 
Google Analytics provides several sections for the settings.       
**Demographics** : Segment users by demographic information such as gender, language, ages and location.         
**Technology** : Segment users' visits by oprating system, browser, screen resolution, device and mobile.             
**Behavior** : Segment users by the frequency of visit and transactions         
**Date of First Visit** : Segment users by the date they first visited the page         
**Traffic Source** :  Segment users by the methods they used to find the page such as campaign, medium, source and kewordfileds.             

The advanced segment function also allows users to setup custom segment similar to how it was done before with the "**Conditions**" section and "**Sequences**" section allow you to segment users and visits according to sequential conditions.

![p4-ConditionsSegment]({{ site.url }}/images/AdSeg/P4.png "ConditionsSegment")            
The summary part (at the right of the interface) will show the immediate result for review while user is setting the condition at conditions part.
                
Once you save the custom segment, you can see the segment appearing under the report title, where you can edit, remove this segment.

![p5-CustomSegmentSave]({{ site.url }}/images/AdSeg/P5.png "ConditionsSegmentSave")
          
You can also see it on the default advance segment interface when you add another segment next time.

![p6-SegmentOnPanel]({{ site.url }}/images/AdSeg/P6.png "SegmentOnPanel")




### How to Import Segments From Gallery?
Besides the custom segment, user can also import from Gallery to get more segements created by other Google Analytics users. Click the the **Import from gallery** button next to **"+NEW SEGMENT"**, the gallery interface jumped out.   

![p7- GalleryInterface]({{ site.url }}/images/AdSeg/P7.png "GalleryInterface")    
Select the segment bundle that can satisfy your work.Click **Import**.    


![p8-AfterImportSegment]({{ site.url }}/images/AdSeg/P8.png "CreateImportedSegment")   

In this window you can see the Segments, Custom Report or Dashboards created by the author. Check intems that you need, then click **Create**.

![p9-DefaultSegmentPanel]({{ site.url }}/images/AdSeg/P9.png "DefaultSegmentPanel") 



When you click the **+Add segment** next time, you can see the imported segments appearing on the default interface and apply them.



#### Reference Resource      
[About segment](https://support.google.com/analytics/answer/3123951?hl=en)        
[Google Analytics: How to use advanced segments](http://www.practicalecommerce.com/Google-Analytics-How-to-Use-Advanced-Segments)          
[How to use the New Google Analytics Advanced Segments](https://blog.kissmetrics.com/new-google-analytics-advanced-segments/)