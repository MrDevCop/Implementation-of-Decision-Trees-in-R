# Implementation-of-Decision Trees-in-R


## Introduction
This project involves using R programming language on python runtime in google colab by using *rmagic*. The reason for not using R runtime is the unavailability of an authentic way to mount google drive on colab with R runtime, which poses an issue of being not able to use project specific datasets uploaded online.

```python
%load_ext rpy2.ipython
```

This will allow the python runtime of google colab to execute code written in R. Keep in mind that by using *rmagic* we will have to include *%%R* at the start of each cell.


The very first step in reproducing this code is to download the 'csv' files named 'TrainingDatas.csv', 'TD.csv', 'Target.csv' and upload them to your google drive. Once uploaded, mount your google drive with the following lines of code:

```python
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
from google.colab import auth
from oauth2client.client import GoogleCredentials
auth.authenticate_user()
gauth = GoogleAuth()
gauth.credentials = GoogleCredentials.get_application_default()
drive = GoogleDrive(gauth)
```

And then:

```python
from google.colab import drive
drive.mount('/content/drive')
```

Make a dedicated folder for this project which stores all the csv files along with the colab notebook. (In my case the drive folder is named as ML Project)

## Data Explaination
The dataset belongs to an insurance company trying to figure out the kind of customers interested in buying their new insursnce policy 'CARAVAN Insurance Policy'. The dataset is compiled by Dutch Data Mining Compnay Sentinal Machine Research. The dataset consists of 86 variables where 1-43 are socio-demographic information about the customers and 43-86 shows ownership of other insurance policies held by customers. Total observations are 9822 where 5822 (almost 60%) will be used for training and 4000 (almost 41%) will be used for testing. Machine learning algorithm, Decision Trees will be applied on the dataset to determine the kind of customers interested in buying the 'CARAVAN' insurance policy. Other techniques like Random Forests, boosted trees, Bagging Trees are no doubt more robust techniques but the objective of this project calls for decision trees as I have to build a 'customer profile' based on prior data to determine who will buy the said insurance policy. The variable names are keywords corresponding to the actual value that column holds. For exmaple the first column 'MOSTYPE' shows the customer subtype. The values of the column are numeric which encodes to the customer subtypes given at the end of this explaination. Hence you have to look at the data dictionary that I have created and listed below:

1 MOSTYPE Customer Subtype see L0

2 MAANTHUI Number of houses 1 – 10

3 MGEMOMV Avg size household 1 – 6

4 MGEMLEEF Avg age see L1

5 MOSHOOFD Customer main type see L2

6 MGODRK Roman catholic see L3

7 MGODPR Protestant ...

8 MGODOV Other religion

9 MGODGE No religion

10 MRELGE Married

11 MRELSA Living together

12 MRELOV Other relation

13 MFALLEEN Singles

14 MFGEKIND Household without children

15 MFWEKIND Household with children

16 MOPLHOOG High level education

17 MOPLMIDD Medium level education

18 MOPLLAAG Lower level education

19 MBERHOOG High status

20 MBERZELF Entrepreneur

21 MBERBOER Farmer

22 MBERMIDD Middle management

23 MBERARBG Skilled labourers

24 MBERARBO Unskilled labourers

25 MSKA Social class A

26 MSKB1 Social class B1

27 MSKB2 Social class B2

28 MSKC Social class C

29 MSKD Social class D

30 MHHUUR Rented house

31 MHKOOP Home owners

32 MAUT1 1 car

33 MAUT2 2 cars

34 MAUT0 No car

35 MZFONDS National Health Service

36 MZPART Private health insurance

37 MINKM30 Income < 30.000

38 MINK3045 Income 30-45.000

39 MINK4575 Income 45-75.000

40 MINK7512 Income 75-122.000

41 MINK123M Income >123.000

42 MINKGEM Average income

43 MKOOPKLA Purchasing power class

44 PWAPART Contribution private third party insurance see L4

45 PWABEDR Contribution third party insurance (firms) ...

46 PWALAND Contribution third party insurane (agriculture)

47 PPERSAUT Contribution car policies

48 PBESAUT Contribution delivery van policies

