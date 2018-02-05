---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:caption: .caption}
{:pre: .pre}

# Coding Guidelines for Deep Learning Programs

In general you should be able to easily run your existing Deep Learning programs using the service.  There are a few caveats including how your program will read data and save a trained model, but the modifications required should be relatively straightforwards.

## Obtaining the input data

Input data files located in the specified COS bucket are available to your program via the folder specified in environment variable `DATA_DIR`

```
# if your code needs to open the file imagedata.csv
# from within your training_data_reference bucket
# use the following approach

input_data_folder = os.environ["DATA_DIR"]

imagefile = open(os.path.join(image_data_folder,"imagedata.csv"))
```
{: codeblock}


## Writing the trained model

Output your trained model to a file or folder named `model` under the folder specified in the environment variable `RESULT_DIR`.  When you store a training-run into the repository, this `model` file or folder is saved.

```
output_model_folder = os.environ["RESULT_DIR"]
output_model_path = os.path.join(output_model_folder,"model")

mymodel = train_model()
mymodel.save(output_model_path)
```
{: codeblock}

For guidelines on how to save a deep learning models in a format that is compatible with the deployment and scoring service please refer to [Deploying and scoring a deep learning model](ml_dlaas_model_deploy_score.html)

## Writing to the log

Just write to `stdout` (for example in python, use the `print` function) and the output will be collected by the service and made available when you monitor a running job or obtain the resulting log.

```
print("starting to train model...")
mymodel = train_model()
print("model training completed..., starting evaluation")
evaluate_model(mymodel)
print("model evaluation completed.")
```
{: codeblock}

## Reading hyper-parameters

If your code is running as part of a larger experiment it may need to obtain values for hyper-parameters defined in the experiment.  The hyper-parameters will be supplied in a file called `config.json` as a JSON formatted dictionary, located in the current folder and can be read using the following example snippet (which expects hyper-parameters to be defined for `initial_learning_rate` and `total_iterations`:

```
hyper_params = json.loads(open("config.json").read())
learning_rate = float(hyper_params["initial_learning_rate"])
training_iters = int(hyper_params["total_iterations"])
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


for iteration in range(0,total_iterations):
    # perform training for iteration
    # compute test metrics into test_summary for iteration and write them to the tensorboard log
    test_writer.add_summary(test_summary, iteration)
```
{: codeblock}

