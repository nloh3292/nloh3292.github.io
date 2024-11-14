---
layout: page
title: Wildfire Detection 
description: By Matiss M Mednis, Nathan Loh, and Nikolaus Schultze
img: assets/img/project2/wildfire_cover.jpg
importance: 3
category: Academic
giscus_comments: false
---

<h2 style="font-size: 1.7em; font-weight: normal;">Project Description</h2>
Wildfires threaten ecosystems and humanity, spreading rapidly and causing significant damage. The recent Canadian wildfires highlight their global impact. Prevention and early detection are crucial to reducing the risk of uncontrollable fires. Implementing automated monitoring methods such as infrared cameras are affordable and accessible, offering the potential for early detection when fires are still manageable.

Today’s wildfire detection faces many challenges, as natural events like clouds, mists, and sunsets may mimic or distort wildfire characteristics and lead to false positives. Alternative approaches, such as satellite or infrared monitoring, provide benefits, with images covering larger areas and bypassing minor weather effects. As such, developing accurate models for early detection can significantly aid in preserving ecosystems, properties, and lives.


<h2 style="font-size: 1.7em; font-weight: normal; margin-top: 25px;">Data Set Description</h2>
The data set, <a href="https://www.kaggle.com/datasets/elmadafri/the-wildfire-dataset">The Wildfire Dataset</a>, is comprised of 2,700 aerial and ground-based images from various sources, including government databases, Flickr, and Unsplash. These images cover various environmental conditions, forest types, geographical locations, and the intricate dynamics of forest ecosystems and fire events. The main directories can be found in this data set: Training (70%), Validation (15%), and Testing (15%). In addition, two folders can be found for each directory: “nofire” folders containing 1653 images without fire and “fire” folders containing 1047 images with fire. The data set also includes images of smoke with and without fire to improve the model. 


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/project2/fire_with_smoke.jpg" title="Fire With Visible Smoke" class="img-fluid rounded z-depth-1" %}
        <div class="sub-caption" style="font-style: italic; font-size: 0.75em; text-align: center">
            (a) Fire with visible smoke
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/project2/fire_only_smoke.jpg" title="Smoke No Fire" class="img-fluid rounded z-depth-1" %}
        <div class="sub-caption" style="font-style: italic; font-size: 0.75em; text-align: center">
            (b) Only smoke, no visible flames
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/project2/forest_nofire.jpg" title="No Fire" class="img-fluid rounded z-depth-1" %}
        <div class="sub-caption" style="font-style: italic; font-size: 0.75em; text-align: center">
            (c) Forest scene with no fire
        </div>
    </div>
</div>
<div class="caption" style="font-style: italic; font-size: 0.85em;">
    Figure 1. This set of images demonstrates different wildfire scenarios: fire with smoke, smoke only, and a forest without fire. Each highlights distinct visual cues for wildfire detection.
</div>


An issue with finding data for this project is the need for available images of wildfires, as they do not often occur. We introduce this problem by implementing a second data set, <a href="https://www.kaggle.com/datasets/phylake1337/fire-dataset">FIRE Dataset</a>, composed of fire images. This way, we can test if our model will improve after adding additional images of fire and whether our original model can accurately classify these images. This new data set consists of images of fire and no fire, however, we will only implement the images with fire. Images of fire can be found in all environments like a forest and a house.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/project2/add_fire1.jpg" title="Additional Fire 1" class="img-fluid rounded z-depth-1" %}
        <div class="sub-caption" style="font-style: italic; font-size: 0.75em; text-align: center">
            (a) Fire on mountain
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/project2/add_fire2.jpg" title="Additional Fire 2" class="img-fluid rounded z-depth-1" %}
        <div class="sub-caption" style="font-style: italic; font-size: 0.75em; text-align: center">
            (b) House fire
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/project2/add_fire3.jpg" title="Additional Fire 3" class="img-fluid rounded z-depth-1" %}
        <div class="sub-caption" style="font-style: italic; font-size: 0.75em; text-align: center">
            (c) Forest fire
        </div>
    </div>
