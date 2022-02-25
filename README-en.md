
### :pushpin: [__Ler em Português__](https://github.com/pedropscf/2-churn-prediction/blob/7a62f7f1bb330e555428179a1338b84751c97ea7/README.md)

# Detecting and avoiding churn of customers at a bank

The TopBank is a fictitious company that offers a variety of banking services, with its main product being a account where they offer a wide variety of products, from investments to insurances.
When customers are active, the company can offer and obtain recurrent revenue from the services. However, the company identified a problem where some customers are no longer using the services, which results to revenue loss.

The company wants to minimize or avoid this from happening and they need the answers for the following questions:

- **What are the main characteristics of churned customers?**
- **Who are the next possible churners?**
- **How to avoid the possible churners from actually churning?**

## Features

To answer these questions, the company sent a dataset with features of around 10,000 different customers across France, Spain and Italy. The features of the dataset are:
| Feature  | Description |
| ------------- | ------------- |
| CustomerID  | Unique customer identifier |
| Surname  | Customer's surname  |
| CreditScore  | Credit score number  |
| Geography | Customer's country |
| Gender | Customer's gender |
| Age | Customer's age |
| Tenure | Number of years of the account |
| Balance | Value deposited |
| NumOfProducts | Number of products active |
| HasCrCard | Whether the customer has a credit card |
| IsActiveMember | Wheter the customer is active |
| EstimatedSalary | Estimated yearly salary |
| Exited | Whether the customer churned |


## Solution

The problem was classified as a classical supervised machine learning problem involving
a binary classification(churned or not churned) of different customers. The solution
was develop **in cycles**, making possible to make incremental improvements on the solution over the time,
and the full detailed solution along with all the code, [can be seen in this file](https://github.com/pedropscf/2-churn-prediction/blob/bd1346f98c4d54d71cf4fdefa8c829d0afabe58b/m03_churn_prediction.ipynb). It can be summarized in the steps below:

At this point 20% of the data were split (stratified, in order to mantain the 20% churn rate), where 80% will be used for the model construction (train and test)
and the other 20% will be used to **evaluate the business problem and financial return with the model.**

### Data cleaning and manipulation

Initially, the dataset were investigated looking for NA values, outliers and strange values along each of the columns, in
order to clean the data. However, the conclusion was that data received were already cleaned and ready for use.

### Exploratory data analysis

Then, we started a analysis looking if we could answer the first question raised by the company: **What are the main characteristics of churned customers?** Despite the fact that
the analysis didn't brought a clear answer for the question, it raised some points of attention for insight, such as:

- **Customers in Germany have higher churn rate than average;**
- **Customers with 0 balance in account have higher churn rate;**
- **Younger customers have higher churn rate;**
- **Customers with more than 2 products have a higher churn rate.**

These points opens the questions of why those things are happening. With a proper investigations, the team can act to solve those problems that can be caused by:
a new competitor in Germany or directed to younger people, customer experience problems for customers with many different products or even receiving a better offer in another account, resulting in balance 0.

### Feature Engineering

For feature engineering, the numerical data were transformed with the use of the MinMaxScaler, while the categorical data were transformed with the one hot encoding method.
No feature were created at this point of the cycle and the Boruta algorithm were used in order to rank importance of the features. All features were used.

### Model creation

For this stage, different models were created, both with and without balancing the data. The models created and evaluated were: LogisticRegression, RandomForest and XGBoost and the resampler used were the SMOTE.

After tuning, the results achieved with **the Random Forest model reached levels above 90% for accuracy, precision, recall, F1 and AUC.** The model and every scaler used were saved in Pickle format for further use.

### Model performance

The lift performance showed that, for the first 20% of the data, the probability to correctly classify a customer is 3 times higher than a random classification for the churned customers and ROC shows that with 20% of data it is possible to correctly classify 60% of the churners.
Also, a threshold analysis shows that customers with 64% or more predicted probability of churn, have higher churn rate than average. With these results, it is possible to answer the second question: **Who are the next possible churners?** A list with all customers with churn probability higher than
64% were sent to the company

### Optimization (business performance)

The company informed that they can estimate the revenue received from a customer based on their salary:

- Customers with income higher than average, return 5% of estimated salary in revenue
- While customers below average, return 3% of estimated salary in revenue.

With this information, the solution for the third answer (**How to avoid the possible churners from actually churning?**) is to give coupons for customers at risk of churn. Those coupons could be used for both using one of the company's services or gift card or stream service. The marketing team
provided a **budget of $20,000.00** to give for those customers.

Based on the estimated revenue, different categories of coupons were created, going from $20.00 for customers below $400.00 in estimated revenue to a coupon of $250.00 for customers above $5,000.00 in estimated revenue.

With this in mind, we selected the customers with highest probability of churn, and with the budget the following results were achieved:

- **The data with 2,000 customers, contained 407 churners, $9.17M estimated revenue and $1.78M revenue lost with the churners;**
- **Customers above 64% probability represented 312 and had 209 churners, representing more than 80% of the revenue lost;**
- **With the budget, 129 customers received the coupon, where 109 were churners. This represents a recovery of about $460,000 recovery with a investiment of $20,000;**
- **Doubling the budget to $40,000.00 it is possible to impact 250 customers, where 179 are churners, recovering about $800,000 from a $40,000 investment (2000% ROIC).**


## Technologies used

**Data manipulation and cleaning:** pandas, numpy

**Data visualization:** matplotlib, seaborn

**Machine learning:** Classification (scikit-learn and xgboost), Imbalanced learning (imblearn), Classification metrics (yellowbrick)



## About Me
I am data science enthusiast, learning new applications of Machine Learning, AI and Data Science in general to solve a diverse range of business problems.


## Authors

- Pedro Fernandes [@pedropscf](https://www.github.com/pedropscf)
