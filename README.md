# Nvidia AI Specialist Certification

# Title

Development of an Automatic Tennis Ball Collection System Using AI Object Recognition

---

# Overview

If a tennis lesson lasts for 30 minutes, around 120 balls are scattered across the court. Collecting the balls manually requires considerable time and effort, which reduces the lesson's efficiency. To address this, we plan to develop an automated tennis ball collection system using AI object recognition.

With recent advancements in artificial intelligence and autonomous driving technology, object recognition-based automation solutions are increasingly being applied to the sports industry. This project aims to combine AI object recognition and autonomous driving technology to detect and collect tennis balls automatically, allowing users to focus more on training or lessons.

For this project, yolov5 will be used to create an object recognition system that distinguishes tennis balls and the net.

---

# Video Acquisition Method

The videos were recorded on a tennis court by throwing balls manually. `video 1`, `video 2`, and `video 3` were used for AI training, while `verify 1`, `verify 2`, `verify 3` and `verify 4` were used for verification.

#### Main Videos
- [Video 1](https://youtu.be/o5Uc-7suMZc)
- [Video 2](https://youtu.be/Gbu-RIKtGzA)
- [Video 3](https://youtu.be/SYhXP_JmIM8)

#### Verification Videos
- [Verify 1](https://youtu.be/pokUUyolhIU)
- [Verify 2](https://youtu.be/isHJXVD45ig)
- [Verify 3](https://youtu.be/NK5Vv6xTxD4)
- [Verify 4](https://youtu.be/76Jy8iYbe-A)

---

# Project Progress

## Yaml file configuration

The tool used for image labeling is [DarkLabel](https://github.com/darkpgmr/DarkLabel). To ensure correct labeling, the `darklabel.yaml` file needs to be modified. 

To label tennis balls, net, on the tennis court, a new `tennis_class` is created with "ball", "net" as elements, and set as the `default classes_set`. Since the labeling will be done in `format1`, the `classes_set` for `format1` is also set to `tennis_class`.

![image](https://github.com/user-attachments/assets/d83acc19-37eb-40fa-a833-e978a3c8beec)

![image 1](https://github.com/user-attachments/assets/de5f805e-1ea4-4559-8582-b801f350c3c6)

![image 2](https://github.com/user-attachments/assets/6f88ff4b-aeda-4f2e-a53e-9b76d53e7940)

When you run DarkLabel and select `format1(darknet yolo)`, you can confirm that the elements of `tennis_class`—“ball,” “net”—are displayed correctly.

---

## Convert Video to Images

![image 3](https://github.com/user-attachments/assets/fe436f7e-27d8-45ad-b634-8c36608f37f0)

Using DarkLabel's "Save as Images" feature, convert `video 1`, `video 2`, and `video 3` into PNG image files for labeling. The total number of images is 811.

#### Train Images
- [View Train Images](https://drive.google.com/drive/folders/1In3oX1hfmI8Awwq1Pb3SyBhHQCMCGJol?usp=drive_link)

---

## Data Labeling

Labeling was performed on a total of 811 images using `DarkLabel`, with labels assigned to the predefined classes “ball” and “net.”

Each label file is saved as a “txt” file in the format `[classid, ncx, ncy, nw, nh]`, where each element represents the following: the class ID of the object, the x-coordinate of the bounding box center, the y-coordinate of the bounding box center, the width of the bounding box, and the height of the bounding box.

This dataset will be used to train the `yolov5` model. Additionally, all coordinates and size values are normalized to the image dimensions, ensuring consistent positional information across various resolutions.

![d](https://github.com/user-attachments/assets/0f0225fa-b394-496c-978e-308e60266a32)

![d2](https://github.com/user-attachments/assets/0a18b794-6885-4d68-be2b-7ea9e03f3b93)

![image 5](https://github.com/user-attachments/assets/56a8c853-2d89-4a3d-aa60-f7c537c8da3b)

#### Labels
- [View Labels](https://drive.google.com/drive/folders/1tjzO498etTYU7tAJrPSASoepTzWGXcrg?usp=drive_link)

---

## Training the yolov5 model

### yolov5 repository clone

After cloning the [yolov5](https://github.com/ultralytics/yolov5) repository, install the packages listed in the `requirements.txt` file.

![clone](https://github.com/user-attachments/assets/e280029a-9df5-40bf-9599-f4166756a585)

---

### Create_npy

The image files in the `imagepath` are loaded, preprocessed in bulk, and saved into a single `.npy` file. The preprocessed image files will be used later for model training and evaluation.

![create_npy](https://github.com/user-attachments/assets/ecd2d6d5-6dc1-4600-b5d0-3aa6bfc4494e)

---

### Training Model

Based on the contents of the `data.yaml` file, the model training is carried out. The progress can be continuously monitored in the console, and it took approximately 90 minutes.

![Training_Model](https://github.com/user-attachments/assets/aff443b1-cff6-4d86-acca-a0327a89adff)

---

# Results

## PR Curve

![image 6](https://github.com/user-attachments/assets/75b0d9a5-5e45-4ea2-9412-de3e53112727)

---

## Confusion Matrix

![image 7](https://github.com/user-attachments/assets/5caf1574-f459-4fb0-a458-9d34d305357f)

---

## Result

![image 8](https://github.com/user-attachments/assets/bb11429b-ee82-4f9f-96c6-f2bf5af20a85)

---

## Label batch - Prediction batch

Label_0 batch
![image 9](https://github.com/user-attachments/assets/b2069d96-8c7c-4a23-9f06-7b6129f89014)

Prediction_0 batch
![image 11](https://github.com/user-attachments/assets/d9b77ea4-9a3c-476b-a467-1395d5207670)

Label_1 batch
![image 10](https://github.com/user-attachments/assets/f980358b-ed77-4f7e-8892-5f488d6df382)

Prediction_1 batch
![image 12](https://github.com/user-attachments/assets/7a1018d6-9539-4123-bce2-938aa335b02b)

#### Results
- [View Results](https://drive.google.com/drive/folders/1H2VWUAt1PbAnyjVDk_D8AM93S3zwPNJk?usp=drive_link)

---

# Detect

After training was completed, detection was performed on the images used for training and on `verify 1`, `verify 2`, `verify 3`, `verify 4`.

![image 13](https://github.com/user-attachments/assets/9cdca6b0-3091-49b9-bc49-d3603c224969)

---

### Image

Detection was performed on all 810 images used for training, and the detection results for the 200th, 400th, 600th, and 800th images are shown.

![image 14](https://github.com/user-attachments/assets/1a4ebcfd-2229-4f0c-8c3b-be50c480643d)

![image 15](https://github.com/user-attachments/assets/b76d85a0-f989-461a-a7b6-1ead4dad0ac3)

![image 16](https://github.com/user-attachments/assets/ce22fa77-1457-4204-849c-1ca9a928a0ca)

![image 17](https://github.com/user-attachments/assets/9f8ef530-7747-46c1-99f7-d5c06f1ee54d)

#### Detect Images
- [View Detect Images](https://drive.google.com/drive/folders/1J0z9ccU6mSd2s33q3UZX5Y-Hz-5si1yf?usp=sharing)

---

### Video

![image 18](https://github.com/user-attachments/assets/aeb09c38-bf9a-406f-a759-479b968a0975)

![image 19](https://github.com/user-attachments/assets/331241fd-af9b-4d55-8fb2-62063cff99c9)

![image 20](https://github.com/user-attachments/assets/b5040225-4acd-49b8-8158-06a4bd543870)

![image 21](https://github.com/user-attachments/assets/9c0d08d3-7dc9-453a-b19d-aa670f7a73ae)

#### Verified Videos
- [Verified 1](https://youtu.be/Y-nVvaKvZls)
- [Verified 2](https://youtu.be/-QSF3mO4yWk)
- [Verified 3](https://youtu.be/JvRVSXdWuws)
- [Verified 4](https://youtu.be/0TclB7A066k)

---

# Conclusion

The `Confidence Score` for the tennis ball is mostly between 0.8 and 0.9, indicating a reliably high value. In contrast, the net has a relatively low score between 0.3 and 0.5.

The tennis ball has a clear shape with distinct boundaries, making it easier for the model to detect it as an independent object. However, the net has a long, stretched form with small, repeating holes, and the background is visible through it, making it difficult for the model to clearly identify its edges.

In particular, the images used for training show that the net’s `Confidence Score` is between 0.7 and 0.8. This suggests that training the model with data containing more varied shapes and angles, along with appropriate post-processing, could lead to improved scores.

---

If you want to check all the elements used in the project, please visit the following link

→ https://drive.google.com/drive/folders/1nwAzmCokE3HGNDCl2NQMne5XMaeQTuKp?usp=drive_link