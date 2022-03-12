# ML and Coding Resources

## PyTorch tutorial by Eric Elmoznino

PyTorch is an easy-to-use deep learning framework for Python. Here are instructions on getting the tutorial up and running.

1. Open your command prompt
2. Navigate to the folder you want to put the project in (e.g. `cd ~/Desktop`)
3. Pull the code from GitHub `git clone https://github.com/BonnerLab/PyTorch_Tutorial`
4. Navigate into the project folder `cd PyTorch_Tutorial`.
5. Switch to the branch used for the lab tutorial `git fetch & git checkout lab_workshop`
6. Read the project instructions in the README.md for the rest https://github.com/BonnerLab/PyTorch_Tutorial/tree/lab_workshop

## Object2Vec Encoder training/inference with PyTorch code by Eric Elmoznino

Train fMRI encoders on the Object2Vec dataset using Python and PyTorch. See the project here https://github.com/BonnerLab/object2vec_encoder_python

## Beta-VAE training and fMRI analysis code by Eric Elmoznino

Variational-Auto-Encoders (VAEs) are useful for extracting latent dimensions of data in an unsupervised way by leveraging a reconstruction task and variational objective. Beta-VAEs add an additional parameter to the training objective in order to encourage the latent dimensions to be disentangled, such that they represent individual concepts and are more interpretable. Eric has made a repository for training such Beta-VAEs on arbitrary datasets (e.g. a folder of images) and comparing the representations learned by the trained model to those encoded by fMRI voxels. See the project here https://github.com/BonnerLab/bvae_reconstruction_fmri

## Blender room generation code by Eric Elmoznino

Eric has written a repository that can procedurally generate specifications for simple room environments in order to create large synthetic datasets. The room specifications can vary according to floor plan, surface texture, object placement, and lighting. The repository also contains code to automatically build  these scenes in Blender and render images from them at specified viewpoints. Most of the code should be easy to understand and easy to extend for your specific purposes. See the project here https://github.com/BonnerLab/scene_generator

## GAN stimulus generation by Eric Elmoznino

Code to visualize stimuli that would produce a target response in some computational model (e.g. encoder fit to fMRI voxels), constrained to a natural image manifold specified by a GAN. See the project here https://github.com/BonnerLab/gan_stimulus_generation
