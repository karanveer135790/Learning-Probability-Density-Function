# Learning Probability Density Function using GAN

**Feature Used:** NO2 Concentration  
**Roll Number:** 102303670  

---

## 1. Objective

The objective of this assignment is to learn the unknown probability density function (PDF) of a transformed random variable using a Generative Adversarial Network (GAN).

The GAN must learn the distribution only from data samples without assuming any analytical or parametric form of the PDF.

---

## 2. Transformation Parameters

The transformed variable is defined as:

z = x + a_r * sin(b_r * x)

Where:

a_r = 0.5 * (r mod 7)  
b_r = 0.3 * (r mod 5 + 1)

For roll number 102303670:

r mod 7 = 0  
r mod 5 = 0  

Therefore:

a_r = 0  
b_r = 0.3  

Thus, the transformation simplifies to:

z = x

So the transformed variable is the original NO2 concentration itself.

---

## 3. Data Preprocessing

The following preprocessing steps were performed:

- Extracted the NO2 column from the dataset.
- Removed missing and invalid values.
- Removed extreme outliers using percentile clipping (1st–99th percentile) to improve training stability.
- Standardized the data using StandardScaler to achieve:
  - Mean approximately 0  
  - Standard deviation approximately 1  

Standardization improves GAN training stability and prevents mode collapse.

---

## 4. GAN Architecture

### Generator

- Input: 100-dimensional Gaussian noise vector (N(0,1))
- Dense layer (64 neurons, ReLU activation)
- Dense layer (64 neurons, ReLU activation)
- Output layer (1 neuron, Linear activation)

The generator maps random noise to synthetic samples of z.

---

### Discriminator

- Dense layer (64 neurons)
- LeakyReLU activation (alpha = 0.2)
- Dense layer (64 neurons)
- LeakyReLU activation (alpha = 0.2)
- Output layer (1 neuron, Sigmoid activation)

The discriminator distinguishes between:
- Real samples (from dataset)
- Fake samples (from generator)

---

## 5. Training Details

- Loss Function: Binary Cross-Entropy
- Optimizer: Adam
- Learning Rate: 0.0001
- Batch Size: 128
- Epochs: 2000

Training was performed by alternating between:
1. Updating the discriminator using real and fake samples.
2. Updating the generator to fool the discriminator.

---

## 6. PDF Estimation

After training:

- 10,000 samples were generated using the trained generator.
- Kernel Density Estimation (KDE) was applied to approximate the probability density function.
- The generated distribution was compared with the real distribution.

---

## 7. Observations

### Mode Coverage

The GAN captured the main mode of the NO2 distribution. The generated samples show similar central tendency and spread compared to the real data.

### Training Stability

Initial oscillations were observed in discriminator and generator losses. After several epochs, the training stabilized and both networks reached equilibrium.

### Quality of Generated Distribution

The generated samples closely match the real data in terms of:
- Mean  
- Standard deviation  
- Overall distribution shape  

Minor deviations are observed in extreme tail regions, which is expected in generative modeling tasks.

---

## 8. Conclusion

A Generative Adversarial Network was successfully trained to learn the unknown probability density function of the transformed NO2 concentration variable.

The model learned the distribution directly from data samples without assuming any parametric PDF.

The KDE-based density estimation confirms that the GAN provides a reasonable approximation of the true underlying distribution.

---

## 9. Files Included

- Colab notebook containing GAN implementation  
- Real vs Generated distribution plot  
- Estimated PDF plot  
