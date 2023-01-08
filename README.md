# RCar_GRAVITY
This project includes a Jupyter notebook with the codes used to obtain the results reported in Rosales-Guzman et al. for the M-type AGB star R Car. 

To obtain the angular size of R Car across the K-band, we applied a geometrical model of a uniform disk (UD) to the V2 data. The visibility function of our models is given by the following equation (Berger & Segransan 2007):

$$   V_{\mathrm{UD}}(u,v) = 2(\mathrm{F_r})\frac{J_1(\pi \rho \Theta_\mathrm{UD} )}{\pi  \rho \Theta_{\mathrm{UD}}}\,,$$

where $\rho = \sqrt{u^2+ v^2}$, $u$ and $v$ are the spatial frequencies sampled by the interferometric observations, $J_1$ is the first order Bessel function, $\Theta_\mathrm{UD}$ and $F_r$ are the angular diameter of the uniform disk profile and a scaling factor that accounts for the over-resolved flux in the observations, respectively. To fit the data, we used a Monte-Carlo Markov-Chain (MCMC) algorithm based on the ```Python``` package ```emcee``` \citep{foreman2013emcee}. We let 250 walkers evolve for 150 steps using the data of each spectral bin independently. 

##### Add here a screenshot of the code corresponding to the MCMC method

The MOLsphere model, used to calculate the size, temperature, and optical thickness of the CO layer consists of a stellar disk with a compact layer around it.  The star is modeled by a stellar surface of radius $R_\*$ which emits as a black-body at a temperature $\mathrm{T_{\*}}$. It is surrounded by a compact spherical layer of radius $\mathrm{R_{L}}$ that absorbs the radiation emitted by the star and re-emits it like a black-body. The MOLsphere is characterized by its temperature $\mathrm{T_{L}}$, radius $\mathrm{R_L}$ and its optical depth $\tau_{\lambda}$. The region between the stellar photosphere and the layer is assumed to be empty
