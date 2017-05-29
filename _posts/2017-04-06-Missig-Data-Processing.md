---
title: " Missig Data Processing with R"
output: github_document
---

The dataset used in this Project is from Kaggle competition: https://www.kaggle.com/c/house-prices-advanced-regression-techniques/kernels

#### Overview
"To fill , or not to fill?" , that is a question when we get the datasets from real world that have a lot of missing values.
Here, use the house price training set on kaggle houseprice competition. Some regually used missing value filling techniques are adopted in the following:

* Discarding examples with missing values
 1. Directly delete the feature based on the understanding of the dataset
* Convert the missing value in to a new value
 1. Use a special value
 2. Add an attribute to indicate the missing value based on the dataset situation
* Imputation methods
 1. Assing the most frequent value to missing one
 2. Adopt some frequent-used imputation mehtods


```r
library(readr)
Houseprice_train <- read_csv("~/Documents/2017spring/Udacity/Project folder/Houseprice_train.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   Id = col_integer(),
##   MSSubClass = col_integer(),
##   LotFrontage = col_integer(),
##   LotArea = col_integer(),
##   OverallQual = col_integer(),
##   OverallCond = col_integer(),
##   YearBuilt = col_integer(),
##   YearRemodAdd = col_integer(),
##   MasVnrArea = col_integer(),
##   BsmtFinSF1 = col_integer(),
##   BsmtFinSF2 = col_integer(),
##   BsmtUnfSF = col_integer(),
##   TotalBsmtSF = col_integer(),
##   `1stFlrSF` = col_integer(),
##   `2ndFlrSF` = col_integer(),
##   LowQualFinSF = col_integer(),
##   GrLivArea = col_integer(),
##   BsmtFullBath = col_integer(),
##   BsmtHalfBath = col_integer(),
##   FullBath = col_integer()
##   # ... with 18 more columns
## )
```

```
## See spec(...) for full column specifications.
```

```r
head(Houseprice_train) 
```

```
## # A tibble: 6 × 81
##      Id MSSubClass MSZoning LotFrontage LotArea Street Alley LotShape
##   <int>      <int>    <chr>       <int>   <int>  <chr> <chr>    <chr>
## 1     1         60       RL          65    8450   Pave  <NA>      Reg
## 2     2         20       RL          80    9600   Pave  <NA>      Reg
## 3     3         60       RL          68   11250   Pave  <NA>      IR1
## 4     4         70       RL          60    9550   Pave  <NA>      IR1
## 5     5         60       RL          84   14260   Pave  <NA>      IR1
## 6     6         50       RL          85   14115   Pave  <NA>      IR1
## # ... with 73 more variables: LandContour <chr>, Utilities <chr>,
## #   LotConfig <chr>, LandSlope <chr>, Neighborhood <chr>,
## #   Condition1 <chr>, Condition2 <chr>, BldgType <chr>, HouseStyle <chr>,
## #   OverallQual <int>, OverallCond <int>, YearBuilt <int>,
## #   YearRemodAdd <int>, RoofStyle <chr>, RoofMatl <chr>,
## #   Exterior1st <chr>, Exterior2nd <chr>, MasVnrType <chr>,
## #   MasVnrArea <int>, ExterQual <chr>, ExterCond <chr>, Foundation <chr>,
## #   BsmtQual <chr>, BsmtCond <chr>, BsmtExposure <chr>,
## #   BsmtFinType1 <chr>, BsmtFinSF1 <int>, BsmtFinType2 <chr>,
## #   BsmtFinSF2 <int>, BsmtUnfSF <int>, TotalBsmtSF <int>, Heating <chr>,
## #   HeatingQC <chr>, CentralAir <chr>, Electrical <chr>, `1stFlrSF` <int>,
## #   `2ndFlrSF` <int>, LowQualFinSF <int>, GrLivArea <int>,
## #   BsmtFullBath <int>, BsmtHalfBath <int>, FullBath <int>,
## #   HalfBath <int>, BedroomAbvGr <int>, KitchenAbvGr <int>,
## #   KitchenQual <chr>, TotRmsAbvGrd <int>, Functional <chr>,
## #   Fireplaces <int>, FireplaceQu <chr>, GarageType <chr>,
## #   GarageYrBlt <int>, GarageFinish <chr>, GarageCars <int>,
## #   GarageArea <int>, GarageQual <chr>, GarageCond <chr>,
## #   PavedDrive <chr>, WoodDeckSF <int>, OpenPorchSF <int>,
## #   EnclosedPorch <int>, `3SsnPorch` <int>, ScreenPorch <int>,
## #   PoolArea <int>, PoolQC <chr>, Fence <chr>, MiscFeature <chr>,
## #   MiscVal <int>, MoSold <int>, YrSold <int>, SaleType <chr>,
## #   SaleCondition <chr>, SalePrice <int>
```

