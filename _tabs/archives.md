---
layout: archives
icon: fas fa-archive
order: 3
---

## [Body Mass Index BMI fitting Distribution with R](https://github.com/MauriLoi/Body-Mass-Index-BMI-fitting-Distribution-with-R)

<div align="justify"> The following text is about the Fourth Dutch Growth Study by Fredriks et al. (2000a, 2000b), which is a cross-sectional study that measures the growth and development of the Dutch population from birth to 21 years of age. The study collected data on height, weight, head circumference, and age for 7482 males and 7018 females. In this analysis, we are using only the BMI (Body Mass Index) of Dutch boys.  </div>   <br/>

1. Body Mass Index (BMI) Data set

   1.1  Introductionto to the Body Mass Index (BMI) data set 
    
   1.2  Selection of all the possible Distributions

   1.3  Distribution choice
    
   1.4  Output the Parameters
   
### 1.1 Introduction to the Body Mass Index (BMI) data set  

<div align="justify"> The aim of this section of the report is to analyze the data set previous given of the Body Mass Index(BMI) from the Fourth Dutch Growth Study, Fredrisk et al.(2000a) and find a suitable probability distribution to fit at the data. The dataset reports 7482 observations and has as explanatory variable the age. The age range from 0.03 (3 days) to 21.70 (21 years and 7 months).To begin, we will create a subset of observations for boys aged 14 to 15. 
Fitting a distribution is the process of finding a mathematical function that represent at the best a statistical variable in our case the BMI. In practice given the unknown distribution density (pdf) derivate from our observation sample that is an univariate continuous distribution with domain[0,+∞ ] we need to select an appropriate distribution that is able to approximate the behavior of the empirical data. We will follow a two-step approach: fitting and diagnostic. I will fit different distribution and compare them using the generalized Akaike information criterion (GAIC) given the nature of not nested gamlss models. After I have compered the different result I will choose the one with the smallest GAIC(k) after the selection of the value k. Reminding the fact that GAIK(k)=GD + (k *df), where df is the effective degree of freedom used in the model and GD is the fitted Global deviance. The first approach is to explore the data with histograms and short descriptive stats, the frequency histogram with 60 bins of the values, the empirical density function and the empirical cumulative frequency distributions are reported below (see figure 1,3 and 4 for the BMI data set ). Descriptive statistic of the distribution as the mean, standard deviation, skewness, kurtosis this could be done using the descdist(). A skewness-kurtosis plot proposed by Cullen and Frey (1999) provided by the descdist() function is shown above (see Figure 2 for the BMI data set).  </div>  <br/>
&emsp;&emsp;&emsp;&emsp;![Histogram](Images/Picture1.png)   

&emsp;&emsp;&emsp;Figure 1 Histogram of the frequencies distibution of the variable BMI

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;![Cullen and Frey graph](/Images/Picture2.png)  

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Figure 2 Cullen and Frey graph  

![](/Images/Picture3.png)  

&emsp;&emsp;&emsp; Figure 3 Shows the pdf() of the BM    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Figure 4 Shows the cdf() of the BMI  

<div align="justify"> The skewness and kurtosis are useful tools for identifying the best fit for a distribution. When the skewness value is zero, it indicates that the distribution is normal and symmetric around its mean. However, if the skewness value is positive or negative, it suggests a non-symmetric distribution.
Similarly, the kurtosis value measures the weight of tails in comparison to the normal distribution (where kurtosis equals 3). The Cullen and Frey plot represents various distributions as a single point or areas of possible values. For instance, gamma and lognormal distributions are represented by lines, while beta distributions are represented by larger areas. It's important to bear in mind that the Cullen and Frey plot is only an approximation, and the non-robust nature of the skewness and kurtosis values makes them uncertain. Even when using the bootstrap method (random sampling with replacement) to address this issue, the results should be interpreted with caution. No consistent information has been given from the Cullen and Frey plot the data doesn’t follow any well know distribution. Analyzing the graphs of the distribution we see that it is slightly skewed on the right(positive skewness ) and leptokurtic (positive kurtosis). Knowing this we can start to formulate some hypothesis. In our case the distribution has mean 𝜇 = 19.748870, standard deviation 𝜎 = 2.268981, skewness 𝜗 = 1.510098 𝑎𝑛𝑑 kurtosis 𝜏 = 9.110169. Although we know that the normal distribution is unlikely to be the best fit, starting with it can give us a general understanding of the distribution's behavior.  </div> <br/>

### 1.2 Selection of all the possible Distributions  

