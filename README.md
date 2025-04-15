# DATA
We are using the GAIA DR3 star sample data set. This is considered to be one of largest and most thorough data sets for stellar astronomers to use. 
We are looking to K-means clustering to identify the stars in the sample and their characteristics on the HR diagram. 
Our first sample is from the Gaia DR3 star survey. We cut our data to include stars with an effective temperature less than 9000K. We do this to elimnate any very high mass stars, which are less common, more variable, and harder to observe. We also then calculate the absolute magnitude of the stars in our sample using the observed parallax and the photmeteric g band magnitude. Once we have a absolute magnitude we can make a magnitude cut. 
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
- Cutting off all stars with temperatures greater than $T_{eff}=7000K$ to limit selection to main sequence stars

# Environment Management
To create a python environment that is suitable to run this code, please run the following:
```
python3 -m venv geo_final
source geo_final/bin/activate
pip install -r requirements.txt
python -m ipykernel install --user --name geo_final --display-name "Python geo_final"
```
From this environment, you can run `jupyter notebook` and select the kernel named `geo_final`
