# DeepCarbon 
Herein lies a high-level overview of my most relevant work in detecting neutral-carbon absorption lines with deep learning. Check out the full paper for more detail!

## Introduction ## 
Quasars are among the brightest and farthest known objects in the universe, emitting light that passes
through multiple galaxies and travels for several billion years before reaching Earth. Different types of
atoms within these galaxies absorb different wavelengths of light, giving rise to visual dips at such wavelengths—we call them absorption lines—in a distribution of quasar light received on Earth (a spectrum). Using these absorption lines, we can deduce what the intervening galaxies were composed of billions of years ago—essentially looking back in time!



Distant galaxies containing neutral-carbon, in particular, are useful because their properties help us investigate star formation in the early universe. However, traditional computational methods for detecting absorption lines are extremely time-consuming and require frequent human intervention. Neutral-carbon lines in particular are rare, weak, and difficult to detect at exceptionally far distances. So, my research develops a convolutional neural network to detect neutral-carbon absorption lines in the Sloan Digital Sky Survey Data Release 12 (SDSS DR12) quasar spectra dataset.

## Methodology ##
SDSS DR12 contains a sample of >500,000 quasar spectra to be searched for neutral-carbon absorption lines.



