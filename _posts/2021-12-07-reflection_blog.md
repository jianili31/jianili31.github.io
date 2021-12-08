---
layout: post
title: PIC16B Project Reflection Blog
---


## What did we achieve in our project?
We built a face mask classifier that can categorize images of people with vs. without masks. The first model was completely built by us, and the second model had the pre-trained MobileNet V2 model as its base layer. Furthermore, we tested both models on three distinct racial groups: black, white and Asian people. 

## Two aspects of the project that we are especially proud of
1. There is no existing dataset of *real* people wearing masks that is sufficiently racially balanced. So, we created our own dataset with racial diversity in mind by downloading images from Google Images one by one. Fortunately, our effort did translate to better performance of our model on black, white and Asian people. 
2. We did observe some discrepancies in performance on different racial groups even after attempting to balance racial representation in the training dataset. So, we used transfer learning to further address racial bias, and was successful in doing so (prediction accuracy was around 95-96% for all three groups).

## Two things to do to improve our project
1. We are currently limited to classifying **static** images of people wearing/not wearing masks. However, face mask classifiers deployed in real life usually receive real-time, **dynamic** input. So one way to improve our project would be to create an algorithm that has the current model as its base, but can process video input, track people's faces in the video, and display predictions and confidence levels besides the faces.
2. Our classifier cannot distinguish between different types of facial coverings (e.g., surgical masks, N95s, scarves, bandanas). Since masks are more effective than other facial coverings in preventing disease spread, we could train our model to classify images of masks vs. not masks in more granularity. One way to do it could be having more categories of labels corresponding to different types of facial coverings. Of course, the model architecture would also have to be adjusted.

## How does what we achieved compared to what we set out to do in the proposal?
Originally, we planned to create a face mask **detector** that can recognize and differentiate masks from other objects on the same region of the face. However, since OpenCV is out of the scope of PIC16B, we eventually settled on building a face mask **classifier**. Other than that, our final product is pretty similar to what we described in the proposal.

## Three things we learned from the experience of completing our project
1. Team coding. This includes getting familiar with GitHub, writing understanable comments/docstrings, and appreciating the importance of pulling before making changes to the file...
2. Different CNN architectures, including the state-of-the-art ones such as ResNet 50. We surveyed different CNN architectures when deciding which model to use as the base layer in transfer learning. In the end, we settled on MobileNet V2 because someone has fine-tuned it to successfully classify synthetic images of people wearing vs. not wearing masks (https://www.pyimagesearch.com/2020/05/04/covid-19-face-mask-detector-with-opencv-keras-tensorflow-and-deep-learning/). 
3. Standard practices in selecting certain CNN hyperparameters. For example, we read papers that suggest having increasing number of nodes in Conv2D layers as the network gets increasingly deep. This mimicks how the brain processes visual input (i.e., extracting coarse features first, then more refined patterns) and has been shown to enhance model performance. So, our Conv2d layers went from 32 nodes to 256 nodes, and prediction accuracy did improve as a result. 

## How will the experience completing this project help me in my future studies or career?
My research interests are using computational models to better understand and classify mental disorders. Although CNNs may not be especially helpful for my research interests, gaining more knowledge of deep neural networks in general is useful. For example, many researchers in the field of psychiatry are trying to use neural networks to model and/or categorize people's behavior in a data-driven, hypothesis-free way, and then relating them to psychiatric symptoms. The current project, although focused on image classification, definitely made me more confident in constructing deep neural networks on my own. Moreover, the project makes me more aware of ethical issues in programming (e.g., unequal model performance on different racial groups) and ways to attenuate them. I will keep this in mind when applying neural networks in my research.



