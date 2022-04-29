
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
    ```sh
    #Installing the pyspark and seaborn library
    !pip install pyspark
    !pip install seaborn
    ```

- Thereafter, saved the uploaded data set to a dataframe and started the spark session
    ```sh
    spark = (SparkSession.builder.config("spark.driver.memory","4g").config("spark.driver.maxResultSize", "4g").getOrCreate())

    df = spark.read.csv(path='/content/games.csv', inferSchema=True, header=True)
    ```

- Executed EDA by using spark.sql() queries on the dataframe and 
  drawing necessary inferences from the data.
- By running the queries successfully, I plotted line plots, pie charts,
  and frequency distribution charts from the tables obtained by the
  queries.
- Trained a simple model using XGboost and Decision Tree Classifier
  for predicting the winning rate for white with respect to the number
  of turns and the victory status.

    ### Overview of the Queries

    #### Query for Top 10 players playing Black
    ```sh
    q3 = spark.sql('''SELECT black_id, black_rating FROM Schema WHERE black_rating>2000 ORDER BY black_rating DESC''')
    q3.show(10)
    ```
    #### Query for Plotting number of turns with white and black rating with Visualization using Seaborn

    ```sh
    q14 = spark.sql('''SELECT black_rating, turns FROM Schema where black_rating<2000''').toPandas()
    fig, axes = plt.subplots(figsize=(10,7))
    sns.lineplot('black_rating', 'turns', data=q14, ci=95).set_title("Trend for Number of Turns per Black's Rating")

    ```

    #### Query for creatig a Pie chart for Registered Rated Players

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

### Analysis for the Queries:

- Performed Average, Min and Max for all the players based on their number of turns and found that most of the players fall that players with higher rating tend to play more number of turns with respect to lower rated players.
- By plotting the number of turns for black and white rating, the inference could be drawn that number of turns increases approximately linearly with the player's rating.
- The most popular opening played by all the players is the "Indian Game"
- Calculated the count for all types of victory statuses. 

    | victory_status      | count(victory_status)
    | :---        |    :----:   
    | draw      | 906       
    | mate   | 6325    
    | out of time      | 1680       
    | resign   | 11147

- The top 4 most popular increment type played on Lichess:
    | increment_code      | count(increment_code)
    | :---        |    :----:   
    | 10+0      | 7721       
    | 15+0   | 1311    
    | 15_15      | 850       
    | 5+5   | 738

- Most of the players on Lichess lies in the rating between 1250 and 2000 based on the freqency distribution plotted. 


    #### Example of Freqency Distribution Query for White Rated Players    
    ```sh
    #Frequency Distribution for White Rated Players
    q23 = spark.sql('''SELECT white_rating from Schema ORDER BY white_rating ASC ''').toPandas()
    sns.displot(q23).set_xlabels("White Rating").set_ylabels("Number of Players").set( title="Frequency Distribution for White Rated Players")
    ```

- 80.5% of the players on Lichess are rated and 19.5% players on Lichess are not rated.
- Trained a model for predicting the effect of Winning Status based on Number of Turns and victory status using XGboost and DecisonTreeClassifier.

    1. XGBoost Accuracy: 0.7808157099697886
    Approximately 78% of the data is predicted by the model efficiently.

    2. Decision Tree Classifier Accuracy: 0.899546827794562
    Approximately 89.9% of the data is predicted by the model efficiently.
