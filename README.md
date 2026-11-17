# Speech to Sign Language (ISL) for a Robotic Hand

Turns spoken words into **Indian Sign Language (ISL)** gestures performed by a 3D-printed robotic hand.

```
🎤 Speech ──► Whisper (speech-to-text) ──► text ──► ISL gesture model ──► servo angles ──► ESP32 + PCA9685 ──► robotic hand
```

## What's inside
| Path | Description |
|------|-------------|
| `notebooks/ISL_Speech_to_Sign_Training.ipynb` | Google Colab notebook: dataset download, hand-landmark extraction, model training, evaluation, live webcam test, servo-angle table export, speech-to-gesture demo |

## How it works
1. **Hand landmarks:** MediaPipe Hand Landmarker finds 21 key points per hand in every dataset image.
2. **Classifiers:** Logistic Regression and a neural network (MLP) are trained on the normalized landmarks and give confidence scores for each sign.
3. **Gesture templates:** the median finger bend of each sign is converted into servo angles for the hand and exported as `gesture_table.h` for the ESP32.
4. **Speech:** OpenAI Whisper (pretrained) transcribes speech. Words with their own sign are played directly; numbers become digits; other words are finger-spelled.

## Dataset
[Indian Sign Language (ISL)](https://www.kaggle.com/datasets/prathumarikeri/indian-sign-language-isl) on Kaggle: 35 classes (1–9, A–Z).
The dataset is **not** included in this repo; the notebook downloads it with `kagglehub`.

## Run it in Google Colab
1. Open the notebook in Colab (File → Upload notebook, or open it from GitHub).
2. Runtime → Change runtime type → **T4 GPU**.
3. Add your Kaggle token as a Colab secret: 🔑 Secrets → name `KAGGLE_API_TOKEN` → turn on *Notebook access*.
   **Never paste tokens into the notebook or commit them.**
4. Run the cells from top to bottom. Outputs (models, `gestures.json`, `gesture_table.h`, confusion matrix) are saved to `MyDrive/ISL/output`.

## Hardware
- 3D-printed hand with silicone-wire tendons on servo pulleys
- 4 × MG995 servos (fingers) + 1 thumb-rotation servo
- ESP32 (NodeMCU) + PCA9685 16-channel servo driver

**Known limitation:** the thumb and index finger share one servo, and many ISL letters need two hands, so some signs can only be approximated. The digits 1–9 work best.

## Team
Harshit Bhatt and Virk Harman. IEEE in-house internship, Amity School of Engineering and Technology (ASET), Amity University Punjab. Supervisor: Dr. Monika Bharti.
