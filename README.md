# DATA
We are using the GAIA DR3 star sample data set. This is considered to be one of largest and most thorough data sets for stellar astronomers to use. We cut our data to include stars with an effective temperature less than 9000K. We do this to elimnate any very high mass stars, which are less common, more variable, and harder to observe. We also then calculate the absolute magnitude of the stars in our sample using the observed parallax and the photmeteric g band magnitude. Once we have a absolute magnitude we can make a magnitude cut. In astronomy we chose a the opposite values, meaning stars with a magnitude greater than zero are less bright than stars greater than zero. So we chose a magnitude cut of only including stars less than 18 magnitudes. This also removes any stars that calculated weird absolute magnitudes from the observations. Our sample initially had 17558141 stars, after our effective temperature and absolute magnitude cuts we have 

We determine the absolute magnitude 
$ M = m + 5*(log 10 (1/ parallax))$


A backup sample is "the GALAH_DR3_main_allstar_v2 is our main results catalogue. It contains results for 588,571 stars observed as part of the GALAH, K2-HERMES, TESS-HERMES, and other related surveys that used the HERMES spectrograph on the Anglo-Australian Telescope between November 2013 and February 2019. For all targets we provide stellar parameters, radial velocities, and elemental abundances."
From GALAH we care about: 
- logg
- teff
- Fe/H

## Data Cleaning and Truncation
**GALAH + APOGEE + RAVE**
- Data missing some metallicities, dropping those rows entirely
- Cutting off all stars with temperatures greater than $T_{eff}=9000K$ to limit selection to less extreme high mass stars, intermediate and low mass stars. 

# Environment Management
To create a python environment that is suitable to run this code, please run the following:
```
python3 -m venv geo_final
source geo_final/bin/activate
pip install -r requirements.txt
python -m ipykernel install --user --name geo_final --display-name "Python geo_final"
```
From this environment, you can run `jupyter notebook` and select the kernel named `geo_final`

# Machine Learning 
K means clustering to determine if we can reproduce the sub groups of stars that astronomers has created. 

# Analysis 
We are looking to determine if the K means clustering can determine the various differentiated charactersitics which astronomers have placed on different stars. Some of these groups include evolutionary states such as main sequence stars, red giant stars, and white dwarfs. Another includes differentiating between low mass and intermediate mass stars, which is an ongoing field of research in stellar structure research (Beyer et al. 2024). Lastly, we will look to possibly see if they can differentiate different spectral classes within stars parameters. Main sequence spectral types were generally broken up into O, B, A, F, G, K, and M type stars. These sub groups were declared by things such as mass, effective temperature and color (Pecaut & Mamajek 2004). 






