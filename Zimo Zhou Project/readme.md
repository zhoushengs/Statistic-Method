
# Final Report: Project1 and Project2 of Statistical Methods for Data Science 

**Submitted to:
Prof. Cuneyt Akcora
Department of Computer Science**

**Reported by:
Zimo Zhou (zxz180016)
Lin Yu (lxy180004)**

**November 30, 2018**


# 1. Ethereum and ERC20 tokens
## 1.1 Ethereum
Ethereum is an open source public blockchain platform with intelligent contract functionality, providing decentralized virtual machines( Ethereum Virtual Machine) through its dedicated cryptocurrency Ether to handle peer-to-peer contracts.
The concept of Ethereum was first proposed by programmer Vitalik Buterin after being inspired by bitcoin between 2013 and 2014, to the effect that the next generation of cryptocurrency and decentralized application platform which was developed in 2014 through ICO crowdfunding
As of February 2018, ether is the second most valuable cryptocurrency after bitcoin.
## 1.2 ERC20 tokens
ERC20 tokens represents the tokens made with ETH and they all followed the REC20 protocol.
Token on behalf of the digital assets, have value, but not in accordance with particular specification. 
Tokens based on ERC20 can swap more easily, and be able to work on the Dapps equally. 
New standards will make token more compatible, allowing other functions, including voting marking. The operation is more like that a token holders can completely control the assets operation.
Tokens that observing the ERC20 can track anyone at any time that how many tokens does he hold.
Standardization is very beneficial, which means that these assets can be used for different platforms and projects, otherwise they can only be used for specific occasions
# 2. Hi Mutual Society (Our primary key)
Hi Mutual Society(HMC) is a blockchain project developed and operated by Asia's largest online crowdfunding platform -- Qfund. HMC is the world's first decentralized insurance platform that supports and guarantees each other
HMC aims to build a global community with open, self-driven and transparent systems that provide mutual support to all members, and has launched three products: global health mutual support, token support and flight delay mutual support.
HMC showcases the advantages of blockchain: breaking down geographic restrictions, high security and privacy, the negligible cost of lowering barriers to entry, and the use of mediation costs down to a small percentage. The usage of smart contracts ensuring fast and secure settlement cycles, resulting in faster payments
# 3. Purpose of Project
## 3.1. Question 1 Model
In question 1, we are trying to model the distribution of how many times a user buys and sells a token.
## 3.2. Question 2 Model
In question 2, we are trying to model the correlation of price data with the frequency of transaction.
## 3.3. Project 2 Model
In project 2, we are trying to use three features from the token network at day t-1 and create a multiple linear regression model to explain price return on day t.
# 4. Preprocessing Step
For our token, the given total supply by coin market is 404,100,000. And token has 10^18 sub-units. So the amount for transaction could not be bigger than 404,100,000*10^18. We filter the ones that exceed the possible maximum value by the function below.
```R
d1<-filter(rt, rt[,4]<404100000000000000000000000)
```
This row filters the 4th column of the table which is the token amount of each transaction from seller to buyer. In our token, there is no data out of scope.
# 5. Data Visualization in the Project
## 5.1 Question 1 Data Visualization
We select the data of buyer in each transaction which is the second column in the table. After that we count the times that how many transactions that each buyer participates in. Below are the codes we use. 
```R
freq1<-(table(d1[,2])) 
plot(freq1)
```
We got this figure from these codes. Ordinate is the times of transaction and the abscissa is the id of each buyer. 
![alt text](https://github.com/zhoushengs/Statistic-Method/blob/master/Zimo%20Zhou%20Project/1.png)

Then we count the times and its times’ frequency that occur in these transactions. For these processes, we use the following codes:
```R
res1<-(table(freq1))
plot(res1)
```
After plot the times and its frequency, we got this figure.
![alt text](https://github.com/zhoushengs/Statistic-Method/blob/master/Zimo%20Zhou%20Project/2.png)

## 5.2 Question 2 Data Visualization
In this question, the convert the unixtime to the date type. And we caculate the transaction times on that day. We use codes in the following.
```R
res2<-data.frame(res1) names(res2)<-c('date','transaction') 
res2$date <- as.Date(as.POSIXct(as.numeric(as.character(res2$date)), origin = '1970-01-01', tz = 'GMT')) 
res3<-table(res2[,1]) plot(res3)
```
Then we get this plot:
![alt text](https://github.com/zhoushengs/Statistic-Method/blob/master/Zimo%20Zhou%20Project/3.png)

Then we match open_price and transaction times in two tables, with condition that on the same day. We draw a plot of these two parameters in X and Y axis by following codes
```R
date_open<-select(rt1, 1, 2) 
names(date_open)<-c('date','price') 
res5<-merge(date_open, res4, by='date') 
#select res6<-select(res5,2,3) plot(res6)
``` 
![alt text](https://github.com/zhoushengs/Statistic-Method/blob/master/Zimo%20Zhou%20Project/4.png)

## 5.3 Project 2 Data Visualization
We merge two table into one result by date to find which will be the better regressor for out model. From the following codes we got the results below.
```R
d2$date <- as.Date(as.POSIXct(as.numeric(as.character(d2$date)), origin = '1970-01-01', tz = 'GMT'))
res6<-table(d2[,2])
res6<-data.frame(res6)
names(res6)<-c('date','buyernum')
res6$date<-as.numeric (as.POSIXct(as.Date(res6$date), format="%Y-%m-%d",tz = 'GMT'))
res7<-merge(res5,res6,by='date')
##res7 : date, price, volumn, transaction,buyernum 
res7
```
![alt text](https://github.com/zhoushengs/Statistic-Method/blob/master/Zimo%20Zhou%20Project/8.png)

# 6. Remove Outliers if Exist
We find that some buyers buy the smallest amount of coins requested by the market every time. Thus, we filter these buyers out. 
```R
d1<-filter(rt,rt[,4]>404100000000000000000000000*0.008)
```
After test some numbers of factors, we find that using 0.008 can filter out these buyers. 
# 7. Functions and Packages in the Project
## 7.1 Functions
Table: count the parameter’s frequency
a)	table
We use table() to count the parameter’s frequency
b)	filter
We use filter() to eliminate the outlier amounts which are bigger than the total amount of the token
c)	as.numeric
We use as.numeric() to convert factor into numeric
d)	Fitdist
We use Fitdist() to estimate the probable distribution type.
e)	as.POSIXct
We use as.POSIXct() to convert date into unix time.
f)	merge
We use merge() to join two tables.
g)	ggscatter
We use ggscatter() to estimate correlation of price data with frequency of transaction.
h)	lm
we use lm() to create a multiple regression of the y and some features.
## 7.2 Packages
a)	dplyr
We use “dplyr” package to receive dataframe object.
b)	ggplot2
We use “ggplot2” package to draw plots with ggplot
c)	fitdistrplus
We use “fitdistrplus” package to use fitdist() function.
d)	ggpubr
We use “ggpubr” package to use ggscatter() function.
# 8. Distributions that Fit the Data(For Q1)
Firstly, we try to connect each data with curved line, to find the shape of the distribution.
```R
res2$num<-as.numeric(res2$num) 
res2$times<-as.numeric(res2$times) 
ggplot(res2,aes(num,times))+geom_point(aes(color='red'))+geom_smooth(se=FALSE)
```
The figure is shown by the above codes:
![alt text](https://github.com/zhoushengs/Statistic-Method/blob/master/Zimo%20Zhou%20Project/5.png)

After observing the shape, we test some distribution for our data, and we find that the Poisson Distribution fit our data most.
```R
library(fitdistrplus) 
fit_pois<-fitdist(res2$times,"pois") 
plot(fit_pois)
``` 
![alt text](https://github.com/zhoushengs/Statistic-Method/blob/master/Zimo%20Zhou%20Project/6.png)

From the chart, we conclude that Poisson Distribution fits the data.

# 9. Data fitting and Findings(For Q2)
We have already gotten the plot having the distribution of the price and the frequency of transactions(Showed in 5.2).
After observing the plot, we decided to find the correlation using pearson correlation.
```R
ggscatter(res6, x = "transaction", y = "price", 
          add = "reg.line", 
          cor.coef = TRUE, cor.method = "pearson",
          xlab = "transaction", ylab = "price")
```
The figure is shown by the above codes:
![alt text](https://zhoushengs/Statistic-Method/blob/master/Zimo%20Zhou%20Project/7.png)

We know from the chart that **R is 0.71**, we conclude that the correlation of price data and the frequency of transactions fits Pearson correlation.
# 10. Data fitting and Findings(For project2)
We consider a lot of features which might fit the return price’s increasing and decreasing, like the transaction times in that day, the total bought/sold tokens in that day, and the highest and open price, the number of buyers in the day and the difference of the open price and the close price. At first, we believe the volume would be a feature of the regression. But after a serial trial, it shows whether the volumn or the square of volumn do not change the results at all. Finally, we find that the square root of number of buyers, the open price and the square root of transaction times run the best results, which come out R square with 0.615.And our p-value is less than 2.2e-16, which shows that our data is close to our predict function which is:
```R
Returnprice = 5.659e-05 + -1.590e-06*openprice + 8.139e-09 *times^(1/2)    +  -1.462e-09*buyernumbers^(1/2)
```
```R
numrow1<-nrow(res7)-1
numrow2<-nrow(res7)
pt0<-rt1[1:numrow1,1]
pt1<-rt1[2:numrow2,1]
returnp<-(pt0-pt1)/pt1

openprice<-res7[2:numrow2,2]
volumn<-res7[2:numrow2,2]
volumn<-as.numeric(volumn)
times<-res7[2:numrow2,4]
times<-sqrt(times)
buyernum<-res7[2:numrow2,5]
buyernum<-sqrt(buyernum)

res<-data.frame(openprice,times,buyernum,returnp)

 s<-lm(returnp~openprice+times+buyernum, data = res)
summary(s)
```
Following are the results of our codes:
![alt text](https://github.com/zhoushengs/Statistic-Method/blob/master/Zimo%20Zhou%20Project/9.png)

# 11. Conclusions and Importance of Findings
## 11.1 Conclusion
In conclusion. We think that (1) The distribution of how many times a user buys and sells a token fits Poisson Distribution. (2) The correlation of price data and the frequency of transactions fits Pearson Correlation **(R = 0.71)**. (3) The square root of number of buyers, the open price and the square root of transaction times are better regressors **(R-square = 0.615)**.
## 11.2 Importance of Findings
In Question 1, we find that the number of people who buy and sell less is huge and the number of people who buy and sell more is small. So, we can infer that whether most people think this token is worth investing or they just try it out and finally give it up.
In Question 2. Because of the Pearson Correlation, we can infer that the frequency of transactions becomes larger when the price become lower. So, we can use this laws of economics to decide in what price should we buy the token.
In Project 2, we find that the square root of number of buyers, the open price and the square root of transaction times run the best results, which come out R square with 0.615. And the p-value is small. So, we choose these three features as our regressors.

# RREFERENCES
[1] Cuneyt Gurcan Akcora, 2017. Blockchain: A Graph Primer. Online.(2017).https://arxiv.org/abs/1708.08749

[2] Mark Tranmer, 2008 Multiple Linear Regression. Online.(2008)http://hummedia.manchester.ac.uk/institutes/cmist/archive-publications/working-papers/2008/

[3]  Montgomery Runger, 2013. *Applied Statistics and Probability for Engineers 6th edition PDF eTextbook*. (2013).
