## **B1. Problem Formulation
**
(a)

This can be framed as a machine learning problem where the goal is to predict how well a promotion will perform at a given store in a given month.

Target variable: items_sold
Input features: store-related factors (store size, location type, competition density), promotion type, and time-related features (month, weekend/festival flags). We could also include past sales if available.

This is a regression problem because the target (items sold) is a continuous numeric value. The idea is to estimate expected sales under different promotions and then choose the best one.

(b)

Using total revenue can be misleading in this context because different promotions affect pricing differently. For example, a heavy discount may increase the number of items sold but reduce revenue per item.

Items sold is a better measure here because it directly reflects customer demand and response to promotions. It focuses on volume rather than pricing effects.

More generally, this highlights an important principle in ML projects:

The target variable should closely reflect the business objective and should not be distorted by external factors.

(c)

Instead of using a single global model for all stores, a better approach would be to account for differences between store types.

One option is to build separate models for different segments (e.g., urban vs rural). Another option is to use a single model but include interaction effects between store characteristics and promotion types.

This makes sense because customer behavior and promotion effectiveness can vary a lot depending on location, and a single model may average out these differences.

## **B2. Data and EDA Strategy**

(a)

The data would come from four tables: transactions, store attributes, promotions, and a calendar table.

These can be joined as follows:

Use store_id to join transactions with store attributes
Use promotion identifiers to join promotion details
Use transaction_date to join with the calendar table

The final dataset should ideally have one row per store per month, since decisions are made monthly.

Before modelling, I would aggregate:

Total items sold
Number of transactions
Average basket size
Promotion used in that period

This ensures the data matches the level at which decisions are taken.

(b)

Before building a model, I would perform the following EDA:

Sales by promotion type (bar chart)
To see which promotions generally perform better
Monthly sales trend (line plot)
To identify seasonality or patterns over time
Sales by store type (box plot)
To understand how different store types behave
Correlation heatmap
To check relationships between variables

These analyses would help decide which features to include and whether more complex models are needed.

(c)

If 80% of transactions happen without promotions, the dataset is imbalanced.

This could cause the model to focus too much on non-promotion cases and underestimate the effect of promotions.

To address this:

Ensure promotions are well represented (sampling if needed)
Include clear promotion-related features
Evaluate model performance separately for promoted vs non-promoted cases


## **B3. Model Evaluation and Deployment
**
(a)

Since the data is time-based, I would use a temporal train-test split. For example, train on the first ~2.5 years and test on the most recent months.

A random split is not appropriate because it can mix past and future data, leading to unrealistic performance (data leakage).

For evaluation:

RMSE helps identify large prediction errors
MAE gives an average error that is easy to interpret

Lower values of both indicate better performance.

(b)

To understand why the model recommends different promotions for the same store in different months, I would look at feature importance.

For example, December might have strong festival-related effects, making loyalty-based promotions more effective. In contrast, March might have lower demand, where discounts work better.

By comparing feature values across months and looking at which features drive predictions, we can explain the model’s decisions in business terms.

(c)

For deployment:

Save the model
Use tools like joblib to store the trained pipeline
Monthly predictions
Each month, collect new data
Apply the same preprocessing
Generate predictions for all stores
Output
Recommend the best promotion for each store
Monitoring
Track prediction vs actual performance
Monitor metrics like RMSE/MAE over time
Check for data drift
Retraining
Retrain the model if performance drops or business conditions change