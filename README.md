# SDExplain

Inspired by J wittakers notebook and this line from this blog:

[1] 

I was curious as to whether diffusion models could provide better human interpretable explanations for classification results than existing methods.


For example, suppose we had a horse vs zebra classifier and we wanted to know what parts of the image were responsible for the classifiers prediction:

Original image:

Explained by:


[heatmap version, sum of noise from steps 10-20]

The stripes seem to be the most important feature for explaining the prediction.

Rat vs mouse:
Original image


Explained by:

[Non-heatmap version sum of noise from steps 30-40]

Ear, nose and eyes appear to be important, the main part of the body is not.

Sports car vs taxi:

Original image:


Explained by:


[Heatmap version sum of noise from steps 20-30]

The spoiler on the back of the car seems to have been highlighted as important, as well as lights and possibly wheel trims. Note the “negative” highlight box on the top of the car. Could this be indicating that the lack of the TAXI “roof light” is a reason why the image is not a taxi?
Indeed, if we look at the comparison with the empty/unguided prompt (see 3. In Method below), this box disappears:



Method:
Take image to be classified and add noise to the image according to diffusion model schedule (resulting in an array of e.g. 50 noised images)
For the labels to be compared, perform noise prediction on each of the noised images from 1. using the classifier labels as prompts (e.g. “zebra” and “horse” prompts). Alternatively, use the target label and the empty prompt.
Keep track of the noise predictions for each noise image (but do not apply the noise predictions to the images themselves as in normal diffusion process). The end result is two arrays of noise predictions for the two prompts.
Create the noise difference array by subtracting the noise predictions from 3. Sum the values of this array for different ranges (e.g. 0-50 or 10-20). This will give a latent variable which can then be passed through the VAE for the end results shown above. 





Future directions:
1. Instead of using the Stable Diffusion pretrained model, train a diffusion model on the specific dataset that the classifier was trained on. Compare results with other methods (e.g. Gradcam, SHAP).
