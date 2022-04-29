
# Exploratory-Data-Analysis-on-Lichess-Dataset using Apache Spark 

The project is based on a dataset of approximately 20,000 entries of number of games of Lichess. The dataset contains various features such as White Rating, 
Black Rating, white and black ids, moves, number of turns etc. The project is deployed on Google Colab and Kaggle. I have performed data analysis by using PySpark queries 
and have visualized the data using seaborn library. Along with the data analysis I have deployed the model for prdicting the Winning Status based on number of turns and victory status.

## Problem Statement
- To Perform EDA on Lichess Dataset 
- Predicting Winning Status on Lichess based on 
         the number of turns and victory status(Ex. Draw, Resign etc.)


## Solution to the Problem Statement


- Executed exploratory data analysis on the data set by applying Spark SQL queries on data frames
- By the acquired results from queries, we could plot line plots, piecharts, and distribution plots for data visualization.
- Aggregation analysis on continuous data sets for deriving descriptive statistics from the data.
- Training the model by using XGboost and DecisonTree Classifier for prediction of winning rate.

## Implementation of the Solution

- Firstly, imported all the required libraries and uploaded the data set
   on google colab
- Thereafter, saved the uploaded data set to a dataframe
- Executed EDA by using spark.sql() queries on the dataframe and 
  drawing necessary inferences from the data.
- By running the queries successfully, I plotted line plots, pie charts,
  and frequency distribution charts from the tables obtained by the
  queries.
- Trained a simple model using XGboost and Decision Tree Classifier
  for predicting the winning rate for white with respect to the number
  of turns and the victory status.

    ### Overview of the Queries

    Query for Top 10 players playing Black
    ```sh
    q3 = spark.sql('''SELECT black_id, black_rating FROM Schema WHERE black_rating>2000 ORDER BY black_rating DESC''')
    q3.show(10)
    ```
    Query for Plotting number of turns with white and black rating with Visualization using Seaborn

    ```sh
    q14 = spark.sql('''SELECT black_rating, turns FROM Schema where black_rating<2000''').toPandas()
    fig, axes = plt.subplots(figsize=(10,7))
    sns.lineplot('black_rating', 'turns', data=q14, ci=95).set_title("Trend for Number of Turns per Black's Rating")

    ```

    Query for creatig a Pie chart for Registered Rated Players

    ```sh
    q26 = spark.sql('''SELECT rated from Schema where rated='TRUE' ''').count()
    q27 = spark.sql('''SELECT rated from Schema''')
    temp = q27.count()
    a = (q26 * 100) / temp #Answer = 80.54%
    values = [a,100-a]
    exp_labels = ["Rated","Not Rated"]
    plt.title("Pie Chart for Registered Rated Players on Lichess")
    plt.pie(values,explode=None,labels=exp_labels,colors=None,autopct="%0.1f%%")
    ```
## Analysis of the Solutions Obtained

