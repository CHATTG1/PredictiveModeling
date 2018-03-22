---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-21"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:caption: .caption}
{:pre: .pre}

# Coding Guidelines for Deep Learning Programs

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_code_guidelines.html). Check there for the most up-to-date information.**

Update any bookmarks you might have to the old location.


_____________

In general you should be able to easily run your existing Deep Learning programs using the service.  There are a few caveats including how your program will read data and save a trained model, but the modifications required should be relatively straightforwards.

## Obtaining the input data

Input data files located in the specified COS bucket are available to your program via the folder specified in environment variable `DATA_DIR`

```
# if your code needs to open the file imagedata.csv
# from within your training_data_reference bucket
# use the following approach

input_data_folder = os.environ["DATA_DIR"]

imagefile = open(os.path.join(input_data_folder,"imagedata.csv"))
```
{: codeblock}


## Writing the trained model

Output your trained model to a sub-folder named `model` under the folder specified in the environment variable `RESULT_DIR`.  You may need to create the `model` folder depending on how the deep learning framework save mechanism works...  When you store a training-run into the repository, files within this `model` sub-folder are collected and saved to the repository.

```
results_folder = os.environ["RESULT_DIR"]
output_model_subfolder = os.path.join(results_folder,"model")

my_tensorflow_model = ... # obtain the trained model
my_tensorflow_model.save(output_model_subfolder) # creates the model folder and saves into that folder
```
{: codeblock}

For guidelines on how to save a deep learning models in a format that is compatible with the deployment and scoring service please refer to the following documents:
* [Deploying and scoring a deep learning TensorFlow model](ml_dlaas_tensorflow_model_deploy_score.html)
* [Deploying and scoring a deep learning Keras model](ml_dlaas_keras_model_deploy_score.html)
* [Deploying and scoring a deep learning Caffe model](ml_dlaas_caffe_model_deploy_score.html)

## Writing to the log

Just write to `stdout` (for example in python, use the `print` function) and the output will be collected by the service and made available when you monitor a running job or obtain the resulting log.  Your program should not write any personally identifiable information (PII).

```
print("starting to train model...")
mymodel = train_model()
print("model training completed..., starting evaluation")
evaluate_model(mymodel)
print("model evaluation completed.")
```
{: codeblock}


## Computing and sending metrics

You can generate metrics of interest using techniques based on the framework being used...  you can of course simply print metric values in an unstructured way to the logs but you can also send metrics to the service in a structured way, so that the metrics can be extracted and monitored.  Currently this feature is supported for `tensorflow` and other frameworks will be supported soon.

## Computing and sending metrics for tensorflow

For the `tensorflow` framework, you can use the tensorboard style summary metrics, with the following configuration, where the tensorboard summaries are written to a particular folder in the file system.

In your tensorflow python code, first configure the location where the metrics will be written, and create the folder as follows:

```
tb_directory = os.environ["JOB_STATE_DIR"]+"/logs/tb"
tensorflow.gfile.MakeDirs(tb_directory)
```
{: codeblock}

Now in your code you can compute metrics and write them to an appropriately named sub-folder, for example to create a writer for test metrics, see the following snippet:

```
test_writer = tf.summary.FileWriter(tb_directory+'/test')


for epoch in range(0,total_epochs):
    # perform training for epoch
    # compute test metrics into test_summary for epoch and write them to the tensorboard log
    test_writer.add_summary(test_summary, epoch)
```
{: codeblock}

## Computing and sending metrics for keras

Coming soon

## Computing and sending metrics for pytorch

For the `pytorch` framework please use the following logging approach based on the python code in [emetrics.py](https://github.com/pmservice/wml-sample-models/raw/master/deep-learning/metrics/emetrics.py).  You can download this python file and add it to your training definition zip, then import it and use it in your program.

```
from emetrics import EMetrics

with EMetrics.open() as em:
	for epoch in range(0,total_epochs):
    	# perform training for epoch
    	# compute training metrics and assign to values train_metric1, train_metric2 etc
    	em.record("training",epoch,{'metric1': train_metric1, 'metric2': train_metric2})
    	
    	# compute test metrics and assign to values test_metric1, test_metric2 etc
    	# NOTE for these use the group EMetrics.TEST_GROUP so that the service will recognize these metrics as computed on test/validation data
    	em.record(EMetrics.TEST_GROUP,epoch,{'metric1': test_metric1, 'metric2': test_metric2})
```
{: codeblock}


