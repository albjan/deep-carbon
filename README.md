# DeepCarbon 
Herein lies a high-level overview of my most relevant work in detecting neutral-carbon absorption lines with deep learning. Check out the full paper for more detail!

## Introduction ## 
Quasars are among the brightest and farthest known objects in the universe, emitting light that passes
through multiple galaxies and travels for several billion years before reaching Earth. Different types of
atoms within these galaxies absorb different wavelengths of light, giving rise to visual dips at such wavelengths—we call them absorption lines—in a distribution of quasar light received on Earth (a spectrum). Using these absorption lines, we can deduce what the intervening galaxies were composed of billions of years ago—essentially looking back in time!

Distant galaxies containing neutral-carbon, in particular, are useful because their properties help us investigate star formation in the early universe. However, traditional computational methods for detecting absorption lines are extremely time-consuming and require frequent human intervention. Neutral-carbon lines in particular are rare, weak, and difficult to detect at exceptionally far distances. So, my research develops a convolutional neural network (CNN) to detect neutral-carbon absorption lines in the Sloan Digital Sky Survey Data Release 12 (SDSS DR12) quasar spectra dataset.

## Methodology ##

SDSS DR12 contains a sample of over 500,000 quasar spectra to be searched for neutral-carbon absorption lines. Each spectrum contains thousands of flux data points over a ~7,000 Å wavelength range. Using traditional (non-DL) methods, I first narrowed down a sample of ~40,000 spectra down to 113 neutral-carbon candidates (this process involved lots of funky data wrangling, done in Python—more on that in the paper). 


Here's an example of neutral-carbon detection: its absorption appears via two weak lines, whose wavelengths are marked by the two dotted blue lines:

<p align="center">
<img src="https://user-images.githubusercontent.com/20466488/147395188-38fa7ad8-2446-4fb5-bc96-ad0194ec1084.png" height="450">
</p>

With only 113 of these, I don't have nearly enough training samples for my CNN. The solution? Genenating artificial samples!

### Generating Synthetic Training Data ###

I randomly selected 20,000 spectra from a quasar spectra dataset within SDSS DR12. After some hefty pre-processing (normalizing the data, fitting the data points to a continuum, etc.), I was ready for the important part: actually inserting artficial neutral-carbon absorption lines. In 10,000 samples, I randomly inserted two absorption lines that mimiced the properties of neutral-carbon's lines. I left the other 10,000 alone. An additional 4,000 samples (2,000 positive, 2,000 negative) served as training data. 

### Data Preprocessing ###

Spectral data is one-dimensional, with the only indicator of absorption being a steep decrease and increase in flux density. Unfortunately, neutral-carbon absorption lines are very weak, and the data is quite noisy. So, I couldn't just feed in the entire spectrum, or even a smaller, continuous window similar to what's shown in the image above—there'd be too much going on for the CNN to distinguish neutral-carbon absorption lines from all the noise and other lines. So, after multiple stages of experimenting, I decided to concatenate two pieces of the spectrum together: one with data points extremely close to where the left absorption line would be, and one with data points extremely close to where the right would be. By masking out irrelevant data, the CNN no longer had to distinguish the two neutral-carbon lines from other absorption lines in between or close to them on each spectrum, thereby simplifying the training process greatly.

![absorber2](https://user-images.githubusercontent.com/20466488/147396089-72180842-8086-4d2b-81e1-a4832fb98717.PNG)


