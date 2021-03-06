|   |![alt-text-2](demo/source_0.png "source")|![alt-text-4](demo/source_1.png "source")|
|:-:|:-:|:-:|
|![alt-text-1](demo/target.gif "target")|![alt-text-3](demo/result_0.gif "result")|![alt-text-5](demo/result_1.gif "result")|

# Controllable Face Generation via Conditional Adversarial Latent Autoencoder (ALAE)

**Authors**: Grigorii Sotnikov, Vladimir Gogoryan, Dmitry Smorchkov and Ivan Vovk (all have equal contribution)

The work has been done as the Deep Learning Course final project "*Controllable Face Generation via Conditional Latent Models*" at Skoltech. You can check the report and details in `demo/paper.pdf`.

Checkout our video presentation on YouTube: [link](https://youtu.be/af2zd35FlGs). 

## Info

![alt-text-6](demo/talking-heads-transfer.png "talking-heads-transfer")

This master branch contains the solution for facial keypoints transfer (check another branches for other types of conditions and approaches) based on publicly available pretrained on CelebA128 generative model Adversarial Latent Autoencoder (ALAE, https://github.com/podgorskiy/ALAE). The solution is simple and just manipulates the latent codes of images using a small mapping MLP network. Generally, the whole pose-transfer architecture is trained adversarially with a special MLP critic network just on the latent codes of ALAE.

It is worth to mention, that ALAE has a huge identity gap during real image restoration. So, if you encode the image and then decode it with the generator, than you'll get a similar high-end quality picture, but the human identity will be not the same. In order to overcome it, we parametrically finetune ALAE's generator on a new face using perceptual VGG19 loss. We succeeded in minimizing the identity gap, but the model experiences problems with image restoration quality, so one thing to improve is to pay more attention for careful finetunning. During experiments we captured the dependence between the detailed (including eyes, mouth, etc.) restoration quality and the quality of keypoints transfer.

## Reproducibility

![alt-text-7](demo/talking-heads.png "talking-heads-inference")

### Inference and training

First of all, make sure you have installed all the python packages by `pip install -r requirements.txt`. To try the solution we prepared `notebooks` folder, where we provided finetunning and inference codes.

Single face finetunning is done in the notebook `notebooks/one_guy_finetunning.ipynb`. To train the facial keypoints transfer model you should set params in and run the bash file `sh train_talking_heads.sh`.

### Data and trained models

The whole model is trained on the single speaker (id00061) from VoxCeleb2 dataset. Dataset is available at [this](https://drive.google.com/drive/folders/1T26YUSpa1RqU9mhgQhJj9M5jA3nDfZoV?usp=sharing) link. Here you can also find the pretrained model `face_rotation_model.pt`. Put it in `checkpts` folder and run necessary notebooks.

## Our ALAE implementation and another latent attributes manipulation experiments

Switch the branch for another experiments. We implemented our own ALAE solution and successfully trained it up to 64x64 resolution on CelebA128 dataset. You can find it in the branch `alae-implementation`. Furthermore, instead of facial keypoints we tried to condition the latent space mapping network with other CelebA class attributes. This experiments were done with ACAI-based (https://arxiv.org/pdf/1807.07543.pdf) encoder-decoder network. You can find in the proper branch also.
