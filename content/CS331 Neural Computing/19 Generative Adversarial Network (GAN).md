A GAN is a **deep learning model** used to **generate** **new data** that *resembles* a training dataset
- New Image generation (faces)
- Super resolution (upscaling)

*Network*:
A GAN needs to train **two neural networks**:
- A **Generator** (G)
- A **Discriminator** (D)

*Adversarial*:
The two networks are **competing** with each other (adversarial)
- One **maximises** the loss function, while the other **minimises**

*Generative*:
A GAN generates **new samples** similar to the data

For the **generative model**:
- The main purpose is to **learn how the data is generated**
	- Joint probability $P(X,Y)$ of feature $X$ and label $Y$
- Can generate new samples that resemble the training data
	- Naive bayes
	- Variational Autoencoders
	- GANs

For the **discriminative model**
- Main purpose is to **find the decision boundary** for a problem
	- Learn the **conditional probability** $P(Y|X)$ or mapping $X\rightarrow Y$
- Focus on **how to classify** or **predict** *labels*
	- Logistic Regression
	- SVM
	- kNN

**Generator** (G)
Input of the generator is typically a **noise vector** 
- *(random seed)*
Output: Generates **synthetic data**
Goal: *Fool* the **discriminator** (D) into classifying fake data as real

Why do we use **random noise**?
- To increase the **diversity** of the generated data

**Discriminator** (D)
Tries to **distinguish** the real data from fake data
Input: *real or fake* data
Output: *probability* indicating how likely the input is **real**
Goal: Accurately **distinguish** the real data from the fake data

From the **discriminator**'s perspective, it wants to be able to **distinguish** between real and fake data
However, from the **generator**'s perspective, it wants to produce data which is **indistinguishable** from real data


GAN can be thought of as a **competing game** between the two networks

- Initially, generator produces synthetic data at a very poor quality, since we just accept random noise and there is no other data
- This poor quality can be detected by the discriminator as fake
- Generator wants to improve itself, so after some training it learns which features it should add in order to fool the discriminator
- As the generator gets better, the discriminator gets better at identifying which data is real or fake
- In the end both get better at their jobs and we get a very good generator as a result

Next: Architecture of GAN
13

