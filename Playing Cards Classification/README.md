# Playing Cards Classification (52-Card Deck)

This project aims to **classify images of playing cards** into one of **52 possible classes** (plus a potential Joker class, totaling 53 classes). By leveraging **transfer learning** with **MobileNetV2**, the model can accurately recognize individual card ranks and suits from raw images. An accompanying **Streamlit** application further demonstrates how to deploy the trained model in a user-friendly interface.

---

## Table of Contents
1. [Overview of the Dataset](#overview-of-the-dataset)  
2. [Model Architecture & Transfer Learning](#model-architecture--transfer-learning)  
3. [Training Strategy](#training-strategy)  
4. [Streamlit App](#streamlit-app)  
5. [Conclusion](#conclusion)

---

## Overview of the Dataset
- **Data Source**: A collection of images representing each card in a standard deck.  
- **Train Set**: 6,121 images, belonging to 53 classes (52 cards + 1 possible extra).  
- **Validation/Test Set**: 1,503 images, also categorized into 53 classes.  
- **Image Loading**: Utilized `ImageDataGenerator` from `tensorflow.keras.preprocessing.image` to read images directly from a directory structure.

**Sample Images**  
![52 Cards](cards.png)

The dataset is relatively small for a deep learning task, making transfer learning an ideal strategy to achieve high accuracy without requiring millions of images.

---

## Model Architecture & Transfer Learning
### Why Transfer Learning?
Transfer learning allows us to **reuse a pretrained network** (in this case, **MobileNetV2**) that has already learned to detect generic features from a large image dataset (ImageNet). By fine-tuning the final layers on our specific dataset, we **speed up training**, **improve performance**, and **reduce overfitting**, even with a limited number of images.

### MobileNetV2
- **Base Model**: MobileNetV2 (`include_top=False`, `weights='imagenet'`, `pooling='avg'`) provides a feature extraction backbone.
- **Trainable Layers**: The last 25 layers of MobileNetV2 were unfrozen to allow fine-tuning. Earlier layers remain frozen to preserve generic features.
- **Custom Classification Head**: 
  - Multiple **Dense** layers with **Dropout** and **BatchNormalization** to reduce overfitting and stabilize training.  
  - Final **Dense** layer with **softmax** activation to output 53 class probabilities.

This architecture combines **pretrained convolutional filters** with a tailored top, maximizing performance for playing card images.

---

## Training Strategy
1. **Optimizer & Loss**  
   - **Adam** optimizer with a low learning rate (`0.00006`) for stable fine-tuning.  
   - **Categorical Crossentropy** loss, suitable for multi-class classification.
2. **Regularization**  
   - **L2 (weight decay)** on Dense layers to prevent overfitting.  
   - **Dropout** (0.3–0.4) further reduces the risk of overfitting by randomly deactivating neurons.
3. **Callbacks**  
   - **EarlyStopping**: Monitors validation loss and stops training when improvements plateau.  

---

## Streamlit App
A **Streamlit** application was developed to provide a **user-friendly interface** for card classification:

1. **Image Upload**: Users can upload an image of a playing card.
2. **Prediction**: The app displays the predicted class (e.g., “Ace of Spades”) and the corresponding confidence level.
3. **Interactive Visualizations**: Screenshots below show how the app’s layout helps users easily navigate and interpret the model’s outputs.

![Streamlit App 1](st_app1.png)  
![Streamlit App 2](st_app2.png)

---

## Conclusion
This project demonstrates how **transfer learning** with MobileNetV2 can be effectively applied to **image classification** tasks involving a **limited dataset**. By carefully fine-tuning the model’s final layers and adding regularization, the model achieves strong performance in recognizing **52 playing cards**. The **Streamlit** app further illustrates a simple, practical way to **deploy** the model for real-world usage, enabling users to quickly and accurately identify playing cards from images.
