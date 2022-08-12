.. _Data_Preprocessing:

Data Preprocessing
==================

Lightcurves
-----------
In most machine tutorials you will run into two common variables: data_x and data_y.

data_x corresponds to the 2-dimensional features matrix, with each row being the one-dimensional feature vector for that particular sample -- data_y is then the corresponding 1-dimensional labels vector. 

In this tutorial we will be working with 150 lightcurves, 30 lightcurves per class: Variable (VAR), Long Period Variable (LPV), Constant (CON), Cataclysmic Variable (CV), and Microlensing (ML). These five classes have been simulated, 30 times each, therefore our training set contains 5 classes, with each containing 30 samples. 

The lightcurves for these 150 simulated lightcurves can be :download:`downloaded here <lightcurves.zip>`. Each text file corresponds to one lightcurve, each containing three columns: time, mag, magerr

We will load these lightcurves to construct our training data matrix, which will look like this:

.. figure:: _static/feature_matrix.png
    :align: center

    Figure 1: Two-dimensional training data matrix, where the first column contains the label of each sample, with the corresponding row representing the feature vector for that particular sample. 


|

First, we will load the lightcurve filenames by specifying the path in which they're located in your local machine:

.. code-block:: python

    import os
    import numpy as np

    path = '/Users/daniel/lightcurves/' 
    filenames = os.listdir(path) 

If you print the filenames you will note that each filename is titled according to its class, so let's first construct our data_y vector by saving the name of the filename, but we don't want the numbers, only the name before the '_' character:

.. code-block:: python
	
	data_y = []

	for name in filenames:
		label = name.split('_')[0]
		data_Y.append(label)

Now that we have our labels vector, we must construct the individual features vector for each lightcurve, after which we will combine all these as rows for our features matrix. For simplicity let's only calculate three features: the mean, the standard deviation, and the standard deviation over the mean. 

We will load each file and record these three features in separate lists:

.. code-block:: python

	mean = []
	std = []
	std_over_mean = [] 

    for name in filenames:
		lightcurve = np.loadtxt(path+name)
		mag = lightcurve[:,1]
		mean.append(np.mean(mag))
		std.append(np.std(mag))
		std_over_mean.append(np.std(mag)/np.mean(mag))

Finally, to construct the data_x features matrix, we need to combine these three vectors row-wise, which the numpy.c_[] function can do for us:

.. code-block:: python

	data_x = np.c_[mean, std, std_over_mean]

We can see that the shape of this matrix is (150, 3): 150 samples (rows), and 3 features (columns). With both the data_y and data_x variables, we are ready to begin training our machine learning classifiers, but to avoid this procedure in the future, let's combine these two matrices to construct our entire training data matrix:

.. code-block:: python

	training_matrix = np.c_[data_y, data_x]

This matrix is the one shown in Figure 1, which we can now save for easier access in the future (note that since our matrix contains labels that are strings, we must specify the format when saving the file):

.. code-block:: python

	np.savetxt('training_data', training_matrix, fmt='%s')

In the future we can load this matrix, and re-construct data_x and data_y. Note that when loading this file, we need to specify that that the dtype is string, since our labels are not numerical. Therefore when designating our data_x matrix, we must set the astype to float:

.. code-block:: python

	training_data = np.loadtxt('training_data', dtype=str)
	data_y = training_data[:,0]
	data_x = training_data[:,1:].astype('float')

Part of data processing may include normalizing the data_x matrix, which is specially important when working with artificial neural networks, since at each epoch, the weights must be updated, and if the feature ranges are huge, the gradient will explode!

For example, we can check the range of our three features using numpy.ptp()

.. code-block:: python

	print('mean range: {}'.format(np.ptp(mean)))
	print('std range: {}'.format(np.ptp(std)))
	print('std over mean range: {}'.format(np.ptp(std_over_mean)))
	
The largest range is within our mean feature, and while a range of 6 may not cause issues, it is always recommended to normalize features when using neural networks. For more information see the other pages! 


To normalize your data, you can use the `sklearn scalers <https://scikit-learn.org/stable/modules/preprocessing.html>`_. There are multiple scales for different normalization types, but they all function the same way -- first we create the scaler itself, for now let's try the StandardScaler(), which scales to unit variance:

.. code-block:: python

	from sklearn.preprocessing import StandardScaler

	scaler = StandardScaler()

We have the standard scaler ready to go, but it's important to remember that if we scale our data during training, we must also scale new, unseen data, in the same manner. This is why the scaler must be fitted and saved. Let's create our scaler by passing along our data_x matrix:

.. code-block:: python

	scaler.fit(data_x)

Now that we have fit our scalar, we need to use it to transform our original data_x, which we can call scaled_data_x:

.. code-block:: python

	scaled_data_x = scaler.transform(data_x)

These preprocessing scalers scale column by column, therefore each feature is scaled independently of one another.

**More importantly, in order to use this scaler in the future, as we must to scale new, unseen data, we must pass along a vector, or matrix, containing the same number of features, in the same order!!!**

If you have a lightcurve you want to predict in the future, but for some reason you are missing one feature, then the scaler will not work, as it needs the same number of features as it was fitted with. In practice you would replace these missing values with 0, for example, but just remember that if you're scaling your training data, you must also scale any new data going forward. Therefore you must always reconstruct the scaler by fitting the original data_x again, so that you can use it to .transform() new data.


Images
------

















      

