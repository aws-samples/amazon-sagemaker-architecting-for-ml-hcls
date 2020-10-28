# Advanced SageMaker Features for Data Science in HCLS
You've seen the lectures, you've watched the videos, you've stepped through a hands on lab. Now it's time to take your usage of SageMaker to the next level by taking advantage of some advanced SageMaker features for training.

In particular, in this project you will train your own object detection model using the built-in Object Detection classifier. This is actually picking up from the results of a Ground Truth data labelling job, and using a manifest file to train on 1000 images. You'll train on spot instances using a sizeable GPU. You will also connect this job to a larger experiment to manage and view your progress. You'll learn how to convert your png images to RecordIO for optimized throughput.

Optionally, if you have extra time, you're welcome to bring your own single shot detection algorithim into script mode for SageMaker. If you're able to do that, you can leverage SageMaker debugger to analyze the gradients of your model, and even produce an interactive TensorPlot! 

# Built-in Object Detection for X-Ray Analysis
In 2017 the National Institute of Health introduced to the public scientific community one of the largest datasets of chest XRays previously available. Clocking in at 112,000 images, this dataset is the result of more than 30,000 patients. It is labelled to identity potentially cancerous masses within the images, helping acceelerate both scientific analysis and positive patient outcomes through faster diagnosis.

In this project you'll leverage a subset of the NIH dataset, specifically a sample of 1000 images that we have previously tested with SageMaker's data labelling solution, Ground Truth. You will use the output of a Ground Truth labelling job, where a labeller manually drew bounding boxes around the neck and/or trachea from these images. We're going to use the output from that job as the input for an Object Detection training job.

## Accessing the Data and Preparing the Training Job
To access the data and get started on your project, please navigate to the following link:

- https://github.com/aws-samples/amazon-sagemaker-architecting-for-ml-hcls/blob/main/Starter%20Notebooks/Advanced%20Data%20Science%20-%20XRay%20Analysis/Computer%20Vision%20for%20XRay%20Analysis.ipynb

This will take you all the way to training an Object Detection algorithm using the built-in image. This job should take about 35 minutes to run, on the GPU's provided. Notice that your job is connected to SageMaker experiments, so you should be able to view the results in the Experiments tab.

Also notice that you are training on SageMaker spot! Go ahead and tell us how much you saved on that job. 

## Extending with bringing your own model
If you have time, you are welcome to extend the solution by bringing your own single-shot-detection algorithm. You can use the script we provided to convert your images to RecordIO if you prefer.

The advantage of bringing your own model in this case is that you'll be able to use SageMaker Debugger. SageMaker Debugger works out of the box with script-mode for TensorFlow, PyTorch and MXNet models. What this means is that you can setup a _debug hook config_, which will listen to the tensors recorded during your training job and let you analyze these. On the one hand, you can use a built-in debugger image which applies up to 18 built-in rules on your tensors, covering things like whether or not your loss is decreasing, if your gradients are vanishing or exploring, and even analyze feature important.

On the other hand, you can download the tensors produced during your job and _plug these into a local visualization solution._ We've included some starter code for doing this with our provided `TensorPlot` framework, which will let you develop a local interactive image for assessing your model.

SageMaker Debugger has exmaples for class activation maps, tensor plots, BERT attention head visualizations, and even model pruning. You're welcome to step through the Debugger examples listed below, play with these, and connect them to the NIH or other dataset as you prefer. 

- https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-debugger