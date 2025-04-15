# DATA
We are using the GAIA DR3 star sample data set. This is considered to be one of largest and most thorough data sets for stellar astronomers to use. 
We are looking to K-means clustering to identify the stars in the sample and their characteristics on the HR diagram. 
Our first sample is from the Gaia DR3 star survey. 
We determine the absolute magnitude M = m + 5*(log 10 (1/ parallax)


A backup sample is "the GALAH_DR3_main_allstar_v2 is our main results catalogue. It contains results for 588,571 stars observed as part of the GALAH, K2-HERMES, TESS-HERMES, and other related surveys that used the HERMES spectrograph on the Anglo-Australian Telescope between November 2013 and February 2019. For all targets we provide stellar parameters, radial velocities, and elemental abundances."
From GALAH we care about: 
- logg
- teff
- Fe/H

# Environment Management
To create a python environment that is suitable to run this code, please run the following:
```
python3 -m venv geo_final
source geo_final/bin/activate
pip install requirements.txt
python -m ipykernel install --user --name geo_final --display-name "Python geo_final"
```
From this environment, you can run `jupyter notebook` and select the kernel named `geo_final`
