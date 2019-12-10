# Colourise-Image-GAN
This is a demonstration of colourising black and white images using a slightly modified Generative Adversarial Network (GAN). The training is done on a specifically curated dataset which is sampled from the [Oxford-IIIT Pet Dataset](https://www.robots.ox.ac.uk/~vgg/data/pets/) using [fastai](https://github.com/fastai).
<p align="center">
  <img width="460" height="300" src="https://github.com/piyushmishra12/Colourise-Image-GAN/blob/master/Screenshot%202019-12-10%20at%208.53.18%20PM.png">
</p>

### Workflow
Since the model must learn to colourise greyscale images, the pet dataset is sampled and each image is greyscaled and put into a separate folder. The greyscaled images are stored by mapping to their corresponding RGB counterparts to ensure cohesiveness and understandability.

These greyscaled images are put into a *generator* and subsequently taught to generate their RGB counterparts. This is achieved by using a [U-net](https://link.springer.com/chapter/10.1007/978-3-319-24574-4_28) for the generator. In a U-net, besides the normal downsampling (encoding) and upsampling (decoding) stages, there are skip-connections that help the learner keep in touch with the input vectors. 