```r
dim(Houseprice_train)
```

```
## [1] 1460   81
```

```r
sapply(Houseprice_train, function(x) sum(is.na(x)))
```

```
##            Id    MSSubClass      MSZoning   LotFrontage       LotArea 
##             0             0             0           259             0 
##        Street         Alley      LotShape   LandContour     Utilities 
##             0          1369             0             0             0 
##     LotConfig     LandSlope  Neighborhood    Condition1    Condition2 
##             0             0             0             0             0 
##      BldgType    HouseStyle   OverallQual   OverallCond     YearBuilt 
##             0             0             0             0             0 
##  YearRemodAdd     RoofStyle      RoofMatl   Exterior1st   Exterior2nd 
##             0             0             0             0             0 
##    MasVnrType    MasVnrArea     ExterQual     ExterCond    Foundation 
##             8             8             0             0             0 
##      BsmtQual      BsmtCond  BsmtExposure  BsmtFinType1    BsmtFinSF1 
##            37            37            38            37             0 
##  BsmtFinType2    BsmtFinSF2     BsmtUnfSF   TotalBsmtSF       Heating 
##            38             0             0             0             0 
##     HeatingQC    CentralAir    Electrical      1stFlrSF      2ndFlrSF 
##             0             0             1             0             0 
##  LowQualFinSF     GrLivArea  BsmtFullBath  BsmtHalfBath      FullBath 
##             0             0             0             0             0 
##      HalfBath  BedroomAbvGr  KitchenAbvGr   KitchenQual  TotRmsAbvGrd 
##             0             0             0             0             0 
##    Functional    Fireplaces   FireplaceQu    GarageType   GarageYrBlt 
##             0             0           690            81            81 
##  GarageFinish    GarageCars    GarageArea    GarageQual    GarageCond 
##            81             0             0            81            81 
##    PavedDrive    WoodDeckSF   OpenPorchSF EnclosedPorch     3SsnPorch 
##             0             0             0             0             0 
##   ScreenPorch      PoolArea        PoolQC         Fence   MiscFeature 
##             0             0          1453          1179          1406 
##       MiscVal        MoSold        YrSold      SaleType SaleCondition 
##             0             0             0             0             0 
##     SalePrice 
##             0
```

```r
# visualize the missing data
image(is.na(Houseprice_train), main = "Missing Values", xlab = "Observation", ylab = "Variable", 
    xaxt = "n", yaxt = "n", bty = "n")
axis(1, seq(0, 1, length.out = nrow(Houseprice_train)), 1:nrow(Houseprice_train), col = "white")
```

![plot of chunk Missing data cleaning](/figure/posts/2017-04-06-Missig-Data-Processing/Missing data cleaning-1.png)
From this visualization, we can generally see a big picture of the missing value.
There are several columns that have the the missing value in the same time. There should be some patterns between these missing data.

```r
# Check columns with missing data
colSums((is.na(Houseprice_train)) == TRUE)
```

