# DiHabitIs,  a data story made by ShareLoc

In a [study](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7029018/) analyzing a record of 420M food items purchased at
Tesco in greater London, researchers identified nutrients that were highly correlated with diabetes. 

We, ShareLoc, would like to dig further in this dataset and look if other factors correlate with diabetes risk. 
Note that purchases have been grouped into areas, meaning that we can not perform analysis on individuals, but rather 
have information about the general tendencies in a specific area.  

## Age study (make this title fun)

Our first step is to differentiate areas by age. We have access to three groups of age : 
- young : 0 to 17 years
- adult : 18 to 64 years
- old   : 65+ years

We computed the age densities of each population (e.g young_density = #young/total_population) and hereby plot the
correlations between the age densities and diabets prevalence : we only took into account correlations having a
p-value < 0.05

{% include_relative html/age_diab_corr.html%}

**Surprising !** The younger the population, the higher its diabetes prevalence score. Older population tend not to be
affected though. So what do younger areas like so much that adults don't like ? 

{% include_relative html/age_prod_corr.html %}

And how are these products affecting diabetes prevalence ? 

{% include_relative html/prod_diab_corr.html %}

In the two plots above, we can observe that young people tend to consume nutrients that
have high diabetes prevalence, such as fats oils and sweets.

Furthermore, more surprising observations emerge from these graphs. In particular, we
notice that water has high diabetes prevalence. Since we don't believe that water directly causes diabetes, we'll
look at the datasets more in depth.

Let us see then a scatter plot of young age density vs diabetes prevalence. We also included the entropy of nutrients
as a color feature, as correlation of entropy with diabetes prevalence is an important result of the previous research. 

{% include_relative html/age_diab_scatter.html %}

It appears that young people's food habits are correlated with low entropy. This is consistent with our previous work
(and the one presented in the paper), which show that entropy is negatively correlated with diabetes.

## Modeling 
Plot of entropy vs young density, with diabetes prevalence as color feature : 
{% include_relative html/h_age_scatter_diab.html %} 

Based on this, we would like to perform a classification model that would estimate if an area has a high, medium or
low risk of diabetes prevalence based on only two features : young density and entropy of nutrients. 

We have selected the thresholds based on the diabetes prevalence quartile distribution. 

{% include_relative html/classes.html %}

We chose to perform the logistic regression model, let's see how it performs by checking its ROC curves : 

{% include_relative html/rocs.html %}

We can see that the classifier performs very well for high and low diabetes risk. Quite naturally, it has more trouble 
with the medium ones, the elbow being reached at tp=0.8 and fp=0.5.


# Part on Net Income and local authorities
Do not hesitate to use the wonderful built in zoom tool that you can find on the top right !
{% include_relative html/corr_matrix_local_auth.html %}

Income classes (the Local authorities represent a great territory, hence a slight difference is still meaningful) 
{% include_relative html/Income_classes.html %}

Below lies a barchart of income classes and product purchases : 

{% include_relative html/prod_income.html%}

We do not notice big differences in terms of product consumptions, but a key observation is that higher income classes
 tend to buy more products that are negatively correlated with diabetes prevalence whereas lower income buy more 
 products that are positively correlated with diabetes prevalence. 
