## Machine Learning code examples in Google Colab
   * [Several callback examples](callbacks_usage_in_training.ipynb) and
     [shareable link](https://colab.research.google.com/drive/1b8cTruDQbw_N_XXna0XOTPuj_FyOR2rW).
     Callbacks are used to stop training to avoid overfitting or if learning rate is enough.
   * [U-Net segmentation](unet_segmentation.ipynb)
     ([shareable link](https://colab.research.google.com/drive/1oGDqlVuBFqzcYu12_L53bjkutBkz9_ne))
     is a convolutional neural network that was developed
     for biomedical image segmentation at the Computer Science Department of the University of Freiburg,
     Germany. [Wiki link](https://en.wikipedia.org/wiki/U-Net).
     The network is based on the fully convolutional network and its architecture was modified
     and extended to work with fewer training images and to yield more precise segmentations.
     Segmentation of a 512*512 image takes less than a second on a modern GPU.
   * [Deep Residual U-Net (ResUNet)](deep_residual_unet_segmentation.ipynb)
     ([shareable link](https://colab.research.google.com/drive/1rhuHSynFqZbyZyRguv7g3TBILdvwG7pB))
     is based on residual neural network and U-Net.
