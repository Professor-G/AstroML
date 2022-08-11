.. AstroML documentation master file, created by
   sphinx-quickstart on Thu Aug 11 10:50:55 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to AstroML!
===============================
AstroML is a compilation of machine learning tutorials and examples, all in the context of astronomy research and astrophysical object classification. The examples include working with lightcurves as well as single and multi-band images.

**NOTE:** This is a private project led by an astronomy PhD student, and is not related to the pypi `AstroML <https://pypi.org/project/astroML/>`_ program! 

Installation
==================
The source code and accompanying data can be installed in your local machine by cloning the development version:    

.. code-block:: bash

    git clone https://github.com/Professor-G/AstroML.git
    python setup.py install
    pip install -r requirements.txt

Pages
==================
.. toctree::
   :maxdepth: 1

   source/Introduction
   source/Data Preprocessing
   source/PCA & tSNE
   source/Ensemble Algorithms
   source/Artificial Neural Networks
   source/Convolutional Neural Networks
   source/Data Augmentation
   source/Hyperparameter Optimization

Documentation
==================
Here is the documentation for all the modules:

.. toctree::
   :maxdepth: 1

   source/AstroML
