# DiHabitIs,  a data story made by ShareLoc

In a [study](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7029018/) analyzing a record of 420M food items purchased at
Tesco in greater London, researchers identified nutrients that were highly correlated with diabetes. They also concluded that diversity in its diet is key to avoid this disease. 

We, ShareLoc, would like to dig further in this dataset and look if other factors correlate with diabetes risk. 
Note that purchases have been grouped into areas, meaning that we can not perform analysis on individuals, but rather 
have information about the general tendencies in a specific area.  

## Young, fats and sweets

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

Let's now switch the y-axis and the color-axis of the previous graph:
{% include_relative html/h_age_scatter_diab.html %} 

Based on this, we would like to perform a classification model that would estimate if an area has a high or low diabetes prevalence based on only two features : young density and entropy of nutrients. The fact to have two features is convenient for visualization. 

We have selected the threshold for high and low based on the diabetes prevalence median, that is, a score of 6,1. 

{% include_relative html/true_plot.html %}

We tried two models: Logistic Regression and Random Forest Classifier. 
### Logistic Regression
{%include_relative html/LogisticReg.html%}

### Random Forest Classifier
{%include_relative html/RandomForest.html%}

### Performance evaluation
{%include_relative html/score_hist.html%}



## Part on Net Income and local authorities

Do not hesitate to use the wonderful built in zoom tool that you can find on the top right !
{% include_relative html/corr_matrix_local_auth.html %}

Let's finally look at the influence of income on food habits. To do this, we start by plotting the average net annual
income of local authorities. These represent very large territories, therefore even a slight difference is interesting
to point at.
 
{% include_relative html/Income_classes.html %}

In this chart, local authorities are separated in 3 categories based on their average net annual income. We'll now
plot nutrient consumptions grouped by income groups:

Below lies a barchart of income classes and product purchases : 

{% include_relative html/prod_income.html%}

We do not notice big differences in terms of product consumptions, but a key observation is that higher income classes
 tend to buy more products that are negatively correlated with diabetes prevalence whereas lower income buy more 
 products that are positively correlated with diabetes prevalence. 
