# Stellar Classification
Classifying stars is useful to understand the patterns in physical properties and therefore formation histories, chemical processes, and other astrophysics. However, stellar classification is a notoriously difficult task in astronomy. The boundaries between stellar classes are hotly debated due to new discoveries in physical chemical properties. There are two persistent complications in this pursuit:
1. _We cannot measure stellar parameters very accurately_
2. _We  do not know the relationships between stellar parameters well, and_
The first issue also exacerbates the second. Our goal in this project is to take **a physics agnostic approach to determining stellar classes** and use machine learning to highlight the inherent strong relationships within the data themselves.
Stellar classes are usually represented on a Hertzsprunng-Russel (HR) diagram (shown below). This diagram plots the luminosity (observed) of stars against their temperature (derived). The resultant shape of the HR diagram is the evolutionary tracks a star could follow in its lifespan:
![HR_diag](https://github.com/user-attachments/assets/6410d742-d82e-48f8-af1e-536099bf4c5a)

The star starts from the bottom right, and evolves along the main-sequence, but could "turn-off" to the giant branch, dwarf branch, or supergiant branch. _Our data will be primarily comprised of main sequence stars._ Notice in the image above, along the x-axis are the stellar classifications - OBAFGKM - listed roughly in decreasing mass order. The best indicator of stellar class is the temperature, and we will be using three different datasets to see if:
1. Within the data set whether clear stellar classifications can be determined (using and not using temperature data)
2. Between datasets whether these classifications match.

# Data
The first data set we are using is the GAIA DR3 TESS target star sample data set. This star sample includes stellar properties derrived from Gaia photometric observations and applied to stellar isochrone fitting to get the temperature, surface gravity, mass, and luminosities that we will use. This is considered to be one of largest and most thorough data available to astronomers, and was designed to estimate distances to stars. The data was made available through the Gaia [https://gea.esac.esa.int/archive/](online archive).

Our second sample is from the Yu et al 2023 sample of revised extinctions and radii for 1.5 million stars observed by APOGEE, GALAH, and RAVE. This sample well known to provide a well derived and unbiased set of parameters for a large number of stars with both spectroscopic and photometric data. The data was made available throug [https://iopscience.iop.org/article/10.3847/1538-4365/acabc8#apjsacabc8t4](this paper) (Yu et al. 2023)

The third set we are using "the GALAH_DR3_main_allstar_v2 is our main results catalogue. It contains results for 588,571 stars observed as part of the GALAH, K2-HERMES, TESS-HERMES, and other related surveys that used the HERMES spectrograph on the Anglo-Australian Telescope between November 2013 and February 2019. For all targets we provide stellar parameters, radial velocities, and elemental abundances."

## Data Cleaning and Truncation
We cut our data to include stars with an effective temperature less than 9000K. We do this to eliminate any very high mass stars, which are less common, more variable, and harder to derive the properties of. We chose to only include stars brigher than 18 magnitudes (stars with a larger magnitude are dimmer). We calculate this magnitude using the observed parallax and the photmeteric g band magnitude.
We determine the absolute magnitude to be:
$M = m + 5*(\log_{10} (1/ \text{parallax}))$
**GALAH + APOGEE + RAVE**
- Data missing some metallicities, dropping those rows entirely
- Cutting off all stars with temperatures greater than $T_{eff}=9000K$ to limit selection to less extreme high mass stars, intermediate and low mass stars.

![Yu_HR](https://github.com/user-attachments/assets/7cb71876-bc85-4ed7-bf54-bb0b81051f9c)

**GALAH**
- We took the entire sample of GALAH data and made no cuts since the temperatures in the sample did not go over 9000 K.

![galah_hrdiagram](https://github.com/user-attachments/assets/9b567aec-e711-4c9c-beba-db2316ab8fbc)

**Gaia and TESS**

![tess_hrdiagram](https://github.com/user-attachments/assets/2185ae27-971a-419a-85a7-21f810153bfc)

# Environment Management
To create a python environment that is suitable to run this code, please run the following:
```
python3 -m venv geo_final
source geo_final/bin/activate
pip install -r requirements.txt
python -m ipykernel install --user --name geo_final --display-name "Python geo_final"
```
From this environment, you can run `jupyter notebook` and select the kernel named `geo_final`

# Analysis 

## K means clustering
We are looking to determine if the K means clustering can determine the various differentiated charactersitics which astronomers have placed on different stars. Some of these groups include evolutionary states such as main sequence stars, red giant stars, and white dwarfs. Another includes differentiating between low mass and intermediate mass stars, which is an ongoing field of research in stellar structure research (Beyer et al. 2024). Main sequence spectral types were generally broken up into O, B, A, F, G, K, and M type stars. These sub groups were categorized based on mass, effective temperature and color (Pecaut & Mamajek 2022). Our star samples allow us to see clustering groups along the F, G, K, and M spectral type lines. Due to our sample selection, we see agreeing results from the K means clustering along the classification lines just based on effective temperature and logg parameters. We chose 
### Gaia K-means clustering results
![gaia_hrdiagram_cluster](https://github.com/user-attachments/assets/3efeb390-e76a-4950-b093-b2f9efd31ed9)

### GALAH clustering results
![galah_hrdiagram_cluster](https://github.com/user-attachments/assets/62d04d80-d3de-41db-94c4-467beb5754c6)

### GALAH+APOGEE+RAVE clustering results
![Yu_clust_3wTemp](https://github.com/user-attachments/assets/6fe2a0a5-2b27-4864-818f-133806198fbc)
![Yu_clust_5wTemp](https://github.com/user-attachments/assets/83f5594a-a927-4f44-aeb7-3b1495a25319)


Another clustering effect we see is the Kraft Break. The Kraft Break is known as the change in rotational velocity between low and intermediate mass stars. It is well known that stars above ~8 Msol will end their lifetime as black holes. The division and understanding of stellar evolution for low and intermediate mass stars is less known. In previous research we know that regular main seuqence F-type stars above 6550 K will continue to rapidly rotate as they evolve and any star less than 6550 K will continue to slowly lose their rotational speed over time. This break can then be theorized back to the internal physics within the stars. Inside the lower mass stars have an outter convective zone which generates a magnetic field. When coupled with the winds the star feels a torque againsts its rotation and begins to slow down. Stars above the break lack an outter convective zone, which causes them to lack an induced magnetic field and the star continues to rapidly rotate over its lifetime. 