49 PMOTSCO Contribution motorcycle/scooter policies

50 PVRAAUT Contribution lorry policies

51 PAANHANG Contribution trailer policies

52 PTRACTOR Contribution tractor policies

53 PWERKT Contribution agricultural machines policies

54 PBROM Contribution moped policies

55 PLEVEN Contribution life insurances

56 PPERSONG Contribution private accident insurance policies

57 PGEZONG Contribution family accidents insurance policies

58 PWAOREG Contribution disability insurance policies

59 PBRAND Contribution fire policies

60 PZEILPL Contribution surfboard policies

61 PPLEZIER Contribution boat policies

62 PFIETS Contribution bicycle policies

63 PINBOED Contribution property insurance policies

64 PBYSTAND Contribution social security insurance policies

65 AWAPART Number of private third party insurance 1 - 12

66 AWABEDR Number of third party insurance (firms) ...

67 AWALAND Number of third party insurane (agriculture)

68 APERSAUT Number of car policies

69 ABESAUT Number of delivery van policies

70 AMOTSCO Number of motorcycle/scooter policies

71 AVRAAUT Number of lorry policies

72 AAANHANG Number of trailer policies

73 ATRACTOR Number of tractor policies

74 AWERKT Number of agricultural machines policies

75 ABROM Number of moped policies

76 ALEVEN Number of life insurances

77 APERSONG Number of private accident insurance policies

78 AGEZONG Number of family accidents insurance policies

79 AWAOREG Number of disability insurance policies

80 ABRAND Number of fire policies

81 AZEILPL Number of surfboard policies

82 APLEZIER Number of boat policies

83 AFIETS Number of bicycle policies

84 AINBOED Number of property insurance policies

85 ABYSTAND Number of social security insurance policies

86 CARAVAN Number of mobile home policies 0 - 1

The averaged variables having keywords L0, L1, L2, L3, L4 are listed below:

L0

1 High Income, expensive child

2 Very Important Provincials

3 High status seniors

4 Affluent senior apartments

5 Mixed seniors

6 Career and childcare

7 Dinki's (double income no kids)

8 Middle class families

9 Modern, complete families

10 Stable family

11 Family starters

12 Affluent young families

13 Young all american family

14 Junior cosmopolitan

15 Senior cosmopolitans

16 Students in apartments

17 Fresh masters in the city

18 Single youth

19 Suburban youth

20 Etnically diverse

21 Young urban have-nots

22 Mixed apartment dwellers

23 Young and rising

24 Young, low educated

25 Young seniors in the city

26 Own home elderly

27 Seniors in apartments

28 Residential elderly

29 Porchless seniors: no front yard

30 Religious elderly singles

31 Low income catholics

32 Mixed seniors

33 Lower class large families

34 Large family, employed child

35 Village families

36 Couples with teens 'Married with children'

37 Mixed small town dwellers

38 Traditional families

39 Large religous families

40 Large family farms

41 Mixed rurals

L1

1 20-30 years

2 30-40 years

3 40-50 years

4 50-60 years

5 60-70 years

6 70-80 years

L2

1 Successful hedonists

2 Driven Growers

3 Average Family

4 Career Loners

5 Living well

6 Cruising Seniors

7 Retired and Religeous

8 Family with grown ups

9 Conservative families

10 Farmers

L3

0 0%

1 1 - 10%

2 11 - 23%

3 24 - 36%

4 37 - 49%

5 50 - 62%

6 63 - 75%

7 76 - 88%

8 89 - 99%

9 100%

L4

0 f 0

1 f 1 – 49

2 f 50 – 99

3 f 100 – 199

4 f 200 – 499

5 f 500 – 999

6 f 1000 – 4999

7 f 5000 – 9999

8 f 10.000 - 19.999

9 f 20.000

Loading the datasets uploaded in the ML project folder by using the link of each file:

```R
%%R
train_data<- read.csv('/content/drive/MyDrive/ML Project/TrainingDatas.csv')
test_data<- read.csv('/content/drive/MyDrive/ML Project/TD.csv')
target<- read.csv('/content/drive/MyDrive/ML Project/Target.csv')
target<-as.vector(target$CARAVAN)
```


