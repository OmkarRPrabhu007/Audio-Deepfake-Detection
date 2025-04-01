# Audio-Deepfake-Detection

GitHub Repository: [Audio Deepfake Detection](https://github.com/media-sec-lab/Audio-Deepfake-Detection)

### **Problem Statement**
Detecting AI-generated human speech, ensuring potential for real-time or near real-time detection, and analyzing real conversations is a growing challenge. 

---

### **Selection of Forgery Detection Technique**

#### **Handcrafted Feature-based Detection**
- **Simple and Easy to Understand**: This method uses predefined features like sound patterns (MFCCs) to detect fake audio.
- **Limited Flexibility**: It might not work well with new and advanced deepfake models as it's based on features that were manually chosen.
- **Slower Processing**: Extracting these features takes extra time, which can make it too slow for real-time use.
- **Verdict**: Not suitable due to poor adaptability and real-time inefficiency.

#### **Hybrid Feature-based Detection**
- **Combines Two Approaches**: It combines manually designed features with those learned by a machine resultig in better accuracy.
- **More Flexible**: It's a step up from purely handcrafted methods because it can learn from data, but still relies on predefined features.
- **Partial Real-time Capability**: While it can work faster than purely handcrafted methods, the time spent on feature extraction still slows it down.
- **Verdict**: Partially suitable but not ideal for real-time analysis.

#### **End-to-End Forgery Detection**
- **Automated Feature Extraction**: Instead of manually extracting features, end-to-end models automatically learn which features are important directly from the raw audio data during training.
- **Flexible to New Techniques**: As the model learns the features on its own, it's more adaptable to evolving deepfake technologies, without needing manual updates.
- **Faster and More Efficient**: This process eliminates the need for extra manual feature extraction steps, making it more efficient and suitable for real-time use.
- **Verdict**: Best suited for real-time applications, with high accuracy and adaptability. While computationally demanding, it is the most efficient method for real-time detection.

#### **Feature Fusion-based Detection**
- **Combines Different Features**: This method uses multiple features from both handcrafted and machine-learned sources, leading to high accuracy.
- **Works Well with Different Deepfake Techniques**: It's robust and can detect various types of speech forgery.
- **Requires High Processing Power**: The complexity of handling multiple features at once makes it slow and impractical for real-time use.
- **Verdict**: Too computationally expensive for real-time applications.

#### **Multi-task Learning-based Detection**
- **Multiple Learning Goals**: This method trains a model to learn several tasks at once, which improves its ability to detect different types of audio forgery.
- **Very Accurate**: It performs well across different deepfake methods, making it reliable for various types of fake speech.
- **Needs a Lot of Data**: It requires large amounts of labeled data to train the model, and even then, it’s slow to make predictions, so it's not ideal for real-time use.
- **Verdict**: High accuracy, but not suitable for real-time detection due to computational demands.

#### **Our Selection: End-to-End Forgery Detection**
End-to-End Detection eliminates the need for manual feature extraction, learning directly from raw audio data. This makes it **fast, highly accurate, and adaptable** to evolving deepfake techniques.

---

### **Data Augmentation Considerations**
- Given that the dataset is already comprehensive and diverse, adding data augmentation might not significantly improve the model's performance for this specific use case. The variety within the ASVspoof 5 dataset should be sufficient for training a robust end-to-end detection model.
- While data augmentation may help in certain cases, the current focus is on the model's ability to detect deepfakes in real-time using high-quality raw data. If used, augmentation techniques like noise addition or pitch shifting may slightly enhance the model's robustness but are unlikely to drastically change the results.
**Verdict**: Data augmentation, while useful in some scenarios, is not critical for this particular project and will not significantly affect the model's performance if omitted.

---

### **Feature Extraction Method**
The top selected feature extraction methods , 

#### **1. Sinc Filter**
- Uses band-pass filters to directly capture frequency distortions in the speech signal.
- Low computational cost and faster than traditional methods.
- Captures fine-grained frequency changes made by deepfake synthesis.
- Might need additional learned features for complex deepfake attacks.
- **Verdict**: Best for real-time detection due to efficiency.

#### **2. WavLM**
- A self-supervised model trained on a vast amount of diverse speech data, capturing complex speech patterns.
- Excellent at distinguishing between real and fake speech.
- Works well in noisy or reverberant environments.
- Not ideal for real-time deployment without optimization.
- **Verdict**: Best for accuracy, but requires more computational resources.

#### **3. FastAudio Filter**
- Optimized for low-latency audio tasks, focusing on quick processing and effective feature extraction.
- Balances speed and accuracy.
- Effectively captures temporal and spectral distortions.
- Slightly lower accuracy compared to WavLM, especially in large-scale datasets.
- **Verdict**: Good balance of speed and accuracy for real-time use.

  So, we would choose one among these for the feature extraction 
---



