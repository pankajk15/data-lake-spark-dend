# Introduction

Music streaming App Sparkify, has grown their user base and song database even more and want to move their data warehouse to a data lake. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

As their data engineer, you are tasked with building an ETL pipeline that extracts their data from S3, processes them using Spark, and loads the data back into S3 as a set of dimensional tables. This code represents an ETL pipeline for the Sparkify startup, which has decided to move its data storage and analytics to AWS, so that growing datasets can be stored and processing using Spark, in the cloud.

# ETL Pipeline

ETL pipeline transform two sets of JSON files stored in Amazon S3 into 5 fact and dimension tables that can be loaded back into Amazon S3 and processed using Spark. The ETL pipeline is written in PySpark.

The JSON consist of following data:
* Log from users
* Details usgae of the Sparkify music app
* Data of the songs and artists available to listen to in the app

## Running the ETL Pipeline 

1. Add your AWS credentials to the ```dl.cfg``` file
2. Specify your input and output S3 bucket locations in the ```dl.cfg``` file
2. Create new ipython file(python3), and run ```%run etl.py```

# Database Schema Design

From the underlined dataset 5 tables will be created that forms the new database in Amazon S3.

### songplays Fact Table

This table records details of each of the music app events that involve the ```NextSong``` action.

The output table is created by joining both the log data JSON file and the ```songs``` table described below, on both the artist names and song names columns.

The output table is partitioned in S3 by the ```year``` and ```month``` column values, to ensure an even distribution of data, and optimal query performance.

### users Dimension Table

This table records details the users of the Sparkify music app, having details of paid or free accounts.

It can be joined to the ```songplays``` table using the ```user_id``` primary/foreign key column to give more information on the users listening to given songs.

The output table is partitioned in S3 by the ```gender``` column, to allow for more even distribution of data.

### artists Dimension Table

This table records details the artists who sing the songs that users listen to in the Sparkify app.

It can be joined to the ```songplays``` table using the ```artist_id``` primary/foreign key column.


### songs Dimension Table

This table records details the songs that users of the Sparkify app are listening to.

It can be joined to the ```songplays``` table using the ```song_id``` primary/foreign key column to give more information on the songs themselves.

The output table is partitioned in S3 by the ```year``` and ```artist_id``` columns, to allow for more even distribution of data.


### time Dimension Table

This table records details for more granular data on the timestamps of the ```NextSong``` events held in the ```songplays``` table - e.g. broken down into columns for ```day``` and ```month```

The output table is partitioned in S3 by the ```year``` and ```month``` columns, to allow for more even distribution of data.