## Exploratory Analysis

This section includes a rudamentary data analysis with the help of pie charts and graphs. Before starting any data science project, it is imperative to fish out important details about the individual variables along with their effect on the dependent variable. All important relational visualizations are done in the colab notebook with sufficent details. Look at the 'Exploratory Analysis' section of colab to know more.


## Data Pre-procesing
The first step of data pre-processing includes checking for multi-colinearity among the data variables. It is important to remove highly collinear variables so that any unwanted bias can be removed from the output. 

```R
%%R
corr_mat<-cor(full_data)
temp<- which(abs(corr_mat) > 0.80, arr.ind=TRUE)
corr_df<-as.data.frame(temp)
col_corr_df<-corr_df$col
var<-colnames(full_data[col_corr_df])
new_corr_mat<-cbind(temp,var)
new_corr_df<-as.data.frame(new_corr_mat)
corr_filter<-new_corr_df$row == new_corr_df$col
final_corr_df<-new_corr_df[!corr_filter,]
names(final_corr_df)<-c("Variable_1_Index", "Variable_2_Index", "Variable 2")
final_corr_df
```

### Correlation Matrix

| Variable 1 | Variable_1_Index| Variable_2_Index | Variable 2 |
| ---------- | --------------- | ---------------- | ---------- |
|MOSHOOFD    |             5   |             1    | MOSTYPE.1  |
|MOSTYPE.1   |             1   |             5    | MOSHOOFD   |
|MRELOV      |            12   |            10    | MRELGE.1   |
|MRELGE.1    |            10   |            12    | MRELOV     |
|MHKOOP      |            31   |            30    | MHHUUR.1   |
|MHHUUR.1    |            30   |            31    | MHKOOP     |
|MZPART      |            36   |            35    | MZFONDS.1  |
|MZFONDS.1   |            35   |            36    | MZPART     |
|AWAPART     |            65   |            44    | PWAPART.1  |
|AWABEDR     |            66   |            45    | PWABEDR.1  |
|AWALAND     |            67   |            46    | PWALAND.1  |
|APERSAUT    |            68   |            47    | PPERSAUT.1 |
|ABESAUT     |            69   |            48    | PBESAUT.1  |
|AMOTSCO     |            70   |            49    | PMOTSCO.1  |
|AVRAAUT     |            71   |            50    | PVRAAUT.1  |
|AAANHANG    |            72   |            51    | PAANHANG.1 |
|ATRACTOR    |            73   |            52    | PTRACTOR.1 |
|AWERKT      |            74   |            53    | PWERKT.1   |
|ABROM       |            75   |            54    | PBROM.1    |
|ALEVEN      |            76   |            55    | PLEVEN.1   |
|APERSONG    |            77   |            56    | PPERSONG.1 |
|AGEZONG     |            78   |            57    | PGEZONG.1  |
|AWAOREG     |            79   |            58    | PWAOREG.1  |
|ABRAND      |            80   |            59    | PBRAND.1   |
|AZEILPL     |            81   |            60    | PZEILPL.1  |
|APLEZIER    |            82   |            61    | PPLEZIER.1 |
|AFIETS      |            83   |            62    | PFIETS.1   |
|AINBOED     |            84   |            63    | PINBOED.1  |
|ABYSTAND    |            85   |            64    | PBYSTAND.1 |
|PWAPART.1   |            44   |            65    | AWAPART    |
|PWABEDR.1   |            45   |            66    | AWABEDR    |
|PWALAND.1   |            46   |            67    | AWALAND    |
|PPERSAUT.1  |            47   |            68    | APERSAUT   |
|PBESAUT.1   |            48   |            69    | ABESAUT    |
|PMOTSCO.1   |            49   |            70    | AMOTSCO    |
|PVRAAUT.1   |            50   |            71    | AVRAAUT    |
|PAANHANG.1  |            51   |            72    | AAANHANG   |
|PTRACTOR.1  |            52   |            73    | ATRACTOR   |
|PWERKT.1    |            53   |            74    | AWERKT     |
|PBROM.1     |            54   |            75    | ABROM      |
|PLEVEN.1    |            55   |            76    | ALEVEN     |
|PPERSONG.1  |            56   |            77    | APERSONG   |
|PGEZONG.1   |            57   |            78    | AGEZONG    |
|PWAOREG.1   |            58   |            79    | AWAOREG    |
|PBRAND.1    |            59   |            80    | ABRAND     |
|PZEILPL.1   |            60   |            81    | AZEILPL    |
|PPLEZIER.1  |            61   |            82    | APLEZIER   |
|PFIETS.1    |            62   |            83    | AFIETS     |
|PINBOED.1   |            63   |            84    | AINBOED    |
|PBYSTAND.1  |            64   |            85    | ABYSTAND   |


