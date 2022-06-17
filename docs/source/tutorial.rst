========================================================================================================
Tutorial
========================================================================================================

Overview
========

Sutra is a python package to calculate the advective removal of microbial organisms 
(also called 'pathogens') from source to end_point.

Main features:
 - Includes database of removal parameters for microbial organisms. 
 - Calculate the removal and concentration of the microbial organism over distance, and with time   

Steps
-----

Operating the microbial organism removal involves 2 steps:

#. Run/load the removal_functions.MicrobialRemoval class to retrieve the default microbial (removal) parameters, if present in the database. Otherwise, an empty dataframe is returned.
#. Run removal_functions.calc_advective_microbial_removal to calculate the final concentration after a distance and time traveled.

Now, let’s try some examples. First we import the necessary python packages

.. ipython:: python

import numpy as np
import pandas as pd
import os
import sys
from pathlib import Path


    .. import sutra.removal_functions as RF