```
##            Id    MSSubClass      MSZoning   LotFrontage       LotArea 
##             0             0             0           259             0 
##        Street         Alley      LotShape   LandContour     Utilities 
##             0          1369             0             0             0 
##     LotConfig     LandSlope  Neighborhood    Condition1    Condition2 
##             0             0             0             0             0 
##      BldgType    HouseStyle   OverallQual   OverallCond     YearBuilt 
##             0             0             0             0             0 
##  YearRemodAdd     RoofStyle      RoofMatl   Exterior1st   Exterior2nd 
##             0             0             0             0             0 
##    MasVnrType    MasVnrArea     ExterQual     ExterCond    Foundation 
##             8             8             0             0             0 
##      BsmtQual      BsmtCond  BsmtExposure  BsmtFinType1    BsmtFinSF1 
##            37            37            38            37             0 
##  BsmtFinType2    BsmtFinSF2     BsmtUnfSF   TotalBsmtSF       Heating 
##            38             0             0             0             0 
##     HeatingQC    CentralAir    Electrical      1stFlrSF      2ndFlrSF 
##             0             0             1             0             0 
##  LowQualFinSF     GrLivArea  BsmtFullBath  BsmtHalfBath      FullBath 
##             0             0             0             0             0 
##      HalfBath  BedroomAbvGr  KitchenAbvGr   KitchenQual  TotRmsAbvGrd 
##             0             0             0             0             0 
##    Functional    Fireplaces   FireplaceQu    GarageType   GarageYrBlt 
##             0             0           690            81            81 
##  GarageFinish    GarageCars    GarageArea    GarageQual    GarageCond 
##            81             0             0            81            81 
##    PavedDrive    WoodDeckSF   OpenPorchSF EnclosedPorch     3SsnPorch 
##             0             0             0             0             0 
##   ScreenPorch      PoolArea        PoolQC         Fence   MiscFeature 
##             0             0          1453          1179          1406 
##       MiscVal        MoSold        YrSold      SaleType SaleCondition 
##             0             0             0             0             0 
##     SalePrice 
##             0
```

```r
colnames(Houseprice_train)[colSums(is.na(Houseprice_train))>0]
```

```
##  [1] "LotFrontage"  "Alley"        "MasVnrType"   "MasVnrArea"  
##  [5] "BsmtQual"     "BsmtCond"     "BsmtExposure" "BsmtFinType1"
##  [9] "BsmtFinType2" "Electrical"   "FireplaceQu"  "GarageType"  
## [13] "GarageYrBlt"  "GarageFinish" "GarageQual"   "GarageCond"  
## [17] "PoolQC"       "Fence"        "MiscFeature"
```
According to the summary , we check columns with "NA" one by one:
"LotFrontage": Linear feet of street connected to property.
"Alley":Type of alley access
"MasVnrType":Masonry veneer type  
"MasVnrArea" : Masonry veneer area in square feet
"BsmtQual": Height of the basement
"BsmtCond" :General condition of the basement   
"BsmtExposure": Walkout or garden level basement walls
"BsmtFinType1":Quality of basement finished area
"BsmtFinType2":Quality of second finished area (if present)
"Electrical":Electrical system  
"FireplaceQu": Fireplace quality
"GarageType":  Garage location
"GarageYrBlt": Year garage was built
"GarageFinish": Interior finish of the garage
"GarageQual" :Garage quality
"GarageCond":Garage condition
"PoolQC":  Pool quality    
"Fence" :  Fence quality  
"MiscFeature": Miscellaneous feature not covered in other categories
#####LotFrontage:Linear feet of street connected to property.
 The LotFrontage should have some correlations with LotArea. There is no null value in LotArea, it is reasonable to check the correlation between the Lot Frontage and Lot Area.

```r
library(ggplot2)

##Correlation Check
cor.test(Houseprice_train$LotFrontage,Houseprice_train$LotArea)
```

```
## 
## 	Pearson's product-moment correlation
## 
## data:  Houseprice_train$LotFrontage and Houseprice_train$LotArea
## t = 16.309, df = 1199, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.3786555 0.4713015
## sample estimates:
##      cor 
## 0.426095
```

```r
# 0.426095
# Check with square root of the LotArea
LotArea_square = sqrt(Houseprice_train$LotArea)
cor.test(Houseprice_train$LotFrontage,LotArea_square)# 0.6020022
```

