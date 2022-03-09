# Kaggle Competition: WiDS Datathon 2022 (Random Forest)

# **Summary**

### **Introduction**

This is a predictive analysis for WiDS Datathon 2022, one of the ongoing Datathon on Kaggle. ([https://www.kaggle.com/c/widsdatathon2022](https://www.kaggle.com/c/widsdatathon2022))

I have participated in this competition with Vivian Yu and built regression model with Random Forest algorithm for predicting Site Energy Usage Intensity.

### **Datasets**

**Train Data** 

63 X variables including information about sites, monthly temperatures and other information related to climate for Year 1 ~ 6. y variable, Site Energy Usage Intensity is also included in the train dataset. The total number of rows is 75757.

**Test Data**

Same X variables for Year 7 are included in test dataset. Total 9,705 rows.

### **Language and libraries**

Python, Pandas, Sklearn, Seaborn

### **Data Preprocessing**

**Imputation** 

‘energy_star_rating’ column showed highest correlation(0.51) with y variable. However, around 1/3 of data in this column was missing. We set using ‘mean’ value for baseline imputation method, and compared with the result after using K-NN and Random Forest algorithms for imputation. The results indicate using mean value for this column provides best score, therefore, all missing values in this column are filled with the mean value of the column. Missing values in the ‘year_built’ column also imputed with its mean values.

**Feature Selection**

Initial modelling after including all variables showed great performance for train dataset. Train RMSE for this model was 14. However, test RMSE(40) after submission was significantly higher than Train RMSE, which indicates that the model is highly overfitting.

Therefore, we checked X variables’ distribution by each dataset(Train/Test).

![ex_screenshot](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5f1767d0-d0f6-434f-9a70-6c1cf9928b59/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220309%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220309T184527Z&X-Amz-Expires=86400&X-Amz-Signature=8203bf8dffda3b25b1826ac06b2be3e074d85e2d729e45a16089dd8644816f22&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

As above sample columns show, most variables related to temperatures and climates were having different distributions for its values. This indicates that Year 7(test dataset) had abnormal climate compared to Year 1 ~6(train dataset).  Due to these discrepancies between train and test dataset’s climate related variables, we decided to exclude these features and agreed to include 'energy_star_rating', 'facility_type', 'floor_area', 'year_built', 'State_Factor', and 'ELEVATION’ for our model.

**Taking log for y variable**

As y variable’s distribution is right-skewed, we took log for y values. This helped to improve the variable’s distribution, but it was not helpful for improving evaluation score when it is used for the model. Therefore, we didn’t take log for y variable for the final model.

![Raw distribution of y variable](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7b30c8ef-92f7-4984-8a31-55a9679809ca/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220309%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220309T184610Z&X-Amz-Expires=86400&X-Amz-Signature=d52a2664a8f06bf1a74440ed392f5dfb54cbca9b3436d9b333dfa711bfbfd24a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Raw distribution of y variable

![Distribution after taking log](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5a2b651d-2e2d-4817-bab3-cf75187622e3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220309%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220309T184622Z&X-Amz-Expires=86400&X-Amz-Signature=011ad51f331420436f2e0daefeec930b8afa6b7d1243a74415891e0341a2889e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Distribution after taking log

### **Model Selection**

**Model** 

We have tested Random Forest, Cat Boost and XGBoost for this dataset, and decided to use Random Forest as it showed lowest RMSE.

**Hyperparameter Tuning**

We tried to tune ‘max_leaf_nodes’ and ‘n_estimators’ to prevent overfitting and optimize the model. After running Random Grid Search for these hyperparameters, we found 150 estimators and max 100 leaf nodes showed best validation score.

![ex_screenshot](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6b713de7-164c-4920-8de0-33dd116e26d1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220309%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220309T184637Z&X-Amz-Expires=86400&X-Amz-Signature=68c14202b469bb353f8c240a82511ffe455e3a2dbee49da2c39cab73f48f9950&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

However, we decided to use default values for hyperparameters as submission score was not improved by the model trained with optimized values for the hyperparameters

### **Evaluation**

The competition’s evaluation metric is RMSE. 

Final model’s Train RMSE was 13.6 and Public score(RMES) for test dataset after submission was 29.5. (rank: 304/829)

### **Takeaways and Open questions**

**Importance of feature selection:**

Through this competition, I realized that using right features is truly important for building a model for prediction. As there are a lot of variables which can disturb training or cause overfitting of the model, conducting EDA to find these variables and examining each variable’s impact on the model was important. Through this process, the model’s accuracy was significantly improved.

**How can we use climate information?**

As topic of this competition is ‘climate change’, I still think that there are other ways that we can use climate information for our model. More brainstorming is necessary to find out use of these features.