<div align="justify"> Improve it
To proceed, we need to identify the possible distributions that can be fitted and tested. Due to the nature of our data, we can limit the choices to some functions that are capable of modeling skewness and kurtosis. In the Box-Cox Cole-Green family, we have a few appropriate options, including the gamma (GA) distribution, which is suitable for positively skewed data, the (IG) inverse Gaussian distribution, which is appropriate for highly positively skewed data, the SEP1(Skew power exp, t1, parametrization of PE, and TF(T family distribution) the t family distribution, which is symmetric but can model leptokurtosis, and the (PE) power exponential distribution, which is suitable for leptokurtic and platykurtic data.
Using the histDist() funtion that fits constants to the parameters of a GAMLSS family distribution and them plot the histogram and the fitted distribution[1]. I will compare all the fitted distributions graphically and with the gamlss() function that returns an object of class "gamlss" I will create a set of linear model for each of the distributions and compare the GD. The results of the histDist() are shown above.  </div>  <br/>
&emsp;&emsp;&emsp;![Histogram](/Images/Picture4.png) 
&emsp;&emsp;&emsp;![Histogram](/Images/Picture5.png)  
&emsp;&emsp;&emsp;![Histogram](/Images/Picture6.png) 
&emsp;&emsp;&emsp;Figure 4  histdist() for all the distribution: NO, GA, IG, SEP1, TF, exGAUS  

###  1.3 Distribution choice  

<div align="justify"> We have run all the gamlss objects and compared the global deviance and the histDist() graphs results to determine the best distribution to fit. BCPE and exGauss are performing better in terms of Global Deviance. The summary() function shows that the GD of the BCPE is 1721.73, and the exGAUS is 1723.54. However, as the models are not nested, the global deviance is not a good indicator for comparison. 
The GAIC(k) is a better estimator for the comparison of the models. The GAIC tells nothing about the absolute quality of a model, only the quality relative to other models. The GAIC(k) is obtained by adding to the fitted global deviance a fixed penalty k for each degree of freedom used in a model, which is an increasing function of the numbers of the estimated parameters, the penalty discourages overfitting, because increasing the number of parameters in the model always improve the goodness of the fitting:
GAIC(k)= GD +(k * df);
Testing the distribution under the AIC that uses k=2 and SBC that uses k=log(n) and different values of k can help. The sensitivity of the selected model to the choice of k can be also tested. Testing the models built on the distributions selected we have this result for k=2 (AIC) k=~6(SBC, log(n=403) and k=2.5,3,3.5,4.  </div>  <br/>
![Histogram](/Images/Picture7.png) &emsp; ![Histogram](/Images/Picture8.png)  
![Histogram](/Images/Picture9.png)

Figure 5 GAIC(k) result: k=2, k=6, k=3, k=3. 5, k=4  

<div align="justify"> Given the result of the GAIC test, we can assume that the best distributions to fit our data and build the model on it is the exGAUS and or the BCPE. As the penalization with respect to the number of parameters of the model is larger k=2,3,3.5,4,6 the best model is changing. For a small number of k the BCPE is chosen for a larger number of k ExGAUSS is selected. Following the criteria of the GAIC, I will select the exGAUS. The plotted fitted distribution and the simple linear model of the exGAUS are shown in Figures 6 and 7.   </div>  <br/>
![Histogram](/Images/Picture10.png) &emsp;&emsp; ![Histogram](/Images/Picture11.png)  

Figure 6 Plotted pdf(BMI) fitted with the exGauss distribution  

<div align="justify"> The exGaus distribution is fitting well the distribution of our data dealing with the skewness and the kurtosis. The fitted parameters of the distribution are shown in the next paragraph.  </div>  

![Histogram](/Images/Picture12.png)  

Figure 7 Linear regression model using exGaus distribution  

###  1.4 Output the Parameters  

<div align="justify"> The outcomes of the linear model fitted with exGAUS distribution are shown in Figure 8. The univariate function is expressed as y = ß0 + ß1 (age), where ß0 is the intercept of value 11.2145 and ß1 = 0.4365. The sigma and nu coefficients express the value of the fitted sigma and the nu fitted value. The sigma coefficient of our fitted model is the natural logarithm of the fitted variance 1.29132 of the distribution. The same goes for the nu given the log link function. The AIC has a value of 1727.725, which alone doesn't have a specific meaning but only when compared with other models, as mentioned above..  </div>  <br>
![Histogram](/Images/Picture13.png)
![Histogram](/Images/Picture14.png)  

Residual of the fitted distribution and the fitted model are show above  

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;![Histogram](/Images/Picture15.png)  