</div>
<div class="caption" style="font-style: italic; font-size: 0.85em;">
    Figure 2. This set of images provides additional examples of fire scenarios to help address the issue of limited fire data.
</div>


<h2 style="font-size: 1.7em; font-weight: normal; margin-top: 25px;">Model Description</h2>
In this project, we utilize and compare CNN architectures, Hugging Face’s ViT model, and a hybrid CNN and transformer architecture to approach wildfire detection in RGB images on the wildfire dataset. We collect and compare all models’ accuracy, F1, recall, and precision metrics. For the ViT and hybrid models, we additionally train them on additional fire dataset images and analyze their final performance on the test set with that additional data in training.


<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px;">I. Simple CNN Model</h3>
The Simple CNN model comprises three convolutional layers, each followed by max-pooling, effectively capturing hierarchical features from the input images. The model contains ReLU activations in the fully connected layers, including a sigmoid activation in the output layer, and is fine-tuned with the Adam optimizer using binary cross entropy as the loss function.


<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px;">II. Complex CNN Model</h3>
The Complex CNN model contains four convolutional layers with progressive max-pooling, incorporating ReLU activations in its fully connected layers. It introduces dropout layers (50% and 30% dropout rates) for regularization, with a sigmoid activation in the output layer. The model is optimized using the Adam optimizer with binary cross entropy as the loss function.


<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px;">III. Pre-trained CNN Model</h3>
The pre-trained CNN model seamlessly utilizes a pre-trained VGG16 with 'imagenet' weights. Allowing the pre-trained layers to remain fixed, a flattened layer connects to a dense layer comprising 128 units with a ReLU activation. The final layer, which tailors the model for binary classification, contains a sigmoid activation—optimized with the Adam optimizer.


<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px;">IV. ViT Model</h3>
The ViT model utilized in this project is derived from <a href="https://huggingface.co/google/vit-base-patch16-224">Google’s Hugging Face ViT-base-path16-224 model</a>. It is pre-trained on ImageNet-21k at a resolution of 224x224. The images used to train are rescaled and transformed accordingly, with different sizes included in the hypertuning section to optimize performance. Twelve images encoded as RGBA (4 channels) were removed due to incompatible data types. Other sets of models are created to incorporate the additional fire images. Each model is trained with 4 epochs, with the evaluation of the validation set every 50 steps.


<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px;">V. Hybrid Model</h3>
The hybrid model is comprised of a CNN and a transformer. This design differs from the ViT model by incorporating a more complex CNN layer, following a similar structure to the <a href="https://huggingface.co/docs/transformers/model_doc/cvt/">Convolutional vision Transformer (CvT)</a> architecture. The components of this structure are a pre-trained CNN model called <a href="https://keras.io/api/applications/efficientnet/">EfficientNetB0</a> and a basic transformer architecture based around a ViT. The transformer section is incorporated with patches of 32 by 32 pixels to capture local features, and multi-head attention to capture the complex relationships between distant image regions. A binary cross-entropy loss function is used as the data set consists of two target classes. A Learning rate scheduler is added to update the learning rate as it trains, allowing the feature weights to converge smoothly. Additional models were developed to accommodate separate datasets, allowing for an evaluation of the effectiveness of training with expanded image sets or testing on new data.


<h2 style="font-size: 1.7em; font-weight: normal; margin-top: 25px;">Model Results</h2>
This section shows the appropriate tables and graphs for the metrics of the models on the test set. Confusion matrices are also included to visualize the effectiveness of the models on different the different testing sets.

<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px; color: #555;">I. Simple CNN</h3>



<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px; color: #555;">II. Complex CNN</h3>



<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px; color: #555;">III. Pre-trained CNN</h3>



<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px; color: #555;">IV. Vision Transformer (ViT)</h3>



<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px; color: #555;">V. Hybrid CNN+Transformer</h3>



<h3 style="font-size: 1.25em; font-weight: normal; margin-top: 20px; margin-left: 20px; color: #555;">Final Comparison of All Models</h3>



