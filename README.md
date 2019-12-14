# AddColour
This is a demonstration of colouring black and white images using a slightly modified Generative Adversarial Network (GAN). The training is done on a specifically curated dataset which is sampled from the [Oxford-IIIT Pet Dataset](https://www.robots.ox.ac.uk/~vgg/data/pets/) using [fastai](https://github.com/fastai).
<p align="center">
  <img width="460" height="300" src="https://github.com/piyushmishra12/Colourise-Image-GAN/blob/master/Screenshot%202019-12-10%20at%208.53.18%20PM.png">
</p>

### Workflow
Since the model must learn to add colour to greyscale images, the pet dataset is sampled and each image is greyscaled and put into a separate folder. The greyscaled images are stored by mapping to their corresponding RGB counterparts to ensure cohesiveness and understandability.

#### Generator
These greyscaled images are put into a *generator* and subsequently taught to generate their RGB counterparts. This is achieved by using a [U-net](https://link.springer.com/chapter/10.1007/978-3-319-24574-4_28) for the generator. In a U-net, besides the normal downsampling (encoding) and upsampling (decoding) stages, there are skip-connections that help the learner keep in touch with the input vectors. 

<p align="center">
  <img width="460" height="300" src="https://github.com/piyushmishra12/Colourise-Image-GAN/blob/master/unet.png">
</p>

The reconstructed images are compared with the ground truths with the help of the mean squared error loss function and back-propagated appropriately. In 2 epochs (computation cost of around 5 minutes), a validation loss of 0.096118 is achieved. A low computation cost is achieved because of [transfer learning](https://machinelearningmastery.com/transfer-learning-for-deep-learning/) from the [Imagenet Dataset](http://www.image-net.org). Hence, a pre-trained generator is ready for future use.

#### Critic
In theory, a critic or a descriminator should possess the ability to tell the model that a certain image is either generated from the generator above or simply a ground truth (the original high-resolution image). In practice, one can think of it simply being a classifier. So, the generated images are saved in a separate folder and the critic is nothing but a binary classifier that learns whether an image is the output of a generator or simply an RGB image from the dataset. This is easily achieved by using binary crossentropy loss from the [PyTorch](https://github.com/pytorch/pytorch) library. The critic learns for 6 epochs (roughly under 23 minutes) and achieves a validation accuracy of 99.85%. This is understandable since at this point it is fairly easy to differentiate between the generated and ground truth images.

#### GAN for Dummies (like me)
A GAN is simply an interconnection of the generator and the critic. The generator generates images and the critic criticises the generator by telling it that it is not doing a good job of generating the images. So, it would be ideal if the model seeks to fool the critic, i.e., the model is doing a good job if the critic is getting confused between the generated and actual images. This can be achieved if the generator and the critic are left to interact with each other. This is done for 10 epochs which roughly takes 90 minutes. The results are as below. The first column indicates the greyscale input images, the second column indicates the images that are generated by this GAN, and the third column indicates the ground truth images.

<p align="center">
  <img width="460" height="300" src="https://github.com/piyushmishra12/Colourise-Image-GAN/blob/master/result.png">
</p>

### GAN-ish
This isn't a proper GAN. In a GAN, the generator and the critic enter into the model without any prior training. So the two entities learn everything while interacting with each other. Here, the generator and the critic are created initially as two separate entities; they first learn independently and then they are put into the model to interact with each other. This is to simulate transfer learning by making a pretrained generator and critic. This helps in time efficiency while training the GAN since in a normal GAN, it takes a lot of initial time for the model to learn basic features, after which it picks up the pace [(refer here)](https://www.jstage.jst.go.jp/article/pjsai/JSAI2017/0/JSAI2017_1A32/_pdf/-char/en).

### Future Scope
From the set of results, the model has learnt to identify greens and browns, but still struggles with reds and blues. This can potentially be achieved by further interaction between the generator and the critic, so that the critic further criticises the generator.
