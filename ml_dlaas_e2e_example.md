---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:caption: .caption}
{:pre: .pre}

# End to end example for running a DL training

This document assumes that you have created an account and have uploaded your training data to IBM COS. Also you have some familiarity with the basic concepts and know how to execute a training. The example below walks through some of the commonly used concepts in training your model and saving your model for scoring via IBM WML

## Concepts
The goal of IBM WML is to basically allow you to run a training that you were able to run on your machine and be able to run the exact same training on IBM WML with little to no changes. The intent is to allow you to use more powerful hardware without the need to make changes in your code. 

Every training via IBM WML basically takes in a definition file that describes what framework/version/compute configuration needs to be used, where the data/results are supposed to be located and what command needs to be executed to process this data and get the results. A example of a basic definition is:

``` yml
model_definition:
  name: tf-mnist
  author:
    name: WML User
    email: wmluser@ibm.com
  description: Simple MNIST model implemented in TF
  framework:
    name: tensorflow
    version: "1.5"
    runtimes:
    - name: python
      version: "3.5"
  execution:
    command: python3 mnist_e2e_example.py --MAX_STEPS=1000
    compute_configuration:
      name: k80
training_data_reference:
  name: MNIST image data files
  connection:
    endpoint_url: <auth-url>
    access_key_id: <username>
    secret_access_key: <password>
  source:
    bucket: mnist-training-data
  type: s3
training_results_reference:
  name: DL Model Storage
  connection:
    endpoint_url: <auth-url>
    access_key_id: <username>
    secret_access_key: <password>
  target:
    bucket: mnist-training-models
  type: s3
```

The above definition is responsible for carrying out a simple mnist training using tensorflow 1.5 as mentioned in the framework name and version section above. The command executed by this training runs the command `python3 mnist_e2e_example.py --MAX_STEPS=1000` . We will be running this training on a single `K80` GPU as mentioned in the compute configuration section. Running this on a more powerful hardware is as simple as change the values in the compute configuration.

The `training_data_reference` section provides information about what bucket in IBM COS has the mnist training data, and the `training_results_reference` provides information on how to connect to result bucket where the results of this training will be stored.

Here's a sample `mnist_e2e_example.py` that can be used for training:

```python
"""
end to end mnist example demonstrating all the concepts of a single learner training using tensorflow
"""
import os, sys, time
import tensorflow as tf
tf.logging.set_verbosity(tf.logging.INFO)

learn = tf.contrib.learn

flags = tf.app.flags
flags.DEFINE_integer("MAX_STEPS", 200, "maximum number of steps to go through")
FLAGS = tf.app.flags.FLAGS

data_dir = os.getenv("DATA_DIR") #this is a reference to your training data bucket
result_dir = os.getenv("RESULT_DIR") #this is a reference to your current training_id/ folder under result bucket
base_checkpoint_dir = os.getenv("CHECKPOINT_DIR") #this is a reference to _wml_checkpoints/ folder under your result bucket 
log_dir = os.getenv("LOG_DIR") # this is a reference to log dir that gets copied over to your result bucket at the end of training

model_path = os.path.join(result_dir, "model/") #where you want your models to be stored, your result bucket/training_id/model/
data_dir_mnist = os.path.join(data_dir, "mnist/") #location of our mnist dataset
checkpoint_path = os.path.join(base_checkpoint_dir, "mnist_experiment_1/") 
tboard_summaries = os.path.join(log_dir, "tboard/") # logging all the tensorboad metrics here

def main(_):
   
   mnist = learn.datasets.mnist.read_data_sets(data_dir_mnist, one_hot=True)

   x = tf.placeholder(tf.float32, [None,784], name="x_input")
   W = tf.Variable(tf.zeros([784,10]))
   b = tf.Variable(tf.zeros([10]))
   y = tf.placeholder(tf.float32, [None,10])
   model = tf.matmul(x, W) + b

   cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(labels=y, logits=model))
   global_step = tf.train.get_or_create_global_step()
   train_op = tf.train.GradientDescentOptimizer(0.5).minimize(cost, global_step=global_step)
   prediction = tf.equal(tf.argmax(model,1), tf.argmax(y,1))
   accuracy = tf.reduce_mean(tf.cast(prediction, tf.float32))

   hooks = [tf.train.StopAtStepHook(last_step=FLAGS.MAX_STEPS)] #ask user for how many steps need to be executed

   #tensorboard
   writer = tf.summary.FileWriter(tboard_summaries)

   saver = tf.train.Saver()
   #store all your checkpoints under result bucket/_wml_checkpoints/mnist_experiment_1/ path
   #if your training crashes, you can resume from the same path
   with tf.train.MonitoredTrainingSession(checkpoint_dir=checkpoint_path, hooks=hooks) as sess:
      writer.add_graph(sess.graph) 
      ckpt = tf.train.get_checkpoint_state(checkpoint_path)
      if ckpt and ckpt.model_checkpoint_path: #if existing checkpoint available then start from there
        # Restores from checkpoint
        print("restoring from path {} with checkpoint {}".format(ckpt.model_checkpoint_path, ckpt))
        saver.restore(sess, ckpt.model_checkpoint_path)

      # loop through data batches
      while not sess.should_stop():
         batch_xs, batch_ys = mnist.train.next_batch(1000)
         _, step = sess.run([train_op, global_step], feed_dict={x: batch_xs, y: batch_ys})
         if (step % 10 == 0) and (not sess.should_stop()):
            loss, acc = sess.run([cost,accuracy], feed_dict={x: mnist.validation.images, y: mnist.validation.labels})
            print("{:4d}".format(step) + ": " + "{:.6f}".format(loss) + ", accuracy=" + "{:.5f}".format(acc))
            sys.stdout.flush()

   # saving the model and preparing it for inferencing later
   latest_checkpoint = tf.train.latest_checkpoint(checkpoint_path)
   print(latest_checkpoint)
   sys.stdout.flush()

   with tf.Session() as sess:
      saver.restore(sess,latest_checkpoint) # restore the latest checkpoint from checkpoints folder
      acc = sess.run(accuracy, feed_dict={x: mnist.test.images,y: mnist.test.labels});
      print("Test accuracy = "+"{:5f}".format(acc))
      # output model 
      predictor = tf.argmax(model, 1, name="predictor")
      inputs_classes = tf.saved_model.utils.build_tensor_info(x)  # input an image
      outputs_classes = tf.saved_model.utils.build_tensor_info(predictor)  # output its class (0-9)
      signature = (tf.saved_model.signature_def_utils.build_signature_def(inputs={tf.saved_model.signature_constants.CLASSIFY_INPUTS:inputs_classes},outputs={tf.saved_model.signature_constants.CLASSIFY_OUTPUT_CLASSES:outputs_classes},method_name=tf.saved_model.signature_constants.CLASSIFY_METHOD_NAME))
      builder = tf.saved_model.builder.SavedModelBuilder(model_path)  # where to store the model
      legacy_init_op = tf.group(tf.tables_initializer(), name='legacy_init_op')
      builder.add_meta_graph_and_variables(sess,[tf.saved_model.tag_constants.SERVING],signature_def_map={'predict_images': signature},legacy_init_op=legacy_init_op)
      save_path = builder.save()
      print("Model saved in file: %s" % save_path.decode("utf-8"))

if __name__ == "__main__":
   tf.app.run()

```

There are a few concepts worth noting here. There are a handful of environment variables available in the environment in which the training is executed:

- **DATA_DIR**: this is a reference to your training data bucket mounted as a folder. in the above example we use the `$DATA_DIR/mnist` which translates to `training data bucket/mnist` where we have our mnist data
- **RESULT_DIR**: this is a reference to your current training_id/ folder under result bucket. this is unique per training. Use this to save your trained model. in the above example we write the model to `$RESULT_DIR/model` which translated to `result bucket/<training id>/model`
- **CHECKPOINT_DIR**: this is a reference to _wml_checkpoints/ folder under your result bucket. you can append to this path and save your checkpoints here, and resume from here in case the training fails. in the above example we write checkpoints under the path `$CHECKPOINT_DIR/mnist_experiment_1` which translates to `result bucket/_wml_checkpoints/mnist_experiment_1`
- **LOG_DIR**: this is a reference to logs directory. It is recommended to write your logs and summary metrics for tensorboard under this directory. Currently, this directory is synched to your $RESULT_DIR making logs and tensorboard summary metrics available to you.

## Checkpointing and resuming from checkpoint

You can checkpoint and resume training using a sample code snippet below. For example, if we change the above command `python3 mnist_e2e_example.py --MAX_STEPS=1000` to have `MAX_STEPS=2000` then the training will resume from the checkpoint created as a part of the 1000th step and not from the beginning. This is a powerful concept that you can leverage to account for any failures in your training or in the underlying infrastructure to make sure that your progress is saved and you can resume from where you left.

```python
 ckpt = tf.train.get_checkpoint_state(checkpoint_path)
      if ckpt and ckpt.model_checkpoint_path: #if existing checkpoint available then start from there
        # Restores from checkpoint
        print("restoring from path {} with checkpoint {}".format(ckpt.model_checkpoint_path, ckpt))
        saver.restore(sess, ckpt.model_checkpoint_path)
```

## Saving model

In order to save a model to score it or use it for inferencing you can use code similar to mentioned below. Notice the user of `$RESULT_DIR` here in order to save your trained model.

```python
with tf.Session() as sess:
      saver.restore(sess,latest_checkpoint) # restore the latest checkpoint from checkpoints folder
      acc = sess.run(accuracy, feed_dict={x: mnist.test.images,y: mnist.test.labels});
      print("Test accuracy = "+"{:5f}".format(acc))
      # output model 
      predictor = tf.argmax(model, 1, name="predictor")
      inputs_classes = tf.saved_model.utils.build_tensor_info(x)  # input an image
      outputs_classes = tf.saved_model.utils.build_tensor_info(predictor)  # output its class (0-9)
      signature = (tf.saved_model.signature_def_utils.build_signature_def(inputs={tf.saved_model.signature_constants.CLASSIFY_INPUTS:inputs_classes},outputs={tf.saved_model.signature_constants.CLASSIFY_OUTPUT_CLASSES:outputs_classes},method_name=tf.saved_model.signature_constants.CLASSIFY_METHOD_NAME))
      builder = tf.saved_model.builder.SavedModelBuilder(model_path)  # where to store the model
      legacy_init_op = tf.group(tf.tables_initializer(), name='legacy_init_op')
      builder.add_meta_graph_and_variables(sess,[tf.saved_model.tag_constants.SERVING],signature_def_map={'predict_images': signature},legacy_init_op=legacy_init_op)
      save_path = builder.save()
      print("Model saved in file: %s" % save_path.decode("utf-8"))
```

## Tensorboard

Summary metric writers intended to be consumed by Tensorboard may be constructed by passing a directory under the $LOG_DIR environment variable. These will create event files that will be copied to the Cloud Object Store result directory at the end of the training.

```# Construct summary metrics writer for Tensorboard consumption, and also enables WML monitoring
writer = tf.summary.FileWriter(os.path.join(os.getenv("LOG_DIR"), "tboard"))```

After the training is done, you may download that directory, and then point your local Tensorboard server to that that directory. For live monitoring the scaler metrics from your summary events will be available via WML monitoring.


## Conclusion

The above example demonstrates how you can basically take an example of from Tensorflow tutorial and with little to no changes in the code run that same example in IBM WML.

## Related

- Try out [IBM DDL] and leverage capabilities of distributed learning with minimal code changes

- Try IBM WML [scoring] capabilities