```
## 
## 	Pearson's product-moment correlation
## 
## data:  Houseprice_train$LotFrontage and LotArea_square
## t = 26.106, df = 1199, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.5646646 0.6368806
## sample estimates:
##       cor 
## 0.6020022
```

```r
df1<-data.frame(Houseprice_train,LotArea_square)

## Imputation
names = c("LotFrontage","LotArea_square")
df1_predict<-df1[!is.na(df1$LotFrontage),names]
df1_result<-df1[is.na(df1$LotFrontage),names]

#Predict LotFrontage
lm1<-lm(formula = LotFrontage ~ LotArea_square, data = df1_predict,na.action = na.omit)
predict1 <-predict(lm1, df1_result)

#Compare the pattern between imputed data and existing data
df.frontage <- as.data.frame(rbind(cbind(rep("Existing",nrow(df1[!is.na(df1$LotFrontage),])),df1[!is.na(df1$LotFrontage), "LotFrontage"]),cbind(rep("Imputed", nrow(df1[is.na(df1$LotFrontage),])),
                           ceiling(predict1))))

ggplot(df.frontage, aes (x = as.numeric(as.character(V2)), colour = V1)) +
    geom_density()+
    xlab("Lot Frontage")+
  theme(legend.title=element_blank())
```

![plot of chunk LotFrontage/LotArea correlation](/figure/posts/2017-04-06-Missig-Data-Processing/LotFrontage/LotArea correlation-1.png)

```r
# Accroding to the pattern graph the imputed value are resonable
Houseprice_train$LotFrontage[is.na(Houseprice_train$LotFrontage)] <- round(predict1)
#Check
sum(is.na(Houseprice_train$LotFrontage)) # 0 
```

```
## [1] 0
```

#####Alley:Type of alley access

```r
## Check the levels
sum(is.na(Houseprice_train$Alley))
```

```
## [1] 1369
```

```r
table(Houseprice_train$Alley)
```

```
## 
## Grvl Pave 
##   50   41
```

```r
#Assume Empty area means no Alley access
Houseprice_train$Alley[is.na(Houseprice_train$Alley)]<-"None"
sum(is.na(Houseprice_train$Alley)) # 0 
```

```
## [1] 0
```

```r
table(Houseprice_train$Alley)
```

```
## 
## Grvl None Pave 
##   50 1369   41
```

##### MasVnrType&MasVnrArea:Masonry veneer type& Masonry veneer area in square feet

```r
df2<- Houseprice_train
names2<-c("MasVnrType","MasVnrArea")
df2_missingcheck<- df2[,names2]

## Plot the missing data in two columns

image(is.na(df2_missingcheck), main = "Missing Location MasVnrType&MasVnrArea", xlab = "Observation", xaxt = "n", yaxt = "n" , bty = "n")
axis(1, seq(0, 1, length.out = nrow(Houseprice_train)), 1:nrow(df2_missingcheck), col = "white")
```

![plot of chunk MasvnrTyle](/figure/posts/2017-04-06-Missig-Data-Processing/MasvnrTyle-1.png)

```r
# The missing values in MasVnrType and MasVnrArea locate at same places. Check the value situation in MasVnrType column
summary(as.factor(df2_missingcheck$MasVnrType))
```

```
##  BrkCmn BrkFace    None   Stone    NA's 
##      15     445     864     128       8
```

```r
# Impute the NA area as "None", which has the highest quantity here. 
Houseprice_train$MasVnrType[is.na(Houseprice_train$MasVnrArea)]<- "None"
Houseprice_train$MasVnrArea[is.na(Houseprice_train$MasVnrArea)]<- 0 
```
#####Basement:BsmtQual & BsmtCond & BsmtExposure & BsmtFinType1 & BsmtFinType2

```r
library(rpart)
df3<-Houseprice_train

##Missing value check
BsmtExposure<-table(is.na(df3$BsmtExposure))
BsmtCond<-table(is.na(df3$BsmtCond))
BsmtQual<-table(is.na(df3$BsmtQual))
BsmtFinType1<-table(is.na(df3$BsmtFinType1))
BsmtFinType2<-table(is.na(df3$BsmtFinType2))
missing_table<-cbind(BsmtQual,BsmtCond,BsmtExposure,BsmtFinType1,BsmtFinType2)
missing_table
```

