# SDExplain

Use diffusion models to provide better human interpretable explanations for image classification.

Based on https://github.com/fastai/diffusion-nbs/blob/master/Stable%20Diffusion%20Deep%20Dive.ipynb

Method:
1. Take image to be classified and add noise to the image according to diffusion model schedule (resulting in an array of e.g. 50 noised images)
2. For the labels to be compared, perform noise prediction on each of the noised images from 1. using the classifier labels as prompts (e.g. “zebra” and “horse” prompts). Alternatively, use the target label and the empty prompt.
3. Keep track of the noise predictions for each noise image (but do not apply the noise predictions to the images themselves as in normal diffusion process). The end result is two arrays of noise predictions for the two prompts.
4. Create the noise difference array by subtracting the noise predictions from 3. Sum the values of this array for different ranges (e.g. 0-50 or 10-20). 
5. Visualise the noise differences



TODO:
1. Instead of using the Stable Diffusion pretrained model, train a diffusion model on the specific dataset that the classifier was trained on. Compare results with other methods (e.g. Gradcam, SHAP).

