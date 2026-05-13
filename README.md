# Lobitania-Jessica_LW3_Custom_Image_Classifier

## Guide Questions – Student Reflection & Explanation

### 1. Dataset Preparation

**How did you organize your dataset in Google Drive?**
The dataset was organized by creating a main folder named `plants_dataset`. Each of the 20 plant categories has its own subfolder, and each subfolder contains all the images for that specific class. We used a Python script to scrub the folders of any corrupted or invalid image formats before training.

**Why is folder structure important for TensorFlow image loading?**
The folder structure is critical because TensorFlow uses the names of the subdirectories as the actual labels for classification. By organizing them nicely, TensorFlow automatically knows which image belongs to which class without us having to label them manually.

---

### 2. Model Training

**What is the role of convolutional layers in image classification?**
Convolutional layers act as feature extractors. They scan over the images to detect important visual patterns like edges, colors, shapes, and textures, which the model then uses to differentiate between the different plant species.

**Why do we split data into training and validation sets?**
We split the data so the model can "learn" from the training set, but be "tested" on the validation set. If we evaluated the model on the same data it trained on, it would just memorize the answers. The validation set proves that the model can generalize to unseen images.

---

### 3. Performance Analysis

**What accuracy did your model achieve?**
On the initial baseline run, the model achieved an incredibly high training accuracy of **99.86%**, but a validation accuracy of **86.66%**. After adding data augmentation and dropout, the training accuracy dropped to **71.91%** and validation accuracy to **72.60%**.

**How did the number of images affect the model’s performance?**
A larger and cleaner dataset gives the model more examples to learn from. However, because our dataset had some noise, adding data augmentation (which artificially creates more images by flipping/rotating) actually made the task harder for the model to memorize, dropping the raw accuracy but improving stability.

---

### 4. Critical Thinking

**What challenges did you encounter while using your own dataset?**
The biggest challenge was dealing with corrupted images. Scraping images from the internet resulted in files that looked like `.jpg`s but were actually unsupported formats (like `.webp` or `.tiff`) or completely corrupted. We had to write a strict Python verification script to clean the dataset before TensorFlow could even process it.

**How can data augmentation improve your model?**
Data augmentation artificially expands the dataset by rotating, zooming, and flipping images. It forces the model to learn the actual features of the plant rather than memorizing the exact pixel layout of the original photos, drastically reducing overfitting.

---

### 5. Application

**Suggest a real-world application for your trained model.**
This model could be the foundation of a mobile application for hikers or botanists. Users could snap a picture of an unknown plant in the wild, and the app would classify it instantly.

**How can this system be integrated into a mobile or web application?**
The trained `.keras` model could be hosted on a cloud backend (like AWS or Google Cloud). A mobile app could send an image payload via an API request to the server, the server runs the image through the TensorFlow model, and returns the predicted plant name and confidence score to the user's screen.

---

## Activity 3A: Improving and Evaluating a Custom Image Classifier

### Visualization & Overfitting

**1. What signs indicated overfitting in your first model?**
The most obvious sign was the massive gap between training and validation. By Epoch 10, the training accuracy was **99.86%** with a loss of **0.0101**, meaning it had almost perfectly memorized the training data. However, the validation accuracy was stuck at **86.66%**, proving it was struggling to generalize that memorization to unseen data.

**2. How did data augmentation affect validation accuracy?**
Data augmentation successfully broke the model's ability to just memorize the data. It caused both the training and validation accuracy to drop down to the ~72% range. While the absolute number is lower, the model is significantly healthier because the gap is completely gone, proving it is learning true features, not just memorizing.

---

### Model Improvement

**3. What is the purpose of dropout layers?**
Dropout layers randomly turn off a percentage of neurons during each training step. This forces the model to not rely on any single neuron or specific feature, creating a more robust and generalized network.

**4. Why does data augmentation improve generalization?**
It forces the network to recognize the plant regardless of its orientation, size, or angle, meaning the model learns what the plant *actually looks like* in the real world, rather than just what it looks like perfectly centered in a Google Image search.

---

### Performance Comparison

**5. Compare accuracy before and after improvements.**
- **Before (Baseline):** Training Acc = 99.86% | Validation Acc = 86.66% (Overfitting by 13.2%)
- **After (Improved):** Training Acc = 71.91% | Validation Acc = 72.60% (Underfitting/Heavily Regularized)
The improvements successfully cured the overfitting, but because we used aggressive augmentation and dropout, the model is now slightly underfitting and would likely benefit from more training epochs to reach the 80%+ range again.

**6. Which technique contributed most to improvement?**
Data augmentation contributed the most because it fundamentally altered the dataset the model was learning from. By constantly shifting and rotating the data, it completely destroyed the model's ability to cheat by memorizing pixel placements.

---

### Deployment & Application

**7. Why is saving the model important?**
Training takes a significant amount of time and computational power. By saving it as a `.keras` file, we can instantly load the "brain" of the AI in seconds without ever having to process the dataset or run epochs again.

**8. How can this model be deployed in a real-world system?**
The saved `.keras` model can be containerized using Docker and deployed via a REST API (using Flask or FastAPI). Any web or mobile client could then send HTTP POST requests containing image data to the API, which would run the model inference and return the classification results in JSON format.