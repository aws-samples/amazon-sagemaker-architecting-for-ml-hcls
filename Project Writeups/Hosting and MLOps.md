# Hosting Models on SageMaker for Rapid Diagnosis
You've just joined the machine learning team at a local hospital. They have completed a POC on diagnosing breast cancer, and need your help hosting this model on SageMaker.

Not only does the team want to build a RESTful API, they want to take advantage of the massive array of features and capabilities that SageMaker brings to the table.

In this module you will:
- host your model on SageMaker
- enable and test autoscaling on your endpoint 
- monitor your model
- push a new model into production on that endpoint
- build an automatic retraining system with Lambda

## 1 & 2. Access Your Model Artifact, Training and Inference Code. Package in the SageMaker SDK
The good news is that your model is already built! You have a pretrained model artifact, defined in SKLearn. This model looks at tabular data and returns a predicted likelihood that the patient has breast cancer. You also have the exact training and inference code necessary to develop this model, written as a Python script. 

Access these artifacts and package them up within the SageMaker SDK following the example here:
- https://github.com/EmilyWebber/architecting-for-ml-in-hcls/blob/main/Starter%20Notebooks/MLOps%20and%20Hosting/Hosting%20Models%20on%20SageMaker.ipynb

## 3 & 4. Create and Endpoint and Enable Autoscaling
Once you have the artifacts packaed within the SageMaker SDK, creating an endpoint should be as simple as calling `model.deploy()`. Follow the notebook for this.

After this is completed, you'll use boto3 to enable autoscaling on that endpoint. Notice that ths is also a single function call, albeit with a few more parameters.

## 5. Enable Model Monitor on your Endpoint
The real-world environment that our data scientists trained their model against actually changes over time - we need to make sure that the trained model we are using stays up to date with those changes. The way we're going to do that on SageMaker is by _setting up model monitor on our endpoints._ The way this happens is actually through passing up our training data. We'll use the SageMaker SDK to spin up a processing job that takes our training data in S3, and learns statistical benchmarks on our data. 

The built-in processing image for model monitor uses _Amazon Deequ_, an open-source solution that ensures data quality at high volumes. It's written in PySpark, so it scales quite well. After your processing job has finished you're welcome to view the thresholds and modify them as you or your data scientists prefer. 
- https://github.com/awslabs/deequ

You'll also specify a percentage of data capture, that is the amount of your traffic you want to store after it hits your endpoint. Then, you'll set up _a monitoring schedule_ to run monitoring jobs which use the thresholds you learnt during the previous step, and simply apply those to the data captured from your endpoint.

## 6. Improve your model with AutoGluon
You might find that the quality of your model drops over time, or possibly wasn't even that great to being with. AutoGluon is an easy way to improve this - it supports tabular data, imaging, and natural language processing. AutoGluon also has data augmentation capabilities, so it works with smaller datasets like we have here. Step six entails runnning an AutoGluon job, using script mode in SageMaker, to find a better version of the model.

## 7. Tune your model and redploy
Another way of finding a better model is using the automatic model tuner. Sadly that doesn't improve the performance of our model in this case, unlike AutoGluon which brings us up to 95% accuracy, but so you can see how it's done we've included the code for both runing a hyperparameter tuning job and a re-deploy to the same endpoint.

## 8. Automate the workflow
For the sake of time we've included a very simple of way of automating this workflow - setting up the local `notebook-runner` toolkit and simply running the entire notebook on it's own processing job. While you might not use this instead of a true MLOps pipeline for a real-time application, you can certainly get a lot of value out of running notebooks automatically and with a CLI. See the blog post and GitHub code suit for more details here: 
- https://aws.amazon.com/blogs/machine-learning/scheduling-jupyter-notebooks-on-sagemaker-ephemeral-instances/

## Extensions
If you have spare time, you're welcome to add the AutoGluon deploy capabilities as referenced in this example notebook:
- https://github.com/aws/amazon-sagemaker-examples/tree/master/advanced_functionality/autogluon-tabular 

You can also step through setting up an MLOps pipeline using Lambda, Step Functions, and Apache AirFlow as referenced here: 
- https://github.com/aws-samples/mlops-amazon-sagemaker-devops-with-ml