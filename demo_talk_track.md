
In this video, I will cover how to use Trifacta to prepare data for Amazon Personalize

So let's begin by looking at: What is Amazon Personalize?

Amazon Personalize is a machine learning service that greatly simplifies the process of 
building robust recommender systems

Amazon has taken the vast experience it has gained from building such systems, and made it accessible
and simple to use for everyone.

You provide some historical data to the system representing user activity patterns and product attributes

Based on this information, Amazon Personalize will automatically train a machine learning model that is 
capable of producing high-quality recommendations that will result in greater user engagement and satisfaction

### Show slide: Datasets (29)

Amazon Personalize can work with 3 types of historical datasets.

You can provide User data, that includes various attributes of your users such as age, gender and loyalty membership

You also provide Item data, which consists of products or services being offered and their attributes

Thirdly, you can provide data about the Interactions between users and items, which serves to inform future recommendations

### Demo - Movies

#### Show flow: Personalize - Completed

In this demo, we will prepare MovieLens movie and ratings information and enrich it with IMDB. Our goal for data preparation is to produce clean datasets and schema definitions in the format expected by Amazon Personalize

This screen shows the completed flow in Trifacta. On the left are data sources representing the MovieLens and IMDB data that are being read from an Amazon S3 bucket. In the middle are the recipes for movies and ratings that define the data transformation steps to produce clean data. And finally to the right are recipes that produce JSON schema definitions that go along with the data files.

Amazon Personalize will use this data and schema to train a recommender system to deliver personalized movie recommendations

Now let's look at how to build this flow.

### Show flow: Personalize (starting point)

We begin by creating a flow and adding datasets to it. You can do so by click on "Add Datasets" on the top right.

In this case, we have added movies.csv, which is stored in this location on Amazon S3. You can quickly see a preview of each dataset before you begin working with them

I will now edit the recipe attached to the movies dataset. This shows a familiar spreadsheet-like view of the data that will let us work through the data preparation steps

Looking at the movie titles, I notice that some of them have extra quotation marks. I can simply use my mouse to select this, and Trifacta offers a suggestion to replace values and remove the quotes. It also shows a preview of the result in this yellow column. These suggestions are driven by machine learning, and I can pick and customize any one of them

Now this MovieLens data has relatively few attributes for each movie. We want to enrich this data by bringing in additional fields from IMDB. To do this, I select Join from the toolbar, and choose links. This table contains an association between MovieLens movieId and imdbId.

Notice that Trifacta has automatically detected the join key. I can easily select the columns I want. In this case we want to include the imdbId column, and add it to the recipe

We now have imdbId in our dataset. However, there is a small problem. We know that our IMDB dataset has a prefix of 'tt' before the Id, that we are lacking. So we need to edit the Id to include the prefix and make the join work

With the correct key, we can now join to our main IMDB dataset. We will perform a Left join based on the key we prepared before. We'll select several attributes from the IMDB table, including titleType, primaryTitle, startYear, runtimeMinutes and genres. These attributes will be used by Amazon Personalize to train the recommender system

Notice that we see a bunch of nulls for the new columns. This is because the IMDB dataset is huge, and our initial sample did not contain matching records for the IDs displayed here. We can easily fix this by drawing a new sample

The sample is now ready, so I will switch to it. This fills in the remaining columns

I will now perform some minor updates to the genre column, and change the delimiter from comma to the pipe symbol

And the final thing I'll do on the movies dataset is to rename the movieId to ITEM_ID. This is the format required by Amazon Personalize

At this point, we are done with the Item list, so I will switch to the ratings table, which represents Interactions

The ratings table is missing column headings, so I'll add a rename step. I can rename multiple columns at the same time. I will match the column names expected by Amazon Personalize and set them to USER_ID, ITEM_ID, RATING and TIMESTAMP

Now we only want to recommend movies that have a high rating, so we'll add a filter condition. We will choose records where ratings is >= 3.6

And finally for this dataset, we will change the data types on a couple of these columns to Strings, to match what is expected by Amazon Personalize

Our movies and ratings data is now ready. The next step is to generate schemas definitions for them. 

I have pre-created the recipes that generate a schema file in the format expected by Amazon personalize. I will now attach these schema recipes to the respective datasets

The items schema is attached to the movies dataset

The interactions schema is attached to the ratings dataset

The flow is now ready. The final step is to define the outputs that we want, and run jobs to produce these outputs

Our outputs will be stored in an S3 bucket that is accessible to Amazon Personalize

We will persist the data as CSV files, and the schemas as JSON files

Choose Run Job to produce the outputs, for each of the outputs.

You can access the job results and view a detailed profile for any of the outputs


You can now access Amazon Personalize from the AWS console, and create a dataset group and dataset that points to the S3 bucket and files that were outputted by Trifacta

Based on these datasets, you create solution versions, which are trained models, and Campaigns which are deployed applications that you can use to generate recommendations

This is the Item data that is consumed by Amazon Personalize. It consists of an ITEM_ID plus up to six categorical variables

The item data is accompanied by a JSON schema that describes the names, nature and data type for each column

Similarly, the Interactions dataset has the data outputted by trifacta containing USER_ID, ITEM_ID and TIMESTAMP, and a JSON schema definition that describes their respective types

Ultimately, investing in data preparation with Trifacta pays off in the form of better recommendations from Amazon Personalize. You can see from the model metrics that recommender system delivers higher scores due to enrichment and improved data quality.

And that's it! I hope you found this demonstration informative and useful. All of the datasets, flows and other demo materials are included in a Github repository you can access in the description below. I highly encourage you to try it out yourself. Thank you!
