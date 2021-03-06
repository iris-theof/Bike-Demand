# Installation
Jupyter Notebook https://jupyter.org/install is the only prerequisit to run the project.

# Project Motivation
In this repository I present a model that uses Machine Learning, to forecast Bike Demand based on real data obtained from Kaggle  
https://www.kaggle.com/c/bike-sharing-demand. The prediction is based on data like date, hour, holiday and weather condition.

# File Description
The main file is the Bike_sharing_kaggle.ipynb, where the statistical description and the model is implemented. The data are found
on train.csv. The rest of the files are graphics useful for the statistical analysis and the prediction, produced when one runs the 
notebook.

# How to Interact with the Project
Running the Jupyter Notebook Bike_sharing_kaggle.ipynb using the data from train.csv. Note that this notebook is not meant to provide a
submission file based on the test data for the kaggle competition https://www.kaggle.com/c/bike-sharing-demand. The predictions are done
on a cross validation set which we have taken out from the train.csv file. The reason for this is that we do not want to make predictions
based only on time and date as it was the target of the kaggle competition.

# Acknowledgments
I would like to acknowledge my friend Dr. Maria Stounrana for valuable input on this project. Kaggle is acknowledged for making available the
database.

# Main results summary
## Exploratory Data Analysis
An initial exploratory analysis ran on the data indicated the following:
* No missing data were found
* 'atemp' appear as highly correlated with 'temp', so it was removed from the dataset to avoid multicolinearity issues. The fields 'registered' and 'casual' were also removed as they would introduce data leakage in our model: we cannot know a priori whether a 'registered' or a 'casual' user will come along to rent a bike. 
![Correlation matrix](https://github.com/iris-theof/Bike-Demand/blob/main/correlation_matrix.png)

### Visualization of the data indicated several interesting points, summarized as follows:
* The usage is different on work days versus weekends. The weekend
activity indicates that people use more the bikes during afternoon, while during work days,
bikes are mostly used at rush hour, i.e. morning and afternoon hours, related to when people
go to and come from work.
![Weekday](https://github.com/iris-theof/Bike-Demand/blob/main/weekday.png)
* Casual users tend to use the bikes during afternoon during the whole week, while registered
users follow the first graph pattern (leisure on weekends and going to work on weekdays).
![Casual-Registered](https://github.com/iris-theof/Bike-Demand/blob/main/casual_registered.png)
* More bikes are rented when it’s sunny and in the fall, respectively,
whereas the usage tends to reduce during spring time. 
![Weather-season](https://github.com/iris-theof/Bike-Demand/blob/main/weather_season.png)
* A positive correlation between temperature and usage for most of the
temperature range was observed, which should intuitively make sense, as people are not
likely to bike outside in cold weather. For very high temperatures, which seem to be a smallsubset of the data, there is a dip in the curve, suggesting that users are discouraged to bikewhen it’s too hot outside. On the other hand, a negative correlation with the usage rate is
observed that could be explained based on the fact that higher humidity is correlated with
higher chances of rainfall.
![Temperature](https://github.com/iris-theof/Bike-Demand/blob/main/weather_humidity.png)

## Modelling

* It was observed that the 'count' variable contains lots of outlier data points that skew the distribution
towards right (as there are more data points beyond Outer Quartile Limit), as shown in Fig.6. In
addition to that, the fit for the count as a function of hour is far from linear, demonstrating lowest
usage is late at night (with the minimum between 4–5 am) and the peaks are during 8–9 am and 5–7
pm, which must correspond to rush hour.
![Before](https://github.com/iris-theof/Bike-Demand/blob/main/count_quantiles_before_log.png)
* To account for the skewed distribution, a log transformation on 'count' after removing the
outliers was taken, changing the distribution closer to normal.
![After](https://github.com/iris-theof/Bike-Demand/blob/main/count_quantiles_after_log.png)
* As a next step, a variety of parametric and non-parametric predictive models were evaluated
based on their mean squared error on cross validation set in order to decide on the model with the best performance.
It turned out that Random Forest had the smallest mean squarer error of 0.23.

| ML Algorithm             |  Mean squared error |
| -------------            | --------------------|
| LinearRegression         |0.38                 |
| Ridge                    |0.38                 |
| Lasso                    |1.74                 |
| DecisionTreeRegressor    |0.46                 |
| GradientBoostingRegressor|0.30                 |
| RandomForestRegressor    |0.23                 |
| BaggingRegressor         |0.26                 |

* After tuning our Random Forest using highparameter tuning we observe that the temperature and whether is a working day or not are the most
crucial factors on bike demand.

![Bike_Demand](https://github.com/iris-theof/Bike-Demand/blob/main/Features.png)

     
