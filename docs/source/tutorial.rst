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

.. ipython:: :python
    import numpy as np
    import pandas as pd
    import os
    import sys
    from pathlib import Path

.. import sutra.removal_functions as RF

Create a reference to the parent directory

.. .. ipython:: python
..    path = Path(__file__).parent

Scenario A: Calculate removal of a microbial organism using default database parameters. 

.. .. ipython:: python

## Default removal parameters ##

organism_name = "carotovorum"

# Redox condition: 3 options ['deeply_anoxic','anoxic','suboxic']
redox_cond = 'anoxic'

# alpha0: 'sticky coefficient' [-]
alpha0 = 0.577
# Reference pH for calculating 'alpha' [-]
reference_pH = 7.5
# --> if pH == reference_pH, then collision efficiency alpha equals the value of alpha0
# time dependent inactivation coefficient 'mu1' [day-1]
mu1 = 0.1279
# organism diameter [m]
organism_diam = 1.803e-6

# Starting concentration
conc_start = 1.
# Ambient groundwater concentration
conc_gw = 0.

# effective porosity
por_eff = 0.33    
# Sediment grainsize      
grainsize = 0.00025
# pH of the groundwater   
pH_water = 7.5
# Water temperature   
temp_water = 10.
# Water density [kg m^-3]
rho_water = 999.703

# Distance traveled along pathline [m]
distance_traveled = 1.
# Time traveled [days]
traveltime = 100.
# Porewater velocity [m day-1]
porewater_velocity = distance_traveled / traveltime

First initialize a class for calculating the removal of an organism
Return the (default) removal parameter values
.. .. ipython:: python
mbo_removal_scenA = RF.MicrobialRemoval(organism = organism_name)
removal_parameters = mbo_removal_scenA.removal_parameters

Calculate final concentration after advective microbial removal
.. .. ipython:: python
C_final_default = mbo_removal_default.calc_advective_microbial_removal()

Retrieve lambda (default): inactivation rate
.. .. ipython:: python
lambda_default = mbo_removal_default.lambda_ 






Scenario B1: Manual input of removal parameters, not included in default database

.. .. ipython:: python
# Organism name
organism_name = "MS2"
# effective porosity
por_eff = 0.33    
# Sediment grainsize      
grainsize = 0.00025
# pH of the groundwater   
pH_water = 7.5
# Water temperature   
temp_water = 10.
# Water density [kg m^-3]
rho_water = 999.703

## Removal parmeters ##

# Redox condition: 3 options ['deeply_anoxic','anoxic','suboxic']
redox_cond = 'anoxic'
# alpha0: 'sticky coefficient' [-]
alpha0 = 0.001 

# Reference pH for calculating 'alpha' [-]
reference_pH = 7.5
# --> if pH == reference_pH, then collision efficiency alpha equals the value of alpha0

# time dependent inactivation coefficient 'mu1' [day-1]
mu1 = 0.149
# organism diameter [m]
organism_diam = 2.33e-8

# Distance traveled along pathline [m]
distance_traveled = 1.
# Time traveled [days]
traveltime = 100.
# Porewater velocity [m day-1]
porewater_velocity = distance_traveled / traveltime

# Starting concentration
conc_start = 1.
# Ambient groundwater concentration
conc_gw = 0.
 
First initialize a class for calculating the removal of an organism
mbo_removal_B1 = RF.MicrobialRemoval(organism = organism_name)

Calculate (relative) concentration following advective microbial removal
.. .. ipython:: python
C_final = mbo_removal_B1.calc_advective_microbial_removal(grainsize = grainsize,
                                        temp_water = temp_water, rho_water = rho_water,
                                        pH = pH_water, por_eff = por_eff, 
                                        conc_start = conc_start, conc_gw = conc_gw,
                                        redox = redox_cond,
                                        distance_traveled = distance_traveled, 
                                        traveltime = traveltime,
                                        organism_diam = organism_diam,
                                        mu1 = mu1,
                                        alpha0 = alpha0,
                                        reference_pH = reference_pH )

# k_att, calculated
k_att = mbo_removal.k_att
# lambda, calculated
_lambda = mbo_removal.lambda_

Scenario B2: An alternative way to enter removal parameters and calculate the final concentration
# Should compare to previous input, be aware to enter the correct redox related values for 'anoxic' situation

.. .. ipython:: python
mbo_removal_B2 = RF.MicrobialRemoval(organism = organism_name,
                alpha0_suboxic=None,
                alpha0_anoxic=0.001,
                alpha0_deeply_anoxic=None,
                reference_pH_suboxic=None,
                reference_pH_anoxic=reference_pH,
                reference_pH_deeply_anoxic=None,
                mu1_suboxic=None,
                mu1_anoxic=mu1,
                mu1_deeply_anoxic=None,
                organism_diam=organism_diam,
                )

Calculate the final concentration, removal parameters for redox condition 'redox_cond'
read from 'removal_parameters'. Check these values as follows
.. .. ipython:: python
removal_parameters = mbo_removal_scenB2.removal_parameters

.. .. ipython:: python
C_final = mbo_removal_B2.calc_advective_microbial_removal(grainsize = grainsize,
                                        temp_water = temp_water, rho_water = rho_water,
                                        pH = pH_water, por_eff = por_eff, 
                                        conc_start = conc_start, conc_gw = conc_gw,
                                        redox = redox_cond,
                                        distance_traveled = distance_traveled, 
                                        traveltime = traveltime)

# k_att, calculated
k_att = mbo_removal.k_att
# lambda, calculated
_lambda = mbo_removal.lambda_