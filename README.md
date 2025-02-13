﻿# Pneumonia Detection from Chest-Xray Images
Udacity project for AI in Healthcare - Data from Kaggle

## Project Overview: 

In this project, data will be analyzed from the NIH Chest X-ray Dataset and a CNN will be trained to classify a given chest x-ray for the presence or absence of pneumonia. This project will culminate in a model that can predict the presence of pneumonia with human radiologist-level accuracy that can be prepared for submission to the FDA for 510(k) clearance as software as a medical device. The model will be formally described, as well as the data that it was trained on, and the validation plan that meets FDA criteria.

The medical images with clinical labels for each image that were extracted from their accompanying radiology reports are also provided.

There are in total 112,000 chest x-rays with disease labels acquired from 30,000 patients.

## Project Highlight

This project GIVES hands-on experience with 2D medical imaging data analysis and preparation of a medical imaging model for regulatory approval.

Upon completion of this project, you would be able to:

* recommend appropriate imaging modalities for common clinical applications of 2D medical imaging
* perform exploratory data analysis (EDA) on medical imaging data to inform model training and explain model performance
* establish the appropriate ‘ground truth’ methodologies for training algorithms to label medical images
* extract images from a DICOM dataset
* train common CNN architectures to classify 2D medical images
* translate outputs of medical imaging models for use by a clinician
* plan necessary validations to prepare a medical imaging model for regulatory approval

The files contained in this repository are the following:

* EDA.ipynb: This is the file to perform the EDA.
* Build and train model.ipynb: This is the file used to build and train the model.
* Inference.ipynb: This is the file for performing clinical workflow integration.
* .dcm files: They are the test files to test the clinical workflow integration.
* sample_labels.csv: This is the file that should be used to assess images in the pixel-level.
* FDA_Submission_Template.md: This is the template to create the FDA submission. Please copy the template into your choice of editor.

For this project it is strongly encouraged the use of GPUs to accelerate the training process.

The paper used is: https://arxiv.org/pdf/1711.05225.pdf

The link to the data is: https://www.kaggle.com/nih-chest-xrays/data

## Pneumonia and X-Rays in the Wild

Chest X-ray exams are one of the most frequent and cost-effective types of medical imaging examinations. Deriving clinical diagnoses from chest X-rays can be challenging, however, even by skilled radiologists. 

When it comes to pneumonia, chest X-rays are the best available method for diagnosis. More than 1 million adults are hospitalized with pneumonia and around 50,000 die from the disease every
year in the US alone. The high prevalence of pneumonia makes it a good candidate for the development of a deep learning application for two reasons: 1) Data availability in a high enough quantity for training deep learning models for image classification 2) Opportunity for clinical aid by providing higher accuracy image reads of a difficult-to-diagnose disease and/or reduce clinical burnout by performing automated reads of very common scans. 

The diagnosis of pneumonia from chest X-rays is difficult for several reasons: 
1. The appearance of pneumonia in a chest X-ray can be very vague depending on the stage of the infection
2. Pneumonia often overlaps with other diagnoses
3. Pneumonia can mimic benign abnormalities

For these reasons, common methods of diagnostic validation performed in the clinical setting are to obtain sputum cultures to test for the presence of bacteria or viral bodies that cause pneumonia, reading the patient's clinical history and taking their demographic profile into account, and comparing a current image to prior chest X-rays for the same patient if they are available.

## About the Dataset

The dataset provided for this project was curated by the NIH specifically to address the problem of a lack of large x-ray datasets with ground truth labels to be used in the creation of disease detection algorithms. 


There are 112,120 X-ray images with disease labels from 30,805 unique patients in this dataset.  The disease labels were created using Natural Language Processing (NLP) to mine the associated radiological reports. The labels include 14 common thoracic pathologies: 

- Atelectasis 
- Consolidation
- Infiltration
- Pneumothorax
- Edema
- Emphysema
- Fibrosis
- Effusion
- Pneumonia
- Pleural thickening
- Cardiomegaly
- Nodule
- Mass
- Hernia 

The biggest limitation of this dataset is that image labels were NLP-extracted so there could be some erroneous labels but the NLP labeling accuracy is estimated to be >90%.

