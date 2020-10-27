# Predicting Hospital Costs Per Patient with SageMaker Autopilot

### Your Problem
Medicare is a national health insurance program, administered by the Center for Medicare and Medicaid Services (CMS). This is a primary health insurance program for Americans who are aged 65 and older. Medicare has published historical data showing hospital’s average spending for Medicare Part A and Part B claims based on different claim types and claim periods covering 1 to 3 days prior to hospital admission up to 30 days after discharge from hospital admission. These hospital spending are price standardized and non-risk adjusted, since risk adjustment is done at the episode level of the claims spanning the entire period during the episode. The hospital average costs are listed against the corresponding state level average cost and national level average cost.

You have just joined the data science team at Well-Forecasted Hospital. Your goal is to use the Medicare historical spending data to estimate potential costs per patient.

### Your Dataset
Medicare has published dataset showing average hospital spending on Medicare Part A and Part B claims. Both the links below refer to the same data set, one is listed in the healthdata.gov site and the other is listed at the data.medicare.gov site. The data dictionary is described in the link marked as #2 below. The dataset has hospital spending data from the year 2018 and has 67,826 data rows spanning across 13 columns. For the purposes of our analysis and machine learning, we use the dataset in csv (Comma Separated Values) format.
1.	https://healthdata.gov/dataset/medicare-hospital-spending-claim
2.	https://data.medicare.gov/Hospital-Compare/Medicare-Hospital-Spending-by-Claim/nrth-mfg3

A direct link to download the dataset to local computer can be accessed at this link - https://data.medicare.gov/api/views/nrth-mfg3/rows.csv?accessType=DOWNLOAD

### Accessing and cleaning your data
To make this easier for you, we've written a starter notebook that downloads the data for you and performs some basic manipulations.

See the link to the starter notebook here:
- https://github.com/EmilyWebber/architecting-for-ml-in-hcls/blob/main/Starter%20Notebooks/Cost%20Prediction/Cost%20Prediction%20with%20Autopilot.ipynb

### Analyzing your data and performing feature engineering
Once you've loaded the cleaned data into your pandas dataframe, spend a bit of time exploring the fields with generating some plots and histograms. We started with using `pandas.plotting.scatter_matrix` and looking at the claims field.

We also found it helpful to use `sklearn.feature_selection.SelectKBest` based on `sklearn.feature_selection.chi2` to identify the best candidate X features.

Feel free to experiment here with your favorite feature engineering and data anlysis steps.

### Train a model using your X and Y variables with SageMaker Autopilot
We recommend using SageMaker Autopilot as a simple way to automate both data analysis and model tuning. In the notebook provided, you'll be able to easily train your own set of 250 models using the proivded datset and wrapping Python code to leverage the Autopilot API. 

The Autopilot job will take quite a bit of time to run. If you are using SageMaker Studio you should be able to monitor the job via the Experiments tab. Once the job moves into "Feature Engineering," you should be able to open up the both the data transformation and candidate generation notebooks. Do that. Open themn up on your local Studio domain, step through them, and try to understand precisely what they are doing for you. Remember, all of this code is generated for your specific dataset! 

### Deploy your solution to a RESTful API
When you have time, you'll notice that the built-in models and the script-mode managed containers within SageMaker come with a `model.deploy()` method. This will automatically create a RESTful API hosting your model! Give that a try.

What's nice about Autopilot is that __it will deploy the featurizing code along with the best model.__ That is to say, for all of the data transformation code that Autopilot generates within the candidates, it will deploy the specific featurizing code that maps to your best candidate. This is wrapped inside of a what SageMaker calls __an inference pipeline__.

### Extend with bringing your own feature engineering script into SageMaker Autopilot
You may remember that this project opened up with some basic data transformation before we even plugged it into Autopilot. If you have time, your task is to port that code into the _bring your own feature engineering_ capabilities of Autopilot.

Step through the example right here, then modify it to point to your dataset and the code from the starter notebook.
- https://github.com/aws/amazon-sagemaker-examples/blob/master/autopilot/custom-feature-selection/Feature_selection_autopilot.ipynb