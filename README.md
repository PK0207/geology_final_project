# Stellar Classification
Beyond the star in our own system, it is rather difficult to observe other stars due to techology, distance, interference, etc. In the new era of new telescopes and large stellar surveys there have many astronomers who have taken on the feat of perfecting the physical structure and feature assumptions of various different subgroups of stars. Classifying stars is useful to understand the patterns in physical properties and therefore formation histories, chemical processes, and other astrophysics. However, stellar classification is a notoriously difficult task in astronomy. The boundaries between stellar classes are debated due to new discoveries in physical chemical properties. H-R diagram placement is plagued with systematic errors, this makes testing evolutionary models impossible. There are two persistent complications in this pursuit:
1. _It is difficult to accurately measure stellar parameters_
2. _Drawing phyiscal relationships between stellar observations and parameters is non-trivial_

The first issue also exacerbates the second. Our goal in this project is to take **a physics agnostic approach to determining stellar classes** and use machine learning to highlight the inherent strong relationships within the data themselves.
Stellar classes are usually represented on a Hertzsprunng-Russel (HR) diagram (shown below). This diagram plots the luminosity (observed) of stars against their temperature (derived). The resultant shape of the HR diagram is the evolutionary tracks a star could follow in its lifespan:

![HR_diag](https://github.com/user-attachments/assets/6410d742-d82e-48f8-af1e-536099bf4c5a)

The star starts from the bottom right, and evolves along the main-sequence, but could "turn-off" to the giant branch, dwarf branch, or supergiant branch. _Our data will be primarily comprised of main sequence stars._ Notice in the image above, along the x-axis are the stellar classifications - OBAFGKM - listed roughly in decreasing mass order. The best indicator of stellar class is the temperature, and we will be using three different datasets to see if:
1. Within the data set whether clear stellar classifications can be determined (using and not using temperature data)
2. Between datasets whether these classifications match.

# Data
The first data set we are using is from targets of the (Transiting Exoplanet Sattelite Survey) TESS planetary host star sample data set. This star sample includes stellar properties derrived from Gaia photometric observations and applied to stellar isochrone fitting to get the temperature, surface gravity, mass, and luminosities that we will use. This is considered to be one of largest and most thorough data available to astronomers, and was designed to estimate distances to stars. The data was made available through the Gaia [https://gea.esac.esa.int/archive/](online archive). 

Our second sample is from the Yu et al 2023 sample of revised extinctions and radii for 1.5 million stars observed by APOGEE, GALAH, and RAVE. This sample well known to provide a well derived and unbiased set of parameters for a large number of stars with both spectroscopic and photometric data. The data was made available throug [https://iopscience.iop.org/article/10.3847/1538-4365/acabc8#apjsacabc8t4](this paper) (Yu et al. 2023)

The third set we are using "the GALAH_DR3_main_allstar_v2 is our main results catalogue. It contains results for 588,571 stars observed as part of the GALAH, K2-HERMES, TESS-HERMES, and other related surveys that used the HERMES spectrograph on the Anglo-Australian Telescope between November 2013 and February 2019. For all targets we provide stellar parameters, radial velocities, and elemental abundances." [https://www.galah-survey.org/dr3/the_catalogues/#gaia-edr3-data-for-all-stars-in-galah-dr3]

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
For each dataset, we search for the temperature classifications using _K Means Clustering_, examine the explained variance from each stellar parameter used using _Principal Component Analysis_, and train a _Random Forest Classifier_ to see if these groupings are maintained across subsamples of the data. We note that for K means clustering, the data is _not_ normalized beforehand as we want to understand the deviation from official temperature bounds for the 5 classes in each datasets. We present an example of clustered, normalized data and discuss what patterns we find there.

Differentiating between low mass and intermediate mass starsis an ongoing field of research in stellar structure research (Beyer et al. 2024). Main sequence spectral types were generally broken up into O, B, A, F, G, K, and M type stars. These sub-groups were categorized based on mass, effective temperature and color (Pecaut & Mamajek 2022). Our star samples allow us to see clustering groups along the F, G, K, and M spectral type lines. Due to our sample selection, we see agreeing results from the K means clustering along the classification lines just based on effective temperature and logg parameters.

## K means clustering
### Gaia K-means clustering results

![image](https://github.com/user-attachments/assets/b374fef8-8098-49c6-8d70-e86a6af3f3e2)

The clustering results show that for low mass stars (stars further right on the diagram), the classification is preferentially at higher temperatures, and for high mass stars are preferentially classified lower. This effect is only seen at the boundaries of these categories.

![image](https://github.com/user-attachments/assets/bebca7f5-4b47-4afc-9b65-04be56eda7fe)

The confusion matrix demonstrates this preferential misclassification seen in all categories except the highest mass (M0) stars.

### GALAH clustering results

![image](https://github.com/user-attachments/assets/315329f8-deba-46e7-8e0c-1ab41539c251)

This plot is of the GALAH survey data with five cluster groups. We can see here a distinct relation between the groups and the divisions in spectral class. These clusters line up better with the boundaries of the spectral classes, and the centers follow the main sequence line and aren't pulled up by the giant branch "turn-off" in the top right.
This dataset also exclusively misclassifies points as higher temperature as they actually are, which again is represented in the confusion matrix below:

![image](https://github.com/user-attachments/assets/5ce22055-4c1d-44c3-a090-b70ecaf91ce9)


### GALAH+APOGEE+RAVE clustering results
The dataset that combines multiple surveys performs the worst when it comes to identifying boundaries in temperature classifications. This is unsurprising because the way that temperature of the stars is determined in these datasets differs based on what information they are able to derive it from:

![Yu_clust_5wTemp](https://github.com/user-attachments/assets/83f5594a-a927-4f44-aeb7-3b1495a25319)

These differences result in G0 stars being entirely misclassified as higher mass stars:

![image](https://github.com/user-attachments/assets/3622ea46-c134-4ee5-9af8-5d5d86b92c83)

### Clustering on normalized data
By clustering on non-normalized data, the clustering depends almost entirely on the temperature divisions, as these numbers (of order ~$10^3$ K) are much greater than any of the other parameters involved.
When we run clustering on normalized data, the question that we ask is a little different: _What divisions in the shape of the HR diagram can we see when other parameters are introduced?_ This investigation is done on the dataset that combines surveys as the HR diagram has more diversity in stars and has more points:

 ![image](https://github.com/user-attachments/assets/b2710b96-4ed4-47e6-b07d-85252d57ddd6)

Here we can see divisions going along the main sequence track, and the algorithm is able to identify the giant branches in the top right. The three main sequence groups can be classified as low mass, and intermediate mass stars, where there are two subgroups in the intermediate mass category (groups 2&4). This pattern can be seen in the clustering of non-normalized data as well.

### Cool Result: The Kraft Break!
Another clustering effect we see is the Kraft Break. The Kraft Break is known as the change in rotational velocity between low and intermediate mass stars. It is well known that stars above ~8 Msol will end their lifetime as black holes. The division and understanding of stellar evolution for low and intermediate mass stars is less known. In previous research we know that regular main seuqence F-type stars above 6550 K will continue to rapidly rotate as they evolve and any star less than 6550 K will continue to slowly lose their rotational speed over time. This break can then be theorized back to the internal physics within the stars. Inside the lower mass stars have an outter convective zone which generates a magnetic field. When coupled with the winds the star feels a torque againsts its rotation and begins to slow down. Stars above the break lack an outter convective zone, which causes them to lack an induced magnetic field and the star continues to rapidly rotate over its lifetime. 

### The class boundaries across datasets:
![image](https://github.com/user-attachments/assets/eddf6a2d-cb1e-4699-a265-e17dc68da10c)

The table above shows the class boundaries identified in each dataset, with the "true" boundaries listed in the right-most column. These true temperatures are spectral class features defined in Pecaut & Mamajek 2022. Every dataset finds higher bounds in every class, but the differences are the lowest in the G0 stars class. This is the class of our sun, meaning its properties are very well studied and we have the most confidence in derived parameter values like temperature in this class.

## Principal Component Analysis
Before conducting PCA on the data, we normalized everything in order to examine the loadings of the 1st PC.

### Gaia and TESS
The temperature, surface gravity, luminosity, and mass (all related and interchangeable parameters) are weighted about equally for the 1st PC. **The explained variance of PC1 in this dataset is the highest at 61%**

![image](https://github.com/user-attachments/assets/1274619b-eb11-43ab-b22d-d7deaceaa654)


### GALAH
The temperature and surface gravity in the 1st PC are very strongly weighted, indicating that the explained variance primarily lies along these two parameters. These are also analogues of each other, so there is some degeneracy of information here. **The explained variance of PC1 in this dataset is 46%**

![image](https://github.com/user-attachments/assets/4adbf8ef-eb85-4c2c-977a-213fa5030c5f)


### GALAH + APOGEE + RAVE
The temperature, surface gravity, luminosity, and radius have about equal loadings. What is interesting is that the first three are analogues of each other (as introduced in the HR diagram discussion) while the latter is derived from them. **The explained variance of PC1 in this dataset is 51%**

![image](https://github.com/user-attachments/assets/afff791d-791f-429f-a90f-9bd5d1434132)