```
##       BsmtQual BsmtCond BsmtExposure BsmtFinType1 BsmtFinType2
## FALSE     1423     1423         1422         1423         1422
## TRUE        37       37           38           37           38
```

```r
## Check other basement related data
data.frame(df3$BsmtFinSF1,df3$BsmtFinSF2,df3$BsmtUnfSF,df3$TotalBsmtSF)[1:10,]
```

```
##    df3.BsmtFinSF1 df3.BsmtFinSF2 df3.BsmtUnfSF df3.TotalBsmtSF
## 1             706              0           150             856
## 2             978              0           284            1262
## 3             486              0           434             920
## 4             216              0           540             756
## 5             655              0           490            1145
## 6             732              0            64             796
## 7            1369              0           317            1686
## 8             859             32           216            1107
## 9               0              0           952             952
## 10            851              0           140             991
```

```r
#Based on commansense, suppose the basement info with missing data can be predicted by the rest of basement related data
names3<-c("BsmtQual","BsmtCond","BsmtExposure","BsmtFinType1","BsmtFinType2","BsmtFinSF1","BsmtFinSF2","BsmtUnfSF","TotalBsmtSF")

#Impute the missing part of data
rpart1<- rpart(as.factor(BsmtExposure) ~., data = df3[!is.na(df3$BsmtExposure),names3],na.action= na.omit,method = "class")
rpart2<- rpart(as.factor(BsmtCond) ~., data = df3[!is.na(df3$BsmtCond),names3],na.action= na.omit,method = "class")

rpart3<- rpart(as.character(BsmtQual) ~., data = df3[!is.na(df3$BsmtQual),names3],na.action= na.omit,method = "class")
rpart4<- rpart(as.character(BsmtFinType1) ~., data = df3[!is.na(df3$BsmtFinType1),names3],na.action= na.omit,method = "class")
rpart5<- rpart(as.character(BsmtFinType2) ~., data = df3[!is.na(df3$BsmtFinType2),names3],na.action= na.omit,method = "class")

df3$BsmtExposure[is.na(df3$BsmtExposure)]<-as.character(predict(rpart1,df3[is.na(df3$BsmtExposure),names3],type = "class"))
df3$BsmtCond[is.na(df3$BsmtCond)]<-as.character(predict(rpart2,df3[is.na(df3$BsmtCond),names3],type = "class"))
df3$BsmtQual[is.na(df3$BsmtQual)]<-as.character(predict(rpart3,df3[is.na(df3$BsmtQual),names3],type = "class"))
df3$BsmtFinType1[is.na(df3$BsmtFinType1)]<-as.character(predict(rpart4,df3[is.na(df3$BsmtFinType1),names3],type = "class"))
df3$BsmtFinType2[is.na(df3$BsmtFinType2)]<-as.character(predict(rpart5,df3[is.na(df3$BsmtFinType2),names3],type = "class"))

Houseprice_train$BsmtExposure<-df3$BsmtExposure
Houseprice_train$BsmtCond<-df3$BsmtCond
Houseprice_train$BsmtQual<-df3$BsmtQual
Houseprice_train$BsmtFinType1<-df3$BsmtFinType1
Houseprice_train$BsmtFinType2<-df3$BsmtFinType2

# Check the missing data in the original dataset
BsmtExposure<-table(is.na(Houseprice_train$BsmtExposure))
BsmtCond<-table(is.na(Houseprice_train$BsmtCond))
BsmtQual<-table(is.na(Houseprice_train$BsmtQual))
BsmtFinType1<-table(is.na(Houseprice_train$BsmtFinType1))
BsmtFinType2<-table(is.na(Houseprice_train$BsmtFinType2))
missing_table<-cbind(BsmtQual,BsmtCond,BsmtExposure,BsmtFinType1,BsmtFinType2)
missing_table
```

