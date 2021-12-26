# DeepCarbon 
Herein lies a high-level overview of my most relevant work in detecting neutral-carbon absorption lines with machine learning. More detail in the full paper.
## Introduction ## 
Quasars are among the brightest and farthest known objects in the universe, emitting light that passes
through multiple galaxies and travels for several billion years before reaching Earth. Different types of
atoms within these galaxies absorb different wavelengths of light, giving rise to visual dips at such wavelengths—we call them absorption lines—in a distribution of quasar light received on Earth (a spectrum). Using these absorption lines, we can deduce what the intervening galaxies were composed of billions of years ago—essentially looking back in time!

Distant galaxies containing neutral-carbon, in particular, are useful because their properties help us investigate star formation in the early universe. However, traditional computational methods for detecting absorption lines are extremely time-consuming and require frequent human intervention. Neutral-carbon lines are especially rare, weak, and difficult to detect at exceptionally far distances. So, my research develops a convolutional neural network (CNN) to detect neutral-carbon absorption lines in the Sloan Digital Sky Survey Data Release 12 (SDSS DR12) quasar spectra dataset.

## Methodology ##

SDSS DR12 contains a sample of over 500,000 quasar spectra to be searched for neutral-carbon absorption lines. Each spectrum contains thousands of flux data points over a ~7,000 Å wavelength range. Using traditional (non-DL) methods, I first narrowed down a sample of ~40,000 spectra down to 113 candidates containing the neutral-carbon lines I sought (this process involved lots of funky data wrangling, done in Python—more on that in the paper). 


Here's an example of neutral-carbon detection: its absorption appears via two weak lines, whose wavelengths are marked by the two dotted blue lines:

<p align="center">
<img src="https://user-images.githubusercontent.com/20466488/147395188-38fa7ad8-2446-4fb5-bc96-ad0194ec1084.png" height="350">
</p>

With only 113 of these, I didn't have nearly enough training samples for my CNN. The solution? Genenating artificial samples!

### Generating Synthetic Training Data ###

I randomly selected 20,000 spectra from a quasar spectra dataset within SDSS DR12. After some hefty pre-processing (normalizing the data, fitting the data points to a continuum, generating artificial noise, etc.), I was ready for the most important part: actually inserting synthetic neutral-carbon absorption lines into the spectra. In what became the 10,000 positive samples, I inserted two absorption lines whose properties mimiced that of real neutral-carbon lines. I left the other 10,000 alone, which served as the negative sample. An additional 4,000 samples (2,000 positive, 2,000 negative) served as testing data.

### Data Preprocessing ###

Spectral data is one-dimensional, with the only indicator of absorption being a steep decrease and increase in flux density. Unfortunately, neutral-carbon absorption lines are very weak, and SDSS data is quite noisy. So, I couldn't just feed in the entire spectrum, or even a smaller, continuous window similar to what's shown in the image above—there'd be too much going on for the CNN to distinguish neutral-carbon absorption lines from all the noise and other lines. So, after multiple stages of experimenting, I decided to concatenate two pieces of the spectrum together: one with data points extremely close to where the left absorption line would be, and one with data points extremely close to where the right would be. By masking out irrelevant data, the CNN no longer had to distinguish the two neutral-carbon lines from other absorption lines in between or close to them on each spectrum, thereby simplifying the training process greatly:

<p align="center">
<img src="https://user-images.githubusercontent.com/20466488/147396089-72180842-8086-4d2b-81e1-a4832fb98717.PNG" height="575">
</p>

The top left plot shows a normalized spectrum with artificially inserted noise and artificially inserted neutral-carbon lines. The bottom left plot shows the normalized spectrum for a real detected sample. However, since the final preprocessing step only keeps and concatenates the two pieces enclosed by the vertical red lines (33 data points per piece), the CNN no longer has to distinguish neutral-carbon lines from other lines. So, the final data forms of the artificial and real samples, shown on the top and bottom right respectively, are quite similar to each other! This data preprocessing algorithm proves useful in exposing weak spectral features.

### The CNN & Results ###

I fed the 20,000 samples into a CNN implemented in Keras. As I improved my data preprocessing pipeline and reduced the data's dimensionality over time, I had to greatly simplify my CNN architecture to accomodate for the changes. Standard CNN architectures like AlexNet, GoogLeNet, ResNet, and VGGNet worked when I fed in uncropped spectra, but became far too complex once I started preprocessing my data in the way described previously—which generated a mere 66 data points per sample. So, I developed my own final CNN architecture.

The final CNN model reached a training accuracy of 99.82% and achieved a testing accuracy of 96.30% on the artificially generated samples. Out of the 113 real neutral-carbon samples, the CNN successfully redetected 105 (92.92%) of them in under one second—not bad, considering that traditional methods would've taken far, far longer to do the same!
