# RCar_GRAVITY
This repository contains a series of scripts (in the form of a __Jupyter__ notebook) used for analyzing GRAVITY-VLTI data of the M-type AGB star R Car (see Rosales-Guzman et al., [2023](https://arxiv.org/abs/2303.00056) ). 
The jupyter notebook is divided in 3 parts. Those parts are described below together with some relevant information of the equations and figures. 
No installation is needed, just download the repository and open the Jupyter notebook `RCar_analysis.ipynb` to star working with it using the following command:

`user@users_pc ~ % Jupyter notebook RCar_analysis.ipynb`

### Required packages (Python > 3.0):  
<nav>
 
<a href="https://www.astropy.org">Astropy</a>  
    
<a href="https://corner.readthedocs.io/en/latest/install/">corner</a> 
    
<a href="https://emcee.readthedocs.io/en/stable/user/install/">emcee</a> 
    
<a href="https://lmfit.github.io/lmfit-py/">lmfit</a>  
    
<a href="https://matplotlib.org">matplotlib</a>  
    
<a href="https://numpy.org">numpy</a>  

<a href="https://pypi.org/project/opencv-python/">Open CV</a> 
 
<a href="https://scipy.org/install/">scipy</a>  
    
<a href="https://scikit-learn.org/stable/install.html">sklearn</a>  
    
</nav>  

`oitools.py (provided)`

## MCMC fit to the $V^2$ across the pseudo continuum
To obtain the angular size of R Car across the K-band, we applied a geometrical model of a uniform disk (UD) to the $V^2$ data. The visibility function of our models is given by the following equation (Berger & Segransan [2007](https://doi.org/10.1016/j.newar.2007.06.003)):

$$   V_{\mathrm{UD}}(u,v) = 2(\mathrm{F_r})\frac{J_1(\pi \rho \Theta_\mathrm{UD} )}{\pi  \rho \Theta_{\mathrm{UD}}}\$$

where $\rho = \sqrt{u^2+ v^2}$, $u$ and $v$ are the spatial frequencies sampled by the interferometric observations, $J_1$ is the first order Bessel function, $\Theta_\mathrm{UD}$ and $F_r$ are the angular diameter of the uniform disk profile and a scaling factor that accounts for the over-resolved flux in the observations, respectively. To fit the data, we used a Monte-Carlo Markov-Chain (MCMC) algorithm based on the ```Python``` package ```emcee``` (Foreman et al., [2013](https://iopscience.iop.org/article/10.1086/670067/pdf)). We let 250 walkers evolve for 150 steps using the data of each spectral bin independently. The results of this code are shown as three plots (Figs. 1, 2, and 3). 

<img src="https://user-images.githubusercontent.com/61716000/218837381-187bd24e-e989-49b7-b4c2-d1e3dc5b24d2.png" width="400">

#### Fig. 1 Best-fit to the $V^2$ from the MCMC. The data are shown with red dots and the model with the blue line. The values corresponding to the best model are also shown in the plot. 

<img src="https://user-images.githubusercontent.com/61716000/211225020-eef6c61b-67b5-4d46-80fe-19d571e185b1.png" width="600">

#### Fig 2. Positions of each walker as a function of the number of steps in the chain. 

<img src="https://user-images.githubusercontent.com/61716000/211225049-78dae323-f3e6-4c50-a037-1f939f7aac0d.png">

#### Fig 3. The corner plot shows all the one and two dimensional projections of the posterior probability distributions of our parameters D (or $\Theta_{UD}$) and $F_r$. The marginalized distribution for each parameter is shown in the histograms along the diagonal. The marginalized two dimensional distribution is also plotted ([1](https://emcee.readthedocs.io/en/stable/tutorials/line/)). 

## Single-layer model to fit the $V^2$ across the first and second CO bandheads
The MOLsphere model, used to calculate the size, temperature, and optical thickness of the CO layer consists of a stellar disk with a compact layer around it.  The star is modeled by a stellar surface of radius $R_\*$ which emits as a black-body at a temperature $\mathrm{T_{\*}}$. It is surrounded by a compact spherical layer of radius $\mathrm{R_{L}}$ that absorbs the radiation emitted by the star and re-emits it like a black-body. The MOLsphere is characterized by its temperature $\mathrm{T_{L}}$, radius $\mathrm{R_L}$ and its optical depth $\tau_{\lambda}$. The region between the stellar photosphere and the layer is assumed to be empty (See the figure for a skecth of the model). The analytical expression of the model is given by:

![image](https://user-images.githubusercontent.com/61716000/211214093-77fe21b5-fe47-4bdb-8e93-bb1935bec40a.png)

where  $I_\lambda^r = I_{\lambda}^r(\mathrm{T_{\*}} , \mathrm{T_L}, \mathrm{R_\*}, \mathrm{R_L}, \tau_{\lambda})$,  $\mathrm{T_\*}$ and $\mathrm{T_L}$ are the temperatures of the photosphere and of the CO layer, respectively; $\mathrm{R_*}$ and $\mathrm{R_L}$ are the angular radius of the star and the layer, respectively; $\tau_\lambda$ is the optical depth of the molecular layer at wavelength $\lambda$; $B_\lambda(\mathrm{T})$ is the Planck function (at wavelength $\lambda$ and temperature T); and $\beta$ is the angle between the radius vector and the line-of-sight so that $\cos\beta = \sqrt{1-(\mathrm{r}/\mathrm{R_L})^2}$.

For the fitting, we adopted a $\mathrm{T_{\*}}$=2800 K and $\mathrm{R_{\*}}$=5 mas (Monnier et al., [2014](https://doi.org/10.1117/12.2057312); McDonald et al., [2012](https://doi.org/10.1111/j.1365-2966.2012.21873.x)). We use the disk estimation reported in the H- band because it is considerably less affected from molecular contribution, in contrast with the K-band where  molecules like CO, $H_{2}O$, and OH among others (see e.g. Wittkowski et al.,  [2018](https://doi.org/10.1051/0004-6361/201833029); Paladini [2011](https://core.ac.uk/download/pdf/11597038.pdf)) are present over the entire band. The unknown parameters are then $\mathrm{T_L}$, $\mathrm{R_L}$ and $\tau_L$. We performed a two-step process to estimate these parameters. As first step, for each pair of $\mathrm{T_L}$ and $\mathrm{R_L}$, we performed a least-squares minimization to find the best-fit value of $\tau_L$ that reproduces the spectrum $F_{\lambda}^{\mathrm{R Car}}$. The next step consisted of using an MCMC method based on the ```Python``` library ```emcee``` to estimate the best combinations of $\mathrm{T_L}$, $\mathrm{R_L}$, and $\tau_L$ that reproduce the $V^2$. 
 
To account for the over-resolved flux when modeling the $V^2$ data, we added a scaling factor Fr. This value was estimated simultaneously with the other parameters in the model. 

<img src="https://user-images.githubusercontent.com/61716000/211214604-cc6b37b3-7c0d-4e92-9ebe-5dc5081f5d54.png" width="250">

#### Fig 4. Illustration of the single-layer model. The yellow disk represents the star, and the orange ring represents the layer. The parameters in the image are as described in the text above.

Figs. 5 and 6 are generated by the single-layer model part of the notebook. 

<img src="https://user-images.githubusercontent.com/61716000/211623460-cf831124-c923-4138-b86c-1d5383d3dc6d.png" width="350">

#### Fig 5. Best-fit to the $V^2$ using the Single-layer model. The green dots correspond to the data and the purple ones to the fit (see the labels on the plot). The best-fit parameters are shown in the top right corner of the figure.   

<img src="https://user-images.githubusercontent.com/61716000/211625765-533ea754-47e3-4504-9cbb-64789048129f.png" width="400">

#### Fig 6. Corner plot of all the one and two dimensional projections of the posterior probability distributions of our parameters $T_L$, $R_L$ and $F_r$. As shown in Fig. 3, the marginalized distribution for each parameter is located in the histograms along the diagonal.   


## Bootstrapp method for image reconstruction

To build new data samples from the original one, we employed the bootstrap technique. The samples were created by keeping the original number of data points unaletered, but allowwing random sampling with replacement. This created some data sets where some points have higher weights than in the original data set, and some others with zero weights. This algorithm also produces slight changes in the u-v plane, which allows us to trace the impact of the u-v coverage on the reconstructed images, without loosing the statistical significance of the original number of data points (see Fig. 7). 
According to Babu & Singh [1983](https://www.jstor.org/stable/2240663), the statistical moments such as mean, variance and standard deviation of the bootstrapped samples are good approximations to the statistical moments of the original data sets. Finally, since each bootstrapped data set is different, we will have slightly different reconstructed images. 

<img width="905" alt="image" src="https://user-images.githubusercontent.com/61716000/211895310-813a141b-cb2e-4822-a568-b47bc724fad0.png">

#### Fig 7. From left to right: $V^2$ and CPs as a function of the Spatial Frequency, and UV coverage for a sample of ten bootstrapped data sets. Each color corresponds to a different bootstrapped data set. 
# Principal Component Analysis (PCA) to understand the structure of R Car
To have a more precise characterization of the asymmetries in the reconstructed images of the CO band-heads, we used the Principal Component Analysis (PCA) described by Medeiros et al., [2018](https://iopscience.iop.org/article/10.3847/1538-4357/aad37a/meta). Those authors demonstrate that the visibilities of the Principal Components are equal to the Principal Components of the visibilities. This method is useful to trace the changes across a set of images that have the largest effect on the visibility (amplitude and -closure- phase) profile. In our case, we estimate the most significant structural changes of the observed asymmetric structures across wavelength for each of the observed epochs. The following procedure was applied to the ensemble of wavelength dependent images per epoch to extract their Principal Components.

For the PCA analysis, we normalize each data set by subtracting the corresponding mean image and dividing by their standard deviation image across the wavelength range. To perform the PCA analysis, we used the [CASSINI](https://github.com/cosmosz5/CASSINI) package. This software allows to compute the covariance matrix of the data set and to transform it into the space of the principal components. This allows us to determine the eigenvectors (or eigen-images, in our case) and their corresponding eigenvalues. Since we only have seven images per data set,  and the possible number of components must be smaller or equal than the number of images in the data set, we decided to only keep the first four principal components. From our tests, we observed that those components explain, at least, the 93\% of the variance in the data sets. 

<img src="https://user-images.githubusercontent.com/61716000/220493849-a4324925-16ba-4a62-8f9f-953883b0a319.png" width="600">

#### Fig 8. First four Principal Components for the January epoch for the 1st CO band head. The relative scale of the structures in the eigen-images is displayed with a colorbar at the bottom of each panel. Only the central 20 mas of the eigen-images are shown on each panel. The white contours correspond to the mean image across wavelength (per given data set) and they represent 10, 30, 50, 70, 90, 95, 97, and 99 \% of the intensity's peak. 

<img src="https://user-images.githubusercontent.com/61716000/220494444-56202ea9-0a83-48cc-8f10-4202d47ce857.png" width="800">

#### Fig 9. Representative example of the CPs versus spatial frequencies of the data set that corresponds to the 1st CO band-head at 2.2946 $\mu m$ (gray squares). The colored dots show the CPs from the recovered images obtained with different numbers of Principal Components (see label on the plot). 

# Disclaimer
All the scripts that compose this repository are open source under the GNU General Public License 3.0 and correspond to the A&A article <a href="https://www.aanda.org/articles/aa/full_html/2023/06/aa45370-22/aa45370-22.html#FN2">Imaging the innermost gaseous layers of the Mira star R Car with GRAVITY-VLTI</a>   from Rosales-Guzman et al. For enquiries and/or contributions please contact jarosales@astro.unam.mx
