# MIE1628_Assignment_5
 
**Azure for Machine Learning:**

![image](https://github.com/user-attachments/assets/2e3d0afe-9317-4268-81a9-9d59368d04dc)

Azure Data Lake: Provides scalable storage for relational, semi-relational, and/or unstructured data

Azure Databricks: Data analytics platform optimized for Microsoft cloud services platform

Azure Data Factory: Cloud-based data integration service for Extract, Load, and Transform pipelines

Azure Synapse Analytics: Analytics engine designed to process big data quickly using parallel processing architecture

Azure Cosmos DB: Globally distributed, massively scalable, multi-model NoSQL database service for modern app development.

To ingest data: Azure Data Factory offers a scale-out, managed data movement solution. It allows users to ingest data with Azure Machine Learning.

Data store: Azure Data Lake allows storing any type of data of any size while Azure Cosmos DB only stores NoSQL data. Since the architecture deals with structured and unstructured data, Azure Data Lake stores any ingested data.

Prepare and Transform Data: Azure Databricks allows the preparation and the transformation (i.e. clean, sort, merge, join, etc.) of data in a notebook environment. It is used to prepare and transform the stored data.

Model and serve data: Azure Synapse Analytics is a limitless analytics service able to fulfill the needs of complex big data analysis and modeling.

**Question 2: Explain how Stream Analytics works in Azure.**

Azure stream analytics can be used for real-time data ingestion and processing in the Azure cloud. Azure Stream Analytics allows the user to input (ingest) real-time data, query (analyze), and output (deliver). Data is ingested from Azure Event Hubs, IoT Hub, or Blob Storage. It allows simple transformation and queries to be applied to the data as well as machine learning. Analysis can be performed based on SQL query language for filtering, sorting, aggregating, and joining streaming data over some time. Transformed data can be sent to other services such as Azure Function, Service Bus, or event hubs, or can be sent to Power BI for visualization purposes and real-time dashboarding. Users can also store the data in other Azure storage services for training machine learning models.

**Question 3: Implementing a Stream Analytics job by using the Azure portal**

Implemented a stream analytics job using Azure Portal and Internet of Things Hub (IoT) to retrieve the sample's temperature in real-time.

**Part B – Implement in Azure Machine Learning using Azure Notebook**

Dataset: https://archive.ics.uci.edu/ml/datasets/Gas+Turbine+CO+and+NOx+Emission+Data+Set 

**Problem Statement**

The gas turbine is very useful in the modern world since it is relatively compact and creates a lot of power. Gas turbines are used in backup power systems in Manhattan, for example, when the grid goes down due to natural disasters, gas turbines power up and can produce power for emergencies.
The dataset contains 36733 instances of 11 sensor measures aggregated over one hour (using average or sum) from a gas turbine located in Turkey's northwestern region to study flue gas emissions, namely CO and NOx (NO + NO2). Some of the features listed are ambient temperature, pressure, humidity, etc. We can examine which main features are used for predicting hourly net energy yield.

By determining which features are the most significant in determining the Turbine energy yield, we can determine if the environment is suitable for the energy yield required (clients will have a minimum energy output needed if used as a backup), this can save a lot of cost by researching the environment and critical components before implementing and finding out that the location chosen is not suitable. By comparing experimental results with theoretical results for gas turbine energy yield, the user can determine if any external factors are significant that are not being included.

There will be no data cleaning required since there are no N/A or missing values inside the dataset, however, the data is separated by years throughout 5 different years. We will merge the first three years to be used as our training data and the last two years will be used as our test data to determine how accurate our model is, this will be a 60/40 split. Since the data does not require data cleaning, more effort will be placed on exploratory data analysis and modeling. We will spend a portion on examining the gas emissions (CO and NOx), due to the rising concerns of climate change and renewable energy.

**List of Exploratory Data Analysis for the Dataset**

1. Correlation Plot - Can see which features are related to one another
2. Histogram - see the count distribution of each feature
3. Plot each feature against TEY to see the direct relationship
4. Pairplot to see the relationship between all features
5. Time Series to compare changes/differences between years for TEY and CO (Worse for Global warming) to see significant differences

No null/missing values, no need for data cleaning/preprocessing

**Correlation Plot:**

![image](https://github.com/user-attachments/assets/a41fc303-64b9-49ef-b868-9cbf48574525)

Significant Values:

![image](https://github.com/user-attachments/assets/3083a1d3-2c06-49ca-8598-b975b98306de)

We can observe that multiple features are correlated with one another, mostly the compressor features and not regarding the environment conditions. 

Features such as the air filter difference pressure, gas turbine exhaust pressure, turbine inlet temperature, turbine energy yield, and compressor discharge pressure are highly correlated with one another. 

For the Turbine energy yield, we can observe that the values are highly correlated with the gas turbine exhaust pressure, turbine inlet temperature, and compressor discharge pressure

**Histogram**

![image](https://github.com/user-attachments/assets/4437a492-1471-4260-a0bb-a215d3d4f70e)

We can observe that some features have a similar distribution to TEY such as the CDP and a bit of GTEP, we can also observe that AP and NOx follow similar to normal distribution based on the bell-shaped curve

**Probability Plot**

![image](https://github.com/user-attachments/assets/263ff63d-4a08-4d23-9790-47896cef75bc)

We can observe that the ambient pressure there follows a normal distribution, the amount of NOx emissions has a normal distribution around the average values and skews around the ends.

**Find the relationship between features and Turbine Energy Yield using Scatter Plots**

![image](https://github.com/user-attachments/assets/e5e86b89-110a-4797-9569-02e2273d9bf9)

We can double-check the relationship by plotting it against TEY, it is observed that GTEP and CDP fall in a straight line, thus following a similar distribution as the TEY.  

**Gas Emissions and Global Warming Concerns**

Observe the relationship between TYE and the CO and NOx emissions

![image](https://github.com/user-attachments/assets/e10ed552-1752-488b-9ecf-8457cb5a04cb)

There's also an objective to minimize the emissions of gas turbines while maintaining the purpose of the gas turbine, high yield energy. It seems that a high energy yield does not mean higher CO or NOx emissions

**Pairplot to see related features**

![image](https://github.com/user-attachments/assets/d8805844-c251-41ef-8312-28df8e10c944)

We can see a linear relationship between GTEP, TEY, and CDP

**Time Series**: See if there's any difference between the years, 2011, 2012, and 2013

![image](https://github.com/user-attachments/assets/e120dfbf-ef29-44d1-9e52-015af718613a)
![image](https://github.com/user-attachments/assets/b26969f3-4c25-44bf-ba86-5043ce1e2d45)
![image](https://github.com/user-attachments/assets/9da3cb3c-c069-4a64-aec5-2b1d4bf7e0b1)

We can see that the minimum and maximum range for the Turbine Energy Yield stays the same throughout the years, we see that the average energy output is slightly decreasing. In 2011, we can observe that the energy yield was higher for the first quarter, and for the middle of the graphs, there are many more low points, this may be because we require more energy/electricity in the winter compared to the summer.

**Part 4: Machine Learning Model using Azure**

**Multiple Linear Regression** model was chosen to predict the Turbine energy yield, this is one of the simplest predictive regression models. We can see if a linear scaling of the features can be summed up to determine the turbine yield energy. Linear regression predicts the weights of each feature. It used the method of ordinary least squares, minimizing the sum of squared residuals (SSR) to determine the best weights.

The accuracy score is high at about 98.31%, the RMSE is also low at about 1.94 MWH considering it goes on average to 130 MWH. R2 score is the coefficient of determination, the amount of variation in the target (Turbine energy yield) that can be explained by the dependence of the input features. We can further improve the accuracy by hypertuning the model. But this would be easier to leave the hyperparameter tuning to AutoML in Part B-5.

We ran the model against the training data again to see whether it was overfitting or underfitting, the model did slightly better on the training data compared to the test data so the model still has relatively high R2 scores. The simple multi linear regression model is slightly overfitting the training data.

Since the data is normalized, the bias is set at around the average value. We can examine the weight of each feature. We can observe that a higher Turbine inlet temperature and Compressor Discharge pressure are correlated with a higher turbine energy yield. We can also see that Turbine after temperature is negatively correlated with a higher energy yield (most of the energy turning into heat possibly).

![image](https://github.com/user-attachments/assets/b3cdfd2c-e848-4b75-b582-6f44c94b8a3a)

**XGBoost** stands for Extreme Gradient Boost, boosting is an ensemble technique where new models are added to correct the errors made by existing models. Models are added sequentially until no further improvements can be made. Gradient boosting is an approach where new models are created that predict the residuals or errors of prior models and then added together to make the final prediction. It is called gradient boosting because it uses a gradient descent algorithm to minimize the loss when adding new models. XGBoost is a library for developing fast and high-performance gradient-boosting tree models that can be used for solving regression-type problems.

First tried this model with a learning rate of 0.1, and got the RMSE at around 46, this meant that the model was overfitting. Now the RMSE is around 4.395 MWH which is relatively low with an R2 score of 91.3% without too many hyperparameter tuning.

We ran the model against the training data again to see whether it was overfitting or underfitting, the model did slightly better on the training data compared to the test data so the model still has high R2 scores. The more complex XGBoost model is slightly overfitting the training data.

![image](https://github.com/user-attachments/assets/99ff2994-26bf-4185-b880-7fccae08ae9c)

Our simple multiple linear regression model performed better than the complex XGBoost model in terms of having a higher accuracy (R2 score) of (98.31% vs 91.38%) and a lower RMSE value (1.94 vs 4.395 MWH). From the plot, we can see that the multi-linear regression model does a good job predicting the values except when it gets to the extremes (100-110 or 160-170 MWH). From the XGBoost model, we can see that we are overfitting our model to the training data since it has such a large spread and a relatively low R2 score. If we were to hyper-tune the models and run cross-fold validations with the training data, the model would give better results. This will be performed in part 5 by the AutoML program.

**Question 5: Perform Automated ML for your dataset**

List of models that Azure tested to determine which model provides the best results:

![image](https://github.com/user-attachments/assets/5a51767c-3425-475f-9221-9f6c895f2b4e)

Out of the multiple models that were tested, Voting Ensemble was the best model chosen. A voting ensemble (or a “majority voting ensemble“) is an ensemble machine learning model that combines the predictions from multiple other models. It is a technique that may be used to improve model performance, ideally achieving better performance than any single model used in the ensemble. We can further investigate the best model.

It has a very high accuracy (R2 score) and a relatively small RMSE value. It can explain most of the variance with the features provided.

![image](https://github.com/user-attachments/assets/fa1505d4-fb54-4b66-bebd-f4b2e4a1bc40)

We can see that most of the mean squared error is close to 0.276, RMSE is 0.5256 MWH, and the average prediction of the energy yield is 133 MWH. There are still a few outliers predicted by the model that result in large errors. The R2 score is much higher than the multi-linear regression model and the XGboost model I used in Part B-4. The RMSE is also significantly smaller as well compared to those two models.

Comparing the true and predicted Turbine energy yield, we can observe that the model has the most outliers near the average value, slightly overestimating the energy yield.

![image](https://github.com/user-attachments/assets/584d059d-64ff-4e7e-b4f6-d39f085fdf1e)

We can determine the 4 most important features in determining the Turbine Energy Yield, which are: Compressor discharge pressure (CDP), Turbine inlet temperature (TIT), Ambient temperature (AT), and Gas turbine exhaust pressure (GTEP). We can see that CO and NOx emissions aren’t key features in calculating the Turbine Energy Yield, thus, the next steps would be to reduce the emissions without indirectly affecting the energy yield.

![image](https://github.com/user-attachments/assets/86ec3bb4-210d-4bfd-b8bb-ffc873196333)

We can see the feature importance of the compressor discharge pressure; a higher discharge pressure will have more significance on determining the turbine energy yield. We see that this has the largest significance on the energy yield and should be prioritized when creating/maintaining a gas turbine.