```
##       BsmtQual BsmtCond BsmtExposure BsmtFinType1 BsmtFinType2
## FALSE     1460     1460         1460         1460         1460
```

#####Electrical:Electrical system  

```r
df4<-Houseprice_train
sum(is.na(df4$Electrical)) # 1
```

```
## [1] 1
```

```r
table(df4$Electrical)
```

```
## 
## FuseA FuseF FuseP   Mix SBrkr 
##    94    27     3     1  1334
```

```r
## Impute : impute the value as the most frequent value "SBrkr"
Houseprice_train$Electrical[is.na(Houseprice_train$Electrical)]<-"SBrkr"

##Check the column in the original dataset
table(is.na(Houseprice_train$Electrical))
```

```
## 
## FALSE 
##  1460
```
#####FireplaceQu: Fireplace quality

```r
df5<-Houseprice_train
sum(is.na(df5$FireplaceQu)) #690
```

```
## [1] 690
```

```r
## Check related Fireplace column: "FirePlaces"
table(df5$Fireplaces)
```

```
## 
##   0   1   2   3 
## 690 650 115   5
```

```r
# There is also 690 rows with value 0 in "FirePlaces", are they the same?
FP_id<- df5$Id[df5$Fireplaces == 0]
FPQ_id<- df5$Id[df5$Fireplaces == 0]
table(FP_id == FPQ_id)
```

```
## 
## TRUE 
##  690
```

```r
# Thus, the NA in the "FireplaceQu" means no FirePlaces.Impute the missing value as "None"
Houseprice_train$FireplaceQu[is.na(Houseprice_train$FireplaceQu)]<- 'None'
sum(is.na(Houseprice_train$FireplaceQu))
```

```
## [1] 0
```

#####Garage

```r
##Check garage related columns
df6<-Houseprice_train


#GarageArea <-table(is.na(df6$GarageArea))
#GarageCars <-table(is.na(df6$GarageCars))
GarageCond<-table(is.na(df6$GarageCond))
GarageFinish<-table(is.na(df6$GarageFinish))
GarageQual<-table(is.na(df6$GarageQual))
GarageType<-table(is.na(df6$GarageType))
GarageYrBlt<-table(is.na(df6$GarageYrBlt))
#GrLivArea<-table(is.na(df6$GrLivArea))
missing_table2<- cbind(GarageCond,GarageFinish,GarageQual,GarageType,GarageYrBlt)
missing_table2
```

```
##       GarageCond GarageFinish GarageQual GarageType GarageYrBlt
## FALSE       1379         1379       1379       1379        1379
## TRUE          81           81         81         81          81
```

```r
df6_missingcheck<- df6[ ,c("GarageCond","GarageFinish","GarageQual","GarageType","GarageYrBlt")]

# Checking missing value location in the dataset
image(is.na(df6_missingcheck), main = "Missing Location - Garage ", xlab = "Observation",ylab = "Variables", xaxt = "n", yaxt = "n" , bty = "n")
axis(1, seq(0, 1, length.out = nrow(df6)), 1:nrow(df6_missingcheck), col = "white")
```

![plot of chunk Garage](/figure/posts/2017-04-06-Missig-Data-Processing/Garage-1.png)

