# DiHabitIs,  a data story made by ShareLoc
In a [study](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7029018/) analyzing a record of 420M food items purchased at
Tesco in greater London, researchers identified nutrients that were highly correlated with diabetes. They also concluded that diversifying nutrients is key to avoid this disease. 

We, ShareLoc, would like to dig further in this dataset and look if other factors correlate with diabetes risk. We are mainly going to look if age and income have impact on diabetes (in an area scale). 

## Young, fats and sweets
Our first step is to differentiate areas by age. We have access to three groups of age : 
- young : 0 to 17 years
- adult : 18 to 64 years
- old   : 65+ years

We computed the age densities of each population (e.g young_density = #young/total_population) and hereby plot the
correlations between the age densities and diabetes prevalence :
(N.B: we only took into account correlations having a p-value < 0.05, for our entire study)

{% include_relative html/age_diab_corr.html%}

**Surprising !** The younger the population, the higher its diabetes prevalence score. Older population tend not to be
affected though. So what do younger areas like so much that adults don't like ? To check this, we plot the correlation between age density of youngs and adults vs the products distribution of the areas. 

{% include_relative html/age_prod_corr.html %}

Looking at the yellow bars, we identify a little cliché of the teenager who eats cereals in the morning, says **no** to fruits and veggies but **yes** to sweets and soft drinks ! Of course,the teenager can not buy beer and wine for legal reasons. But weirdly enough, younger populations tend to buy more spirits... We let the reader imagine why. 

Having the food habits of the younger population under the eyes, we now look for the correlations between diabetes prevalence and product consumption:
{% include_relative html/prod_diab_corr.html %}

What a surprise ! Younger areas buy more products that are positively correlated with diabetes.

Furthermore, surprising observations emerge from these graphs. In particular, we
notice that water has high diabetes prevalence. We do not think that bottled water is direclty related to diabetes. However, perhaps areas buying more bottled water instead of water from the tap are areas with lower quality of life. We do not have yet access to data permitting us to dig further in this aspect, and therefore leave this for further study. 

## Modeling diabetes prevalence
From the above barcharts, we are interested in viewing a scatter plot of young density vs diabetes prevalence. We add two features to the scatter plot: the entropy of nutrients represented by the color and the number of gp patients represented by the radius. We decided to include the entropy (h_nutrients_calories), i.e, the level of diversification in the areas average diet, because it is one of the main variable of the previous research. We also include the number of gp patients to check if there is no bias at that scale. 

{% include_relative html/age_diab_scatter.html %}

It appears that younger areas food habits are correlated with low entropy. This is consistent with the previous research which shows that entropy is negatively correlated with diabetes and our above discoveries which show that younger areas are positively correlated with diabetes. 

Let's now switch the y-axis and the color-axis of the previous graph:
{% include_relative html/h_age_scatter_diab.html %} 
Based on this, we would like to perform a classification model that would estimate if an area has a high (1) or low (0) diabetes prevalence based on only two features : young density and entropy of nutrients. The fact to have two features is convenient for visualization. 

We have selected the threshold for high and low based on the diabetes prevalence median, that is, a score of 6,1. 

{% include_relative html/true_plot.html %}

We tried two models: Logistic Regression and Random Forest Classifier. We split the data into a train and test set. We only train the model with the train set and then plot the overall prediction of the model below (train + test set)
### Logistic Regression
{%include_relative html/LogisticReg.html%}
We can identify a clear linear cut for the decision frontier. It does not take into account the intersections of points in the middle of the plot, but identifies a good frontier from which probability becomes higher to belong to class 0 (left) or 1 (right)
### Random Forest Classifier
{%include_relative html/RandomForest.html%}
This model handles better mixed labels and therefore has a more convincing result. We are then going to evaluate its performance on the test set only. 
### Performance evaluation
First, we check a histogram of its scores (between 0 and 1) and true values on the test set.
{%include_relative html/score_hist.html%}
This is not a bad result, the model tends to rightly classify the points more frequently thant it does mistakes, especially for the low diabetes cases. 
Below we plot the TPR and FPR of the model, according to certain thresholds:
{%include_relative html/fpr_tpr_test.html%}
And finally we plot its ROC curve:
{%include_relative html/roc_rf_test.html%}
We reach an AUC of 0.88 for the test set, which is not bad ! 

## What's On with Net Income ?

Finally, we would like to observe if there are any differences on products consumption between richer or poorer areas and if so, we would like to know if it has an impact on diabetes. To do this, we start by plotting the average net annual income of local authorities. Note that we have access to annual income data only for MSOA and that the above study was performed using OSWARD data. We therefore do not have access to diabetes prevalence score for the income analysis, but we can relate to the previous findings to interpret our results. 
 
{% include_relative html/Income_classes.html %}

In this chart, local authorities are separated in 3 categories based on their average net annual income. We used the 1st and 3rd quartile of the income distribution to diferentiate low and medium income areas, higher income areas lie then above the 3rd quartile. We now plot nutrient consumptions grouped by income groups:

{% include_relative html/prod_income.html%}

We do not notice huge differences in terms of product consumptions, but a key observation is that higher income classes
tend to buy more products that are negatively correlated with diabetes prevalence whereas lower income buy more 
products that are positively correlated with diabetes prevalence. Low income areas buy 83% more bottled water than high income areas. It might be because tap water quality is not as good as in richer areas.  

## Conclusion and discussion
We have been able to define low diabetes prevalence products for customers seeking healthier food habits.
We investigated the contribution of age groups within areas with diabetes prevalence and have found that younger areas have higher diabetes prevalence scores. 
This is due to the fact that younger populations tend to buy products that are positively correlated with diabetes prevalence.
Scatter plots stress on this relationship as younger areas have low entropy which is respectively positively correlated with diabetes.

From the income point of view, high income areas are considered healthier as they have the tendency to buy products negatively correlated (fruits and veggies, dairy, wine) with diabetes unlike low income areas food consumptions(sweets, fats).

The results of this study are to interpret with caution. Indeed, we only have access to aggregated data and therefore only see the trends in consumption by areas. Interesting further research would be to determine other causes of why younger populations tend to have higher diabetes, and why diabetes is positively correlated with botlled water consumption. 
