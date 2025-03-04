**Problem Statement**

Develop a Gesture recognition model for Smart TVs that can recognise five different gestures performed by the user which will help users control the TV without using a remote.


The gestures are continuously monitored by the webcam mounted on the TV. Each gesture corresponds to a specific command:

- Thumbs up:  Increase the volume
- Thumbs down: Decrease the volume
- Left swipe: 'Jump' backwards 10 seconds
- Right swipe: 'Jump' forward 10 seconds  
- Stop: Pause the movie

**The data has been uploaded to Kaggle.**

**https://www.kaggle.com/datasets/kk20krishna/gesture-recognition-dataset**


Each video is a sequence of 30 frames (or images).
The training data consists of a few hundred videos categorised into one of the five classes. Each video (typically 2-3 seconds long) is divided into a sequence of 30 frames(images). These videos have been recorded by various people performing one of the five gestures in front of a webcam - similar to what the smart TV will use.


The data consists of a 'train' and a 'val' folder with two CSV files for the two folders. These folders are in turn divided into subfolders where each subfolder represents a video of a particular gesture. Each subfolder, i.e. a video, contains 30 frames (or images).
Note that all images in a particular video subfolder have the same dimensions but different videos may have different dimensions. Specifically, videos have two types of dimensions - either 360x360 or 120x160 (depending on the webcam used to record the videos). Hence, you will need to do some pre-processing to standardise the videos.


Each row of the CSV file represents one video and contains three main pieces of information - the name of the subfolder containing the 30 images of the video, the name of the gesture and the numeric label (between 0-4) of the video.

# Comparison of Architecture options for Hand Gesture Recognition

| Model | Strengths | Weaknesses |
|--------|-----------|------------|
| CNN-3D | Captures both spatial and temporal features simultaneously. Works well with short video clips. No need for explicit sequential modeling. | Computationally expensive due to 3D convolutions. Requires large datasets for effective training. |
| CNN-2D + GRU | Lighter than CNN-3D. Extracts spatial features per frame and models temporal dependencies using GRU. Suitable for real-time applications. | Does not directly capture spatio-temporal dependencies like CNN-3D. Requires careful frame selection for consistency. |
| MobileNet + GRU (non-trainable base) | Lightweight and efficient for edge devices. MobileNet extracts spatial features while GRU models temporal dependencies. Faster training due to frozen base. | Loss of fine-tuned feature extraction. May not generalize well to new datasets. |
| MobileNet + GRU (trainable base) | More accurate than the non-trainable version. Fine-tuning allows better adaptation to the dataset. Still relatively lightweight. | Slightly heavier than the non-trainable version. Needs careful tuning to avoid overfitting. |
| EfficientNetB0 + GRU (trainable base) | More accurate than MobileNet while remaining efficient. Neural Architecture Search (NAS) optimizes feature extraction. GRU models temporal dependencies well. | Heavier than MobileNet but still efficient. Fine-tuning required for optimal results. |


We will experiment with these models:

- CNN-3D  
- CNN-2D + GRU  
- MobileNet + GRU (non-trainable base)  
- MobileNet + GRU (trainable base)  
- EfficientNetB0 + GRU (trainable base)  

This will help compare their performance and efficiency.

![image](https://github.com/user-attachments/assets/0e240d00-601d-4e54-8c30-233568889bd9)


# Model for Real-Time TV Application
Since both MobileNet and EfficientNet transfer learning models provide the simillar accuracy in this scenario, the deciding factors are latency, computational efficiency, and deployment feasibility:

- **Choose MobileNet** if the application demands ultra-low latency and needs to run on resource-constrained devices like TV processors, edge AI chips, or mobile hardware.
- **Choose EfficientNet** if the application can afford slightly higher latency but needs better scalability and feature extraction for complex TV-based AI tasks (e.g., high-resolution content analysis).


# **Final Model**

**For real-time TV applications where fast inference and low power consumption are crucial, MobileNetV3 transfer learning model model_17_MobileNet_Train_RNN_GRU is the best choice.**
