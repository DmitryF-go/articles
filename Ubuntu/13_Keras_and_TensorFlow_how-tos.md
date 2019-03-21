How-to:
   - [Additional useful tools](#tools)
   - [Exercises and tutorials](#exercises)
   - [Limit TensorFlow to one GPU](#limit)

---
### <a name="tools" />Additional useful tools

Additional useful tools everybody should know about:

   - [Fatkun Batch Download Image add-on for Chrome browser](https://chrome.google.com/webstore/detail/fatkun-batch-download-ima/nnjjahlikiabnchcpehcpkdeckfgnohf) --
   fast and easy download of many images from Google Images.
   Menu _"Ask where to save each file before downloading"_
   must be turned off before downloading images.
   - [Hands-on TensorBoard (TensorFlow Dev Summit 2017)](https://youtu.be/eBbEDRsCmv4) --
   demonstration of TensorBoard tool
   with the [source code](https://github.com/martinwicke/tf-dev-summit-tensorboard-tutorial).
   - [Deep Learning add-on for Adaptive Vision Studio Lite](https://www.adaptive-vision.com/en/software/deep-learning) --
   if you need out-of-the-box solutions, you could try Adaptive Vision tool with Deep Learning add-on.
   [Lite version](https://www.adaptive-vision.com/en/software/editions)
   is a limited-functionality freeware available for trial and non-commercial use.
   Recommended especially for students who want to learn how to develop
   complex algorithms with images loaded from files.
   - [K3D Jupyter](https://github.com/K3D-tools/K3D-jupyter) --
   Jupyter notebook extension for *3D visualization*
   with [examples](https://github.com/K3D-tools/K3D-jupyter/tree/master/examples).
   Just start the examples and you'll like it :-)
   - [Juxtapose](https://juxtapose.knightlab.com/) --
   Easy-to-make frame comparisons. Useful for presentations.
   Generate an embedable code snippet that you can use on any website or Jupyter notebook.
   There must be an Internet connection to obtain URL.
   Copy-paste code below into your Jupyter notebook and replace the `url`:

```python
# Display the associated webpage in a new window
import IPython
url = 'https://cdn.knightlab.com/libs/juxtapose/latest/embed/index.html?uid=7e8015a0-4be7-11e9-8106-0edaf8f81e27'
iframe = '<iframe frameborder="0" class="juxtapose" width="100%" height="600" src="' + url + '"></iframe>'
IPython.display.HTML(iframe)
```

---
### <a name="exercises" />Exercises and tutorials

   - [Get Started with TensorFlow](https://www.tensorflow.org/tutorials) --
   page with many tutorials. Everyone should make at least 5 exercises for beginners
   and then continue with more advanced tutorials.
   - [Neural Networks Demystified](https://www.youtube.com/playlist?list=PLiaHhY2iBX9hdHaRr6b7XevZtgZRa1PoU) --
   intro into neural networks
   with the [source code](https://github.com/stephencwelch/Neural-Networks-Demystified).
   - [A Neural Network in 11 lines of Python (Part 1)](http://iamtrask.github.io/2015/07/12/basic-python-network) --
   simple neural network in 11 lines of Python code with detailed explanation
   and [Russian translation is here](https://habr.com/ru/post/271563).
   - [A Neural Network in 13 lines of Python (Part 2 - Gradient Descent)](https://iamtrask.github.io/2015/07/27/python-network-part2) --
   explanation of gradient descent with 13 lines of Python code.
   - [Create A Neural Network That Classifies Diabetes Risk In 15 Lines of Python](https://youtu.be/T91fsaG2L0s) --
   Neural network in 15 lines of Python code to diagnose diabetes.
   Russian translation [here](file:///D:/Pavlenko/%23_%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D1%8B/Python/2019.02.25_ML_study/2019.02.27%20Diabetes/%D0%9D%D0%B5%D0%B9%D1%80%D0%BE%D0%BD%D0%BD%D0%B0%D1%8F%20%D1%81%D0%B5%D1%82%D1%8C%20%D0%BD%D0%B0%20Python%20%D0%B2%2015%20%D1%81%D1%82%D1%80%D0%BE%D0%BA%20%D0%BA%D0%BE%D0%B4%D0%B0%20%D0%B4%D0%BB%D1%8F%20%D0%B4%D0%B8%D0%B0%D0%B3%D0%BD%D0%BE%D1%81%D1%82%D0%B8%D0%BA%D0%B8%20%D0%B4%D0%B8%D0%B0%D0%B1%D0%B5%D1%82%D0%B0.html).
   Text explanations are [here](https://www.andreagrandi.it/2018/04/14/machine-learning-pima-indians-diabetes/).
   CSV database file is [here](https://www.kaggle.com/uciml/pima-indians-diabetes-database).
   - [Unsupervised Learning by Siraj Raval](https://youtu.be/8dqdDEyzkFA) --
   using signal processing and K-means clustering to extract and sort neural events in Python plus
   [source code](https://github.com/llSourcell/spike_sorting)
   and [dataset](http://www.vis.caltech.edu/~rodri/Wave_clus/UCLA_data.zip).
   - [Unet Segmentation in Keras](https://youtu.be/M3EZS__Z_XE) --
   easy explanation of the U-net in Keras
   with the [source code](https://github.com/nikhilroxtomar/UNet-Segmentation-in-Keras-TensorFlow/blob/master/unet-segmentation.ipynb).
   - [Deep Residual Unet Segmentation in Keras](https://youtu.be/BOoBWRTpaKk) --
   easy explanation of the ResUNet in Keras
   with the [source code](https://github.com/nikhilroxtomar/Deep-Residual-Unet/blob/master/Deep%20Residual%20UNet.ipynb).
   - []() --

---
### <a name="limit" />Limit TensorFlow to one GPU

```shell
# Calculate on the 2nd GPU
import os
os.environ["CUDA_DEVICE_ORDER"] = "PCI_BUS_ID"  # so the IDs match nvidia-smi
os.environ["CUDA_VISIBLE_DEVICES"] = "1"  # "0,1" for multiple GPU or "-1" for CPU

# For Jupyter Notebook one has to restart the kernel
# after changing of environment variables.

# Monitor GPU permanently. To exit, press <Ctrl>+<C> keys.
watch -n 0.5 nvidia-smi
# Monitor CPU permanently. To exit, press <Q> key.
htop
```