```r
# For the missing areas in columns, the GarageAreas are not zero. Adopt garage-related features as predictors to impute missing values
names4<-c("GarageCond","GarageFinish","GarageQual","GarageType","GarageYrBlt","GarageArea","GarageCars","GrLivArea")


df6[df6$GarageArea == 0 & df6$GarageCars==0 & is.na(df6$GarageType), names4] <- apply(df6[df6$GarageArea == 0 & df6$GarageCars==0 & is.na(df6$GarageType), names4], 2, function(x) x <- rep("None", 81))


# GarageType
#Type.rpart<-rpart(GarageType ~ ., data =df6[!is.na(df6$GarageType),names4],method = "anova", #na.action = na.omit)
#df6$GarageType[is.na(df6$GarageType)]<- rep("None", 81)

#Qual.rpart<-rpart(GarageQual ~ ., data = df6[!is.na(df6$GarageQual),names4],na.action = na.omit)

#df6$GarageQual[is.na(df6$GarageQual)]<- as.character(predict(Qual.rpart, #df6[is.na(df6$GarageQual),names4]))


#GarageFinish
#Finish.rpart<-rpart(GarageFinish ~ ., data = df6[!is.na(df6$GarageFinish),names4],na.action = #na.omit)
#df6$GarageFinish[is.na(df6$GarageFinish)]<- as.character(predict(Finish.rpart, #df6[is.na(df6$GarageFinish),names4]))
#GarageCond
#Cond.rpart<-rpart(GarageCond ~ ., data = df6[!is.na(df6$GarageCond),names4],na.action = na.omit)
#df6$GarageCond[is.na(df6$GarageCond)]<- as.character(predict(Cond.rpart, #df6[is.na(df6$GarageCond),names4])) 
#GarageYrBlt
#Year.rpart<- rpart(GarageYrBlt ~ ., data = df6[!is.na(df6$GarageYrBlt),names4],na.action = na.omit)
#df6$GarageYrBlt[is.na(df6$GarageYrBlt)]<- as.character(predict(Year.rpart, #df6[is.na(df6$GarageYrBlt),names4])) 

#Replace in the original dataset
Houseprice_train$GarageType<-df6$GarageType
Houseprice_train$GarageFinish <-df6$GarageFinish
Houseprice_train$GarageCond <-df6$GarageCond
Houseprice_train$GarageYrBlt <-df6$GarageYrBlt
Houseprice_train$GarageQual<-df6$GarageQual


table(is.na(Houseprice_train$GarageType))
```

```
## 
## FALSE 
##  1460
```

```r
table(is.na(Houseprice_train$GarageFinish))
```

```
## 
## FALSE 
##  1460
```

```r
table(is.na(Houseprice_train$GarageCond))
```

```
## 
## FALSE 
##  1460
```

```r
table(is.na(Houseprice_train$GarageYrBlt))
```

```
## 
## FALSE 
##  1460
```

```r
table(is.na(Houseprice_train$GarageQual))
```

```
## 
## FALSE 
##  1460
```
#####PoolQC

```r
df7<-Houseprice_train
sum(is.na(df7$PoolQC))
```

```
## [1] 1453
```

```r
table(df7$PoolQC)
```

```
## 
## Ex Fa Gd 
##  2  2  3
```

```r
#Check poolArea feature
table(df7$PoolArea >0, df7$PoolQC,useNA = "ifany")
```

```
##        
##           Ex   Fa   Gd <NA>
##   FALSE    0    0    0 1453
##   TRUE     2    2    3    0
```

```r
#According to the table, the houses with PoolQC empty also has no swimming pool available. Thus, NA value in "PoolQC" areas can be imputed as non
df7$PoolQC[is.na(df7$PoolQC)]<-as.character("None")
sum(is.na(df7$PoolQC))
```

```
## [1] 0
```

```r
Houseprice_train$PoolQC<-df7$PoolQC
```
#####Fence

```r
table(df7$Fence)
```

```
## 
## GdPrv  GdWo MnPrv  MnWw 
##    59    54   157    11
```

```r
table(is.na(df7$Fence))
```

```
## 
## FALSE  TRUE 
##   281  1179
```

```r
#Replace NA as None

Houseprice_train$Fence<-as.character("None")
```

#####MiscFeature

```r
table(df7$MiscFeature)
```

```
## 
## Gar2 Othr Shed TenC 
##    2    2   49    1
```

```r
table(is.na(df7$MiscFeature))
```

```
## 
## FALSE  TRUE 
##    54  1406
```

```r
##Replace NA as None
Houseprice_train$MiscFeature[is.na(Houseprice_train$MiscFeature)]<-as.character("None")
sum(is.na(Houseprice_train$MiscFeature))
```

```
## [1] 0
```

#####Final Check

```r
sum(is.na(Houseprice_train))
```

```
## [1] 0
```


