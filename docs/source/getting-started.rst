===============
Getting Started
===============
Sutra is a python package to calculate the advective removal of microbial organisms 
(also called 'pathogens') from source to end_point.

Main features:
 - Includes database of removal parameters for microbial organisms. 
 - Calculate the removal and concentration of the microbial organism over distance, and with time   

Installation
------------
To get the latest stable version, use::

    python -m pip install git+https://github.com/KWR-Water/sutra.git@main

or::

    pip install git+ssh://git@github.com/KWR-Water/sutra.git@main


Philosophy
----------

..
  #AH AH @MartinvdS @MartinK ...  what here?

Sutra calculates the subsurface removal of microbial organisms over a distance and with time using an analytical approach.  
The aim is to allow for a quick assessment of subsurface removal of microbial organisms for a growing selection of species.    
A database was added, starting with plant pathogens 'solani' (Dickeya solani), 'carotovorum' (Pectobacterium carotovorum), 
and 'solanacearum' (Ralstonia solanacearum). Additional species will be added once more data will become available. 