The output of `final_corr_df` is a dataframe shwoing all persistent correlations between variables of the dataset (shown above). Each variable name is written against its index and the name of the variable it is most closely related with. For example; in first row the first variable MOSHOOFD is collinear with MOSTYPE, and so on.
Looking at the above correlation matrix, it is evident that we got two complete sets of variable to use under the column names, `Variable 1` and `Variable 2`. We can either take the left hand side variables, `Variable 1` or the right hand side variables, `Variable 2` to create a model. Hence we have to remove the redundant variables from the dataset:

```R
%%R
rm_var<-c(5,10,30,35,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85)
new_train_data <- train_data [, -rm_var]
```
Hence the number of features have been reduced to 61.

Now its time to randomize the data to exclude any danger of data bias if any:

```R
%%R
random<-sample(1:nrow(new_train_data))
new_train_data<- new_train_data[random,]
```

## Model
For the purpose of cutomer profiling, the algorithm of `decision trees` is used. The function `rpart()` of R library `rpart` is used to train the model on our dataset of 61 variables. 

```R
%%R
model<- rpart(formula=CARAVAN ~ .,data= new_train_data)
model
```


Now that the model is trained and stored in the variable `model', it is time to make some predictions. Before moving on to predict our target dataset, there is a need to *prune* our model. Pruning is necessary because it avoids data redundancy in the output decision trees hence making it more robust. If a tree is too large with many redundant nodes, it performs poorly on the new data. Furthermore, a large tree is susceptible to overfitting and hence generalizes very poorly. 

```R
%%R
print(model$cptable)

cp<-model$cptable[which.min(model$cptable[,"xerror"]),"CP"]

model_cp<-prune(tree =model,cp=cp)
```

`model_cp` is our new, pruned model. The predictions will be made by utilizing this model:

```R
pred<-predict(object = model_cp, new_test_data)

pred<-as.vector(pred)
for (i in 1: 4000){
  if (pred[i]<0.2){
   pred[i]=0
  }   
  else{
    pred[i]=1
  }
}
```
To evaluate the performance of the model there is a need to develop metrics against which it is judged. A good perforamance metric is a `confusion matrix` which is often used for classification tasks.

```R
%%R
actual<-target
predicted<-pred
cm<-confusionMatrix(actual, predicted) 
print(cm)
```

| Confusion Matrix | True | False |
| :---         |     :---:      |          ---: |
| True   | 3479     | 191    |
| False     | 283       | 47      |

As shown in above confusion matrix the model was successful in predicting the customer choice of either purchasing or not purchasing CARAVAN insurance policy by 88.1%. The misclassification error turns out to be 11%.

```R
%%R
prediction <- factor(as.factor(pred), c(0, 1), labels = c("Not purchased", "Purchased"))
target <- factor(as.factor(target), c(0, 1), labels = c("Not purchased", "Purchased"))
tb<-table(prediction)
print(tb)
tb_t<-table(target)
print(tb_t)
```

         
| Title | Predicted | Actual |
| :---         |     :---:      |          ---: |
| Purchased   | 330     | 238    |
| Not Purchased     | 3670       | 3762          |

As seen from above table, the model misclassified 92 observations of 'Not purchased' as 'Purchased'.

The rmse of the model turns out to be:

```R
rms<-rmse(target,pred)
print(rms)
```
>0.3442383

## Analysis
For in depth analysis and customer profiling see **Analysis Section** of the google colab  
