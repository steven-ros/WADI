.. _ipython_directive:

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

During transport in the subsurface, irus removal takes place by both attachment to the soil matrix and by inactivation.
The virus concentration 'C' [m-3] through steady-state transport of microbial organisms along pathlines in the saturated
groundwater can be approximated by:

$dC/dx + ((\lambda)/v_por)*C=0$

Where lambda equals k_att + mu1
'k_att': attachment coefficient [day-1]
'mu1': inactivation coefficient [day-1] 
x: the distance traveled [m] 
v_por: the porewater velocity [m day-1] or 'darcyflux divided by the effective porosity'

Assuming that the background concentration of the relevant microbial organism equals 0, the relative removal can be calculated as follows.

log(C_x/C_0)=-((k_att+mu1))/ln⁡(10) *  x/v_por	

The attachment coefficient k_att depends on the effective porosity 'por_eff', the grain diameter of the sediment 'grainsize',
'sticky coefficient' alpha [day-1], the porosity dependent Happel's parameter 'As_happ', diffusion constant 'D_BM' [m2 day-1], and
the porewater velocity.

k_att=3/2∙((1-por_eff))/grainsize ∙alpha*4*As_happ^(1⁄3)*(D_BM/(grainsize*por_eff*v_por))^(2⁄3)∙v_por

Steps
=========

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
    import sutra.removal_functions as rf


Scenario A: Calculate removal of a microbial organism using default database parameters. 

.. .. ipython:: python

## Default removal parameters ##

organism_name = "carotovorum"

# Redox condition: 3 options ['deeply_anoxic','anoxic','suboxic']
redox_cond = 'anoxic'

# alpha0: 'sticky coefficient' [-]
alpha0 = 0.577
# Reference pH for calculating 'alpha' [-]
pH0 = 7.5
# --> if pH == pH0, then collision efficiency alpha equals the value of alpha0
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
mbo_removal_scenA = rf.MicrobialRemoval(organism = organism_name)
removal_parameters = mbo_removal_scenA.removal_parameters

Calculate final concentration after advective microbial removal
.. .. ipython:: python
C_final_default = mbo_removal_default.calc_advective_microbial_removal()

Retrieve lambda (default): inactivation rate
.. .. ipython:: python
lambda_default = mbo_removal_default.lambda_rate 






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
pH0 = 7.5
# --> if pH == pH0, then collision efficiency alpha equals the value of alpha0

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
mbo_removal_B1 = rf.MicrobialRemoval(organism = organism_name)

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
                                        pH0 = pH0 )

# k_att, calculated
k_att = mbo_removal.k_att
# lambda, calculated
lambda_rate = mbo_removal.lambda_rate

Scenario B2: An alternative way to enter removal parameters and calculate the final concentration
# Should compare to previous input, be aware to enter the correct redox related values for 'anoxic' situation

.. .. ipython:: python

mbo_removal_B2 = rf.MicrobialRemoval(organism = organism_name,
                alpha0_suboxic=None,
                alpha0_anoxic=0.001,
                alpha0_deeply_anoxic=None,
                pH0_suboxic=None,
                pH0_anoxic=pH0,
                pH0_deeply_anoxic=None,
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
lambda_rate = mbo_removal.lambda_rate