The original radiology reports are not publicly available but you can find more details on the labeling process [here.](https://arxiv.org/abs/1705.02315) 


### Dataset Contents: 

1. 112,120 frontal-view chest X-ray PNG images in 1024*1024 resolution (under images folder)
2. Meta data for all images (Data_Entry_2017.csv): Image Index, Finding Labels, Follow-up #,
Patient ID, Patient Age, Patient Gender, View Position, Original Image Size and Original Image
Pixel Spacing.

## Steps described in FDA_Submission_Final_last.pdf
Adapted from https://github.com/udacity/AIHCND_C2_Starter

### 1. Exploratory Data Analysis

The first part of this project will involve exploratory data analysis (EDA) to understand and describe the content and nature of the data.

Note that much of the work performed during your EDA will enable the completion of the final component of this project which is focused on documentation of the algorithm for the FDA. This is described in a later section, but some important things to focus on during your EDA may be: 

* The patient demographic data such as gender, age, patient position,etc. (as it is available)
* The x-ray views taken (i.e. view position)
* The number of cases including: 
    * number of pneumonia cases,
    * number of non-pneumonia cases
* The distribution of other diseases that are comorbid with pneumonia
* Number of disease per patient 
* Pixel-level assessments of the imaging data for healthy & disease states of interest (e.g. histograms of intensity values) and compare distributions across diseases.

### 2. Building and Training the Model

**Training and validating Datasets**

From the findings in the EDA component of this project, curate the appropriate training and validation sets for classifying pneumonia. Be sure to take the following into consideration: 

* Distribution of diseases other than pneumonia that are present in both datasets
* Demographic information, image view positions, and number of images per patient in each set
* Distribution of pneumonia-positive and pneumonia-negative cases in each dataset

**Model Architecture**

In this project, fine-tuning of an existing CNN architecture will be performed to classify x-rays images for the presence of pneumonia. There is no required archictecture required for this project, but a reasonable choice would be using the VGG16 architecture with weights trained on the ImageNet dataset. Fine-tuning can be performed by freezing a chosen pre-built network and adding several new layers to the end to train, or by doing this in combination with selectively freezing and training some layers of the pre-trained network. 


**Image Pre-Processing and Augmentation** 

Someone may choose or need to do some amount of preprocessing prior to feeding imagees into the network for training and validating. This may serve the purpose of conforming to the model's architecture and/or for the purposes of augmenting the training dataset for increasing your model performance. When performing image augmentation, be sure to think about augmentation parameters that reflect real-world differences that may be seen in chest X-rays. 

**Training** 

In training the model, there are many parameters that can be tweaked to improve performance including: 
* Image augmentation parameters
* Training batch size
* Training learning rate 
* Inclusion and parameters of specific layers in your model 

Descriptions of the methods by which given parameters were chosen, are provided in the final FDA documentation.

 **Performance Assessment**

As the model is trained, its performance over subsequence training epochs is monitored. The appropriate metrics upon which to monitor performance are also choosen. Note that 'accuracy' may not be the most appropriate statistic in this case, depending on the balance or imbalance of the validation dataset, and also depending on the clinical context that this model will be used in (i.e. can you sacrafice high false positive rate for a low false negative rate?)

 __Note that detecting pneumonia is *hard* even for trained expert radiologists, so you should *not* expect to acheive sky-high performance.__ [This paper](https://arxiv.org/pdf/1711.05225.pdf) describes some human-reader-level F1 scores for detecting pneumonia, and can be used as a reference point for how well your model could perform.

### 3. Clinical Workflow Integration 

The imaging data provided for training of the model were transformed from DICOM format into .png to help aid in the image pre-processing and model training steps of this project. In the real world, however, the pixel-level imaging data are contained inside of standard DICOM files. 

For this project, create a DICOM wrapper that takes in a standard DICOM file and outputs data in the format accepted by the model. Make sure to include several checks in your wrapper for the following: 
* Proper image acquisition type (i.e. X-ray)
* Proper image acquisition orientation (i.e. those present in the training data)
* Proper body part in acquisition


### 4. FDA  Submission

For this project, the following steps that are derived from the FDA's official guidance on both the algorithm description and the algorithm performance assessment should be completed. __*Much of this portion of the project relies on what was done during EDA, model building, and model training. Figures and statistics from those earlier parts can be used to complete the following documentation.*__

**1. General Information:**

* First, provide an Intended Use statement for the model 
* Then, provide some indications for use that should include: 
    * Target population
    * When your device could be utilized within a clinical workflow
* Device limitations, including diseases/conditions/abnormalities for which the device has been found ineffective and should not be used
* Explain how a false positive or false negative might impact a patient

**2. Algorithm Design and Function**

In this section, describe the _fully trained_ algorithm and the DICOM header checks that have been built around it. Include a flowchart that describes the following: 

* Any pre-algorithm checks that were performed on the DICOM
* Any preprocessing steps performed by the algorithm on the original images (e.g. normalization)
    * Note that this section should _not_ include augmentation
* The architecture of the classifier


For each stage of the algorithm, briefly describe the design and function.

**3. Algorithm Training**

Describe the following parameters of the algorithm and how they were chosen: 

* Types of augmentation used during training
* Batch size
* Optimizer learning rate
* Layers of pre-existing architecture that were frozen
* Layers of pre-existing architecture that were fine-tuned
* Layers added to pre-existing architecture

Also describe the behavior of the following throughout training (use visuals to show):

* Training loss
* Validation loss 

Describe the algorithm's final performance after training was complete by showing a precision-recall curve on your validation set.

Finally, report the threshold for classification that you chose and the corresponded F1 score, recall, and precision. Give one or two sentences of explanation for why this threshold value was chosen. 

**4. Databases**

For the database of patient data used, provide specific information about the training and validation datasets that were curated separately, including: 

* Size of the dataset
* The number of positive cases and the its radio to the number of negative cases
* The patient demographic data (as it is available)
* The radiologic techniques used and views taken
* The co-occurrence frequencies of pneumonia with other diseases and findings

**5. Ground Truth**

The methodology used to establish the ground truth can impact reported performance. Describe how the NIH created the ground truth for the data that were provided for this project. Describe the benefits and limitations of this type of ground truth.  

**6. FDA Validation Plan**

_Describe_ how a FDA Validation Plan would be conducted for the specific algorithm, rather than actually performing the assessment. Describe the following: 

* The patient population that imaging data should be requested from, from your clinical partner. Make sure to include: 
    * Age ranges
    * Sex
    * Type of imaging modality
    * Body part imaged
    * Prevalence of disease of interest
    * Any other diseases that should be included _or_ excluded as comorbidities in the population

* Provide a short explanation of how an optimal ground truth would be obtained
* Provide a performance standard chosen based on [this paper.](https://arxiv.org/pdf/1711.05225.pdf)
