# AI Solution Design Report — Agriculture Domain

## Task 1 — Business Domain
**Selected Domain:** Agriculture

## Task 2 — Business Problem Definition

**What problem is being solved?**
Crop diseases cause significant damage to agricultural yields worldwide.
Farmers often fail to identify diseases early enough to take corrective
action, resulting in large-scale crop loss and reduced income.

**Who are the users or stakeholders?**
- Farmers and agricultural workers
- Agricultural extension officers
- Government agricultural departments
- Agri-tech companies and cooperatives

**What is the current manual process?**
Currently, farmers visually inspect their crops and either rely on
personal experience or consult a local agricultural expert to identify
diseases. Samples may be sent to laboratories for analysis, which takes
several days.

**Limitations of the current process:**
- Manual inspection is slow and requires expert knowledge
- Laboratory analysis is expensive and time-consuming
- By the time a disease is identified, it may have already spread
- Rural farmers often lack access to agricultural experts
- Inconsistent diagnosis depending on the experience of the inspector

## Task 3 — AI Task Type

**Selected AI Task Type:** Image Classification

**Why is this suitable?**
Each leaf image belongs to one specific disease category or is healthy.
The goal is to look at a photograph of a crop leaf and classify it into
one of several disease categories such as bacterial blight, leaf rust,
or powdery mildew. This is a direct image classification problem where
a CNN model can learn visual patterns such as colour changes, spots,
and texture abnormalities that distinguish one disease from another.
Object detection is not needed since the entire leaf image represents
the condition, and segmentation is not required since we only need the
class label, not pixel-level annotations.

## Task 4 — Data Requirement Plan

**Type of data needed:**
High-resolution photographs of crop leaves showing various disease
conditions and healthy leaves.

**Structured or Unstructured:**
Unstructured — image data stored as JPEG or PNG files.

**Input Features:**
- Leaf images captured by smartphone cameras or drone cameras
- Image resolution of at least 224x224 pixels
- Images taken under natural daylight conditions
- Multiple crop types such as wheat, rice, maize, and tomato

**Target Variable:**
Disease class label for each image, such as:
- Healthy
- Bacterial Blight
- Leaf Rust
- Powdery Mildew
- Early Blight
- Late Blight

**Data Collection Method:**
- Field photography by agricultural extension workers
- Crowdsourced images from farmer mobile applications
- Existing public datasets such as PlantVillage dataset
- Partnership with agricultural research institutes

**Data Quality Risks:**
- Images taken in poor lighting conditions may reduce accuracy
- Overlapping symptoms between different diseases may confuse the model
- Dataset may be imbalanced if some diseases are rarer than others
- Images from different regions may show the same disease differently
- Mislabelled images from crowdsourcing can introduce noise

## Task 5 — Model Recommendation

**Recommended Model:** Transfer Learning using MobileNetV2 or ResNet50

**Why this model is appropriate?**
Training a CNN from scratch requires very large datasets and significant
computing resources. Transfer learning uses a pre-trained model that has
already learned rich visual features from millions of images. By fine-tuning
only the final layers on the crop disease dataset, we can achieve high
accuracy even with a relatively small dataset. MobileNetV2 is particularly
suitable for deployment on mobile devices used by farmers in the field,
as it is lightweight and fast. ResNet50 offers higher accuracy for more
complex disease patterns where deeper feature extraction is needed.

**Model Architecture:**
- Input: Leaf image resized to 224x224 pixels
- Base: Pre-trained MobileNetV2 or ResNet50 (frozen layers)
- Global Average Pooling layer
- Dense layer with ReLU activation
- Dropout layer for regularization
- Output: Softmax layer with one neuron per disease class

## Task 6 — Evaluation Plan

**Technical Metrics:**
- Accuracy — overall percentage of correct predictions
- Precision — how many predicted disease cases are actually diseased
- Recall — how many actual disease cases were correctly detected
- F1-Score — balance between precision and recall
- Confusion Matrix — to identify which diseases are most confused

**Business Metrics:**
- Reduction in crop loss percentage after AI deployment
- Time saved per diagnosis compared to manual inspection
- Number of farmers using the mobile application per month
- Cost saved per farm compared to laboratory testing

**Possible Failure Cases:**
- Model misclassifies a diseased leaf as healthy — farmer takes no action
  and disease spreads
- Model trained on one region performs poorly in another region due to
  different crop varieties or climate conditions
- Poor image quality from low-end smartphones reduces accuracy

**Human Review Process:**
- All high-risk predictions (low confidence score) flagged for expert review
- Agricultural extension officers verify a random sample of predictions monthly
- Feedback loop where incorrect predictions are corrected and used for retraining

## Task 7 — Responsible AI Considerations

**Bias in Data:**
If the training dataset contains mostly images from one region or crop
variety, the model may perform poorly on crops from other regions. Care
must be taken to collect diverse data from multiple geographies and
growing conditions.

**Incorrect Predictions:**
A false negative — predicting a diseased crop as healthy — could lead
to untreated disease spreading across an entire farm. This could cause
significant financial loss for farmers. Confidence thresholds should
be set so that uncertain predictions are always referred to a human expert.

**Privacy Concerns:**
If the mobile application collects GPS location data along with images,
farmers' field locations and crop conditions become sensitive commercial
information. Data must be stored securely and farmers must consent to
data collection.

**Over-Reliance on AI:**
Farmers may stop developing their own knowledge of crop diseases and
rely entirely on the app. If the app is unavailable due to no internet
connection in rural areas, farmers may be helpless. Offline model
deployment should be considered.

**Impact on Users:**
Incorrect recommendations could lead farmers to apply the wrong pesticide,
causing crop damage, financial loss, and environmental harm. Clear
disclaimers must be shown and farmers should be encouraged to verify
recommendations.

**Need for Human Oversight:**
The AI system should be positioned as a decision support tool, not a
replacement for agricultural experts. All critical decisions should
involve a human expert, especially for new or rare disease types not
well represented in the training data.

## Task 8 — Final Solution Summary

**Problem:**
Farmers lack fast, affordable, and accurate tools to detect crop diseases
early, leading to significant yield loss and economic hardship.

**Proposed AI Solution:**
A mobile application powered by a CNN-based image classification model
that allows farmers to photograph a crop leaf and instantly receive a
disease diagnosis with recommended treatment steps.

**Required Data:**
Labelled photographs of crop leaves covering healthy and diseased
conditions across multiple crop types, collected from diverse geographic
regions and lighting conditions.

**Model Recommendation:**
Transfer learning using MobileNetV2 pre-trained on ImageNet, fine-tuned
on a crop disease dataset. Deployed as a lightweight model on a mobile
application for offline use in rural areas.

**Expected Business Impact:**
- Up to 40% reduction in crop loss through early disease detection
- Diagnosis time reduced from days to seconds
- Accessible to farmers in remote areas without agricultural experts
- Estimated cost saving of thousands of rupees per farm per season

**Risks and Mitigation Plan:**
- Risk of misdiagnosis mitigated by confidence thresholds and expert review
- Regional bias mitigated by collecting diverse training data
- Privacy protected by anonymising location data and obtaining consent
- Over-reliance mitigated by educating farmers and enabling offline use
- Incorrect pesticide use mitigated by clear disclaimers and human verification