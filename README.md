# Audio-Deepfake-Detection

GitHub Repository: [Audio Deepfake Detection](https://github.com/media-sec-lab/Audio-Deepfake-Detection)

### **Problem Statement**
Detecting AI-generated human speech, ensuring potential for real-time or near real-time detection, and analyzing real conversations is a growing challenge. 

---
## Research & Selection

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

### **Selection of Network Structure**

Given the requirement for **real-time or near real-time detection**, **high adaptability**, and **analysis of real conversations**, we evaluated various network structures based on efficiency, scalability, and robustness against deepfake speech synthesis.

#### **1. RawNet + SincConv (Modified Architecture)**
- **SincConv for Initial Filtering**: The raw waveform is first processed by a Sinc convolutional layer, which applies learnable band-pass filters. This directly targets the frequency artifacts introduced by AI-generated speech.
- **RawNet Backbone**: The processed signal is passed through a CNN-based architecture (like RawNet), which is tailored for end-to-end speaker verification tasks but adapted here for spoof detection.
- **Attention Mechanism**: Additional self-attention layers after the residual CNN blocks help the network focus on relevant temporal segments of the speech for improved detection accuracy.
- **Fast Inference & Low Latency**: The combined use of SincConv and CNN enables faster training and inference, making it suitable for real-time applications.
- **Verdict**: Best suitable for real-time deepfake detection in natural conversations.

#### **2. ResNet-based Architectures (e.g., RawNet2, ResNet18)**
- **Deep Residual Learning**: Capable of learning subtle differences between real and fake audio due to deep structure.
- **Adaptable & Robust**: Performs well across multiple datasets and deepfake methods.
- **Slightly Higher Inference Time**: While effective, it requires more computational resources than lighter CNNs with SincConv.
- **Verdict**:  High accuracy, but not ideal for strict real-time use without hardware acceleration.

#### **3. Transformer-based Architectures**
- **Excellent Context Modeling**: Models like AST (Audio Spectrogram Transformer) capture global audio context effectively.
- **Computationally Expensive**: Transformers are powerful but require significant computation, which hinders real-time deployment.
- **Verdict**: Not suitable for real-time detection due to high latency.

---

### **Final Model Selection**

Keeping all the above considerations in mind — especially real-time capability, robustness against diverse spoofing techniques, and effectiveness with raw audio inputs — the following three models have been selected for implementation and comparative evaluation:

#### **1. [End-to-End Anti-Spoofing with RawNet2](https://ieeexplore.ieee.org/abstract/document/9414234)**
#### **2. [Audio Anti-Spoofing with a Simple Attention Module and Joint Optimization](https://arxiv.org/abs/2211.09898)**  
#### **3. [AASIST: Audio Anti-Spoofing using Integrated Spectro-Temporal Graph Attention Networks](https://arxiv.org/abs/2110.01200)**

#### Summary

| Model | Key Technical Innovation | EER | t-DCF | Why It’s Promising | Potential Limitations |
|-------|---------------------------|-------------------------------|----------|-----------|------------------------|
| **End-to-End Anti-Spoofing with RawNet2** | End-to-end residual CNN that learns directly from raw waveforms; uses AM-Softmax loss to improve class separation. | ASVspoof 2019 LA : 1.12% | ASVspoof 2019 LA : 0.033% | Lightweight and fast, ideal for real-time applications. Eliminates need for handcrafted features. | May be less robust against newer, sophisticated spoofing methods. Requires regularization. |
| **Audio Anti-Spoofing with a Simple Attention Module and Joint Optimization** | Combines simple attention module (SAM), additive angular margin loss, and meta-learning for generalization. | ASVspoof 2021 LA eval : 0.99% | ASVspoof 2021 LA eval : 0.029% | Balances accuracy and efficiency. Attention highlights key segments. Meta-learning improves adaptability to new attacks. | More complex to train. Needs tuning for real-world acoustic variation. |
| **AASIST: Audio Anti-Spoofing using Integrated Spectro-Temporal Graph Attention Networks** | Graph Attention Network capturing both spectral and temporal information from speech spectrograms. | ASVspoof 2019 LA : 0.83% | ASVspoof 2019 LA : 0.028% | High accuracy across multiple datasets. Captures intricate spoofing cues. Robust to noise and reverberation. | Heavier model. Needs optimization (e.g., pruning, quantization) for real-time deployment. |

---





