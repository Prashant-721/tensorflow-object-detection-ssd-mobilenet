#Military Object Real-Time Detection for Smart Surveillance


## Brief Summary

* Real-time object detection is a vast, vibrant yet inconclusive and complex area of computer vision. As the object detection is closely associated with video analysis and image understanding, it has attracted much research attention in recent years. In this paper, we provide a review on military object detection with the goal of achieving high accuracy in a real-time performance. This system of object detection is used to achieve rapid and accurate detection of military objects on the vast and complex battlefield, which is inspired by the process of human visual information processing. The main objective of this research is to assist the human operators, by implementing intelligent visual surveillance systems which help in detecting intrusions. This system is eminently utilized in detecting objects which poses a primary security threat across border.This system works as automatic detection, recognition of intrusion which is very helpful for military application. The very tight security and safety can be assured without endangering precious life of human solider. Our system ensures low execution time, simplicity and also ensures low implementation cost.

## Steps
### 1.Gather Pictures ( Collection of dataset)
TensorFlow needs hundreds of images of an object to train a good detection classifier.
We use “Fatkun Batch Download Image” to download google images
<p align="center">
  <img src="C:/Users/Dell/Pictures/Screenshots/Screenshot (179).png">
</p>

### 2.Normalization of dataset
Normalization done to m ake sure the images aren’t too large. They should be less than 200KB each, and their resolution shouldn’t be more than 800x600
<p align="center">
  <img src="C:/Users/Dell/Desktop/project/screenshot/transform_image_resolution.png">
</p>

### 3.Label Pictures
With all the pictures gathered, it’s time to label the desired objects in every picture. LabelImg is a great tool for labeling images.
<p align="center">
  <img src="C:/Users/Dell/Pictures/Screenshots/Screenshot (80).png">
</p>

### 4.Configure TensorFlow Directory, Environments, model etc.
The TensorFlow Object Detection API requires using the specific directory structure provided in its GitHub repository. It also requires several additional Python packages, specific additions to the PATH and PYTHONPATH variables, and a few extra setup commands to get everything set up to run or train an object detection model.

### 5.Run the training
You can view the progress of the training job by using TensorBoard.
The TensorBoard page provides information and graphs that show how the training is progressing. One important graph is the Loss graph, which shows the overall loss of the classifier over time.
<p align="center">
  <img src="C:/Users/Dell/Pictures/Screenshots/Screenshot (131).png">
</p>
<p align="center">
  <img src="C:/Users/Dell/Pictures/Screenshots/Screenshot (127).png">
</p>

### 6.Use Your Newly Trained Object Detection Classifier!
The object detection classifier is all ready to go!
<p align="center">
  <img src="C:/Users/Dell/Pictures/Screenshots/Screenshot (174).png">
</p>
<p align="center">
  <img src="C:/Users/Dell/Pictures/Screenshots/Screenshot (176).png">
</p>

## Appendix: Common Errors
It appears that the TensorFlow Object Detection API was developed on a Linux-based operating system, and most of the directions given by the documentation are for a Linux OS. Trying to get a Linux-developed software library to work on Windows can be challenging. There are many little snags that I ran in to while trying to set up tensorflow-gpu to train an object detection classifier on Windows 10. This Appendix is a list of errors I ran in to, and their resolutions.

#### 1. ModuleNotFoundError: No module named 'deployment' or No module named 'nets'

This error occurs when you try to run object_detection_tutorial.ipynb or train.py and you don’t have the PATH and PYTHONPATH environment variables set up correctly. Exit the virtual environment by closing and re-opening the Anaconda Prompt window. Then, issue “activate tensorflow1” to re-enter the environment, and then issue the commands given in Step 2e.

You can use “echo %PATH%” and “echo %PYTHONPATH%” to check the environment variables and make sure they are set up correctly.

Also, make sure you have run these commands from the \models\research directory:
```
setup.py build
setup.py install
```

#### 2. ImportError: cannot import name 'preprocessor_pb2'

#### ImportError: cannot import name 'string_int_label_map_pb2'

#### (or similar errors with other pb2 files)

This occurs when the protobuf files (in this case, preprocessor.proto) have not been compiled. Re-run the protoc command given in Step 2f. Check the \object_detection\protos folder to make sure there is a name_pb2.py file for every name.proto file.

#### 3. object_detection/protos/.proto: No such file or directory

This occurs when you try to run the
```
“protoc object_detection/protos/*.proto --python_out=.”
```
command given on the TensorFlow Object Detection API installation page. Sorry, it doesn’t work on Windows! Copy and paste the full command given in Step 2f instead. There’s probably a more graceful way to do it, but I don’t know what it is.

#### 4. Unsuccessful TensorSliceReader constructor: Failed to get "file path" … The filename, directory name, or volume label syntax is incorrect.
 
This error occurs when the filepaths in the training configuration file (faster_rcnn_inception_v2_pets.config or similar) have not been entered with backslashes instead of forward slashes. Open the .config file and make sure all file paths are given in the following format:
```
“C:/path/to/model.file”
```

#### 5. ValueError: Tried to convert 't' to a tensor and failed. Error: Argument must be a dense tensor: range(0, 3) - got shape [3], but wanted [].

The issue is with models/research/object_detection/utils/learning_schedules.py Currently it is
```
rate_index = tf.reduce_max(tf.where(tf.greater_equal(global_step, boundaries),
                                      range(num_boundaries),
                                      [0] * num_boundaries))
```
Wrap list() around the range() like this:

```
rate_index = tf.reduce_max(tf.where(tf.greater_equal(global_step, boundaries),
                                     list(range(num_boundaries)),
                                      [0] * num_boundaries))
```

[Ref: Tensorflow Issue#3705](https://github.com/tensorflow/models/issues/3705#issuecomment-375563179)

#### 6. ImportError: DLL load failed: The specified procedure could not be found.   (or other DLL-related errors)
This error occurs because the CUDA and cuDNN versions you have installed are not compatible with the version of TensorFlow you are using. The easiest way to resolve this error is to use Anaconda's cudatoolkit package rather than manually installing CUDA and cuDNN. If you ran into these errors, try creating a new Anaconda virtual environment:
```
conda create -n tensorflow2 pip python=3.5
```
Then, once inside the environment, install TensorFlow using CONDA rather than PIP:
```
conda install tensorflow-gpu
```
Then restart this guide from Step 2 (but you can skip the part where you install TensorFlow in Step 2d).
