# star-formation-pandeia
SFR Density Data Corrections

The four notebooks provided all contribute to the results in "Capstone URF Poster_ Rahul Shaji.pdf". The goal of this project was to measure the flux of H-alpha emission lines in high-redshift galaxies, and use these as tracers of star formation to calculate the star formation rate density of galaxies in our early Universe. 

## rubies_emcee_fitting.ipynb
The first notebook reads in the RUBIES catalog of H-alpha detected galaxies and runs them through an MCMC Gaussian fitting pipeline called emcee. emcee allows walkers to explore a parameter space, and the median parameter are used as Gaussian parameters. These parameters are used to integrate the area under the curve, which serve as the integrated fluxes. These are converted into luminosities, and luminosity functions (LFs) are created by plotting the number density of objects per unit volume at different luminosity values (for varying redshifts.) These LFs are corrected in the remaining notebooks.

## pandeia_ld_correctiion.ipynb
The second notebook simulates galactic spectra using the JWST ETC Pandeia engine, establishing instrumental specifications and iterating across all luminosity and redshift values. These simulations can then be run through as many noise iterations as needed. The animation in "Pandeia Simulation Animation.mp4" comes from this notebook, displaying a single noise iteration on all luminosity bins within one redshift bin. The line detection completeness corrections account for the noise preventing our emcee pipeline in the previous notebook from fitting every H-alpha to a Gaussian properly (with SNR>5).

## obs_correction.ipynb
The third notebook crossmatches data between UNICORN photometric data and RUBIES spectroscopic data to find the matches at different redshift and magnitude bins. Magnitudes are calculated using a redshift-dependant table of flux filters. After cross-matching, observational completeness corrections are calculated, and weights are applied to each object in the RUBIES spectra to account for the sample selection of NIRSpec. 

## sfrd_correction.ipynb
This final notebook ties everything together. First, by running all the simulated Pandeia spectra through the same emcee pipeline, we calcualted line detection completeness. Then, by applying the weights calculated in the previous notebook, we account for observational incompleteness as well. Creating LFs for the raw, obs_corrected, and full corrected values shows the effect of each data correction. Square root errors account for the uncertainty, and volume calculations are done using Astropy.cosmology library values. All LFs are fit to Schechter curves using another emcee pipeline, and are integrated to calculate SFRD across redshifts. 
