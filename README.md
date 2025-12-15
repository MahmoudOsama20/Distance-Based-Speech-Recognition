# 📄 **Distance-Based Speech Recognition & Deep Learning Classifier**

## 🎧 **Project Title:**

# **Distance-Based Speech Recognition Using FFT + CNN Mel-Spectrogram Classifier**

---

## 📘 **Overview**

This project implements two different approaches for recognizing short spoken commands from the **Mini Speech Commands Dataset**:

### **1️⃣ DSP-Based Distance Classifier (Template Matching)**

- Frames the audio signal
- Computes **FFT magnitude spectrum** per frame
- Averages spectra to produce a **feature vector** per audio file
- Creates **class templates** by averaging all training files of each class
- Classifies a test file by **minimum distance** (Euclidean, Manhattan, or Cosine)

### **2️⃣ Deep Learning Classifier (CNN + Mel Spectrograms)**

- Converts audio into **log-Mel spectrograms**
- Trains a **2D Convolutional Neural Network** to classify speech commands
- Achieves **91% accuracy**, significantly higher than the DSP-based method

This project allows a direct comparison between **traditional DSP approaches** and **modern deep learning** for speech recognition.

---

## 📂 **Dataset**

**Mini Speech Commands Dataset**
Download link:

```
http://storage.googleapis.com/download.tensorflow.org/data/mini_speech_commands.zip
```

### **Target Classes**

The project focuses on **8** spoken words:

```
yes, no, up, down, left, right, go, stop
```

The dataset contains **thousands of short WAV files**, each around **1 second**.

---

# 🔧 **Part 1 — DSP-Based Speech Recognition**

This section describes the full traditional signal-processing pipeline used before deep learning.

---

## 🎛️ **1. Preprocessing**

### **A. Framing**

- Sample Rate: **16 kHz**
- Frame Length: **256 samples** (16 ms)
- Hop Length: **128 samples** (8 ms, 50% overlap)

Each file is divided into overlapping windows to make speech locally stationary.

---

## 🎚️ **2. Feature Extraction**

### **FFT per Frame**

For each frame:

1. Apply **Hamming window**
2. Compute **FFT**
3. Keep only the **first half** (positive frequencies)
4. Extract **magnitude spectrum**

### **Frame Averaging**

All spectra of a file are averaged:

```
file_feature = mean(frame_spectra)
```

Final feature vector size: **128 values**

---

## 🧩 **3. Template Creation (Training Stage)**

For each class:

- Process the first **800 files**
- Compute average spectrum for each file
- Average all file spectra → **class template**

Each class is represented by **one vector**, its "signature".

---

## 🎯 **4. Classification (Testing Stage)**

For every test file:

1. Compute its average spectrum
2. Calculate the distance to each class template
3. Choose the class with the **smallest distance**

### Supported Distance Metrics

- **Euclidean (L2)**
- **Manhattan (L1)**
- **Cosine Distance**

---

## 📊 **5. DSP Model Results**

| Metric     | Accuracy |
| ---------- | -------- |
| Euclidean  | 25%      |
| Manhattan  | 25.3%    |
| **Cosine** | **38%**  |

The best classical DSP accuracy is **38%**, using cosine distance.

---

# 🤖 **Part 2 — Deep Learning Model (CNN Mel-Spectrogram Classifier)**

To improve performance, a modern deep learning model was implemented.

---

## 🎛️ **1. Feature Extraction**

Each WAV file is converted to a **log-Mel spectrogram**:

- FFT size: **512**
- Hop length: **160**
- Mel filters: **40**
- Converted to dB scale
- Normalized between 0 and 1
- Padded so all inputs have equal size

Shape example:

```
(time_steps, 40, 1)
```

---

## 🧠 **2. CNN Architecture**

A compact but powerful 2D CNN:

```
Conv2D (16 filters) → MaxPool
Conv2D (32 filters) → MaxPool
Conv2D (64 filters) → MaxPool
Flatten
Dense(128, ReLU)
Dropout(0.4)
Dense(8, Softmax)
```

### Loss & Optimizer

- **Sparse Categorical Crossentropy**
- **Adam optimizer**

---

## 🚀 **3. Training**

- Train/Test split: **80/20**
- Validation split: **20%**
- Batch size: **32**
- Epochs: **20**

---

## 📈 **4. Deep Learning Results**

The CNN model achieved:

# 🎉 **Accuracy: 91%**

This demonstrates a major improvement over the DSP-based method.

---

# 🧪 **5. Prediction Function**

A standalone function allows predicting any WAV file:

- Load file
- Create log-Mel spectrogram
- Pad to required shape
- Run through CNN
- Output predicted class and probability

---

# 📜 **Conclusion**

The project explored two approaches:

### **1) Classical DSP Speech Recognition**

- Simple
- Fast
- Achieved **38% accuracy**

### **2) Deep Learning Speech Recognition**

- Uses audio spectrograms
- Extracts complex time–frequency patterns
- Achieved **90% accuracy**

The deep learning model clearly provides a more accurate and robust solution for speech command recognition.
