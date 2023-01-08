# RCar_GRAVITY
This project includes a Jupyter notebook with the codes used to obtain the results reported in Rosales-Guzman et al. for the M-type AGB star R Car. We only provide some examples to make clear what the programs are doing.
The jupyter notebook is divided in 3 parts which are described below together with some relevant equations and figures. 
##### MCMC fit to the V^2 across the pseudo continuum
To obtain the angular size of R Car across the K-band, we applied a geometrical model of a uniform disk (UD) to the V2 data. The visibility function of our models is given by the following equation (Berger & Segransan [2007](https://doi.org/10.1016/j.newar.2007.06.003)):

$$   V_{\mathrm{UD}}(u,v) = 2(\mathrm{F_r})\frac{J_1(\pi \rho \Theta_\mathrm{UD} )}{\pi  \rho \Theta_{\mathrm{UD}}}\,,$$

where $\rho = \sqrt{u^2+ v^2}$, $u$ and $v$ are the spatial frequencies sampled by the interferometric observations, $J_1$ is the first order Bessel function, $\Theta_\mathrm{UD}$ and $F_r$ are the angular diameter of the uniform disk profile and a scaling factor that accounts for the over-resolved flux in the observations, respectively. To fit the data, we used a Monte-Carlo Markov-Chain (MCMC) algorithm based on the ```Python``` package ```emcee``` (Foreman et al., [2013](https://iopscience.iop.org/article/10.1086/670067/pdf)). We let 250 walkers evolve for 150 steps using the data of each spectral bin independently. 

##### Add here a screenshot of the code corresponding to the MCMC method

##### Single-layer model to fit the V^2 across the first and second CO bandheads
The MOLsphere model, used to calculate the size, temperature, and optical thickness of the CO layer consists of a stellar disk with a compact layer around it.  The star is modeled by a stellar surface of radius $R_\*$ which emits as a black-body at a temperature $\mathrm{T_{\*}}$. It is surrounded by a compact spherical layer of radius $\mathrm{R_{L}}$ that absorbs the radiation emitted by the star and re-emits it like a black-body. The MOLsphere is characterized by its temperature $\mathrm{T_{L}}$, radius $\mathrm{R_L}$ and its optical depth $\tau_{\lambda}$. The region between the stellar photosphere and the layer is assumed to be empty (See the figure for a skecth of the model). The analytical expression of the model is given by:

![image](https://user-images.githubusercontent.com/61716000/211214093-77fe21b5-fe47-4bdb-8e93-bb1935bec40a.png)

![image](https://user-images.githubusercontent.com/61716000/211214604-cc6b37b3-7c0d-4e92-9ebe-5dc5081f5d54.png)

where  $I_\lambda^r = I_{\lambda}^r(\mathrm{T_{\*}} , \mathrm{T_L}, \mathrm{R_\*}, \mathrm{R_L}, \tau_{\lambda})$,  $\mathrm{T_\*}$ and $\mathrm{T_L}$ are the temperatures of the photosphere and of the CO layer, respectively; $\mathrm{R_*}$ and $\mathrm{R_L}$ are the angular radius of the star and the layer, respectively; $\tau_\lambda$ is the optical depth of the molecular layer at wavelength $\lambda$; $B_\lambda(\mathrm{T})$ is the Planck function (at wavelength $\lambda$ and temperature T); and $\beta$ is the angle between the radius vector and the line-of-sight so that $\cos\beta = \sqrt{1-(\mathrm{r}/\mathrm{R_L})^2}$.

For the fitting, we adopted a $\mathrm{T_{\*}}$=2800 K and $\mathrm{R_{\*}}$=5 mas \citep{monnier20142014,mcdonald2012fundamental}. We use the disk estimation reported in the H- band because it is considerably less affected from molecular contribution, in contrast with the K-band where  molecules like CO, $H_{2}O$, and OH among others \citep[see e.g.,][]{wittkowski2018vlti, paladini2011interferometry} are present over the entire band. The unknown parameters are then $\mathrm{T_L}$, $\mathrm{R_L}$ and $\tau_L$. We performed a two-step process to estimate these parameters. As first step, for each pair of $\mathrm{T_L}$ and $\mathrm{R_L}$, we performed a least-squares minimization to find the best-fit value of $\tau_L$ that reproduces the spectrum $F_{\lambda}^{\mathrm{R Car}}$. The next step consisted of using an MCMC method based on the ```Python``` library ```emcee``` to estimate the best combinations of $\mathrm{T_L}$, $\mathrm{R_L}$, and $\tau_L$ that reproduce the $V^2$. 
 
To account for the over-resolved flux when modeling the $V^2$ data, we added a scaling factor Fr. This value was estimated simultaneously with the other parameters in the model. 

##### Bootstrapp method for image reconstruction
To build new data samples from the original one we employed a bootstrap algorithm. The samples were created by keeping the original number of data points unaltered, but allowing random sampling with replacement. This created data sets with points with higher weights than in the original data set and, some others with zero weights. This algorithm also produces slight changes in the u-v plane, which allows us to trace the impact of the u-v coverage on the reconstructed images, without loosing the statistical significance of the original number of data points. According to \cite{babu1983inference}, the statistical moments such as mean, variance and standard deviation of the bootstrapped samples are good approximations to the statistical moments of the original data sets.