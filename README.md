# Nvidia AI Specialist Certification

# 주제(Title)

AI 객체 인식을 활용한 테니스 공 자동 수거 시스템 개발

Development of an Automatic Tennis Ball Collection System Using AI Object Recognition

---

# 개요(Overview)

테니스 레슨이 30분 동안 진행된다면, 약 120개의 공이 코트 전체에 흩어진다. 공을 직접 수거하는 데는 상당한 시간과 노력이 소요되어 레슨의 효율성이 떨어진다. 이를 해결하기 위해, AI 객체 인식을 활용한 테니스 공 자동 수거 시스템을 개발하려고 한다.

최근 인공지능과 자율 주행 기술이 발전함에 따라, 객체 인식을 통한 다양한 자동화 솔루션이 스포츠 분야에도 적용되고 있다. 이 프로젝트는 AI 객체 인식과 자율 주행 기술을 결합하여 테니스 공을 자동으로 탐지하고 수거하는 시스템을 개발하여 사용자들이 훈련이나 레슨에 더욱 집중할 수 있도록 돕는 것을 목표로 한다.

이번 프로젝트에서는 yolov5를 사용하여 테니스 공과 네트를 구분하는 객체 인식 시스템을 만드는 작업을 할 것이다.

If a tennis lesson lasts for 30 minutes, around 120 balls are scattered across the court. Collecting the balls manually requires considerable time and effort, which reduces the lesson's efficiency. To address this, we plan to develop an automated tennis ball collection system using AI object recognition.

With recent advancements in artificial intelligence and autonomous driving technology, object recognition-based automation solutions are increasingly being applied to the sports industry. This project aims to combine AI object recognition and autonomous driving technology to detect and collect tennis balls automatically, allowing users to focus more on training or lessons.

For this project, yolov5 will be used to create an object recognition system that distinguishes tennis balls and the net.

---

# 영상 취득 방법(Video Acquisition Method)

영상은 테니스 코트에서 공을 던지는 방식으로 직접 촬영했다. `video 1`, `video 2`, `video 3`은 AI 학습에 사용했고, `verify 1`, `verify 2`, `verify 3`, `verify 4` 는 검증하는데 사용했다.

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

# 프로젝트 진행(Project Progress)

## Yaml 파일 설정(Yaml file configuration)

이미지 라벨링을 위한 도구로는 [DarkLabel](https://github.com/darkpgmr/DarkLabel)을 사용했다. 올바른 라벨링을 위해서는 `darklabel.yaml` 파일을 수정해야 한다.
테니스 코트에서 테니스 공, 네트를 라벨링 하기 위해 “ball”, “net”를 원소로 가지는 `tennis_class`를 새롭게 만들고 `default classes_set`으로 설정한다. 또한, `format1`로 진행할 것이므로 `format1`의 `classes_set`도 `tennis_class`로 설정한다.

The tool used for image labeling is [DarkLabel](https://github.com/darkpgmr/DarkLabel). To ensure correct labeling, the `darklabel.yaml` file needs to be modified. 

To label tennis balls, net, on the tennis court, a new `tennis_class` is created with "ball", "net" as elements, and set as the `default classes_set`. Since the labeling will be done in `format1`, the `classes_set` for `format1` is also set to `tennis_class`.

![image](https://github.com/user-attachments/assets/d83acc19-37eb-40fa-a833-e978a3c8beec)

![image 1](https://github.com/user-attachments/assets/de5f805e-1ea4-4559-8582-b801f350c3c6)

![image 2](https://github.com/user-attachments/assets/6f88ff4b-aeda-4f2e-a53e-9b76d53e7940)

DarkLabel을 실행하고 `format1(darknet yolo)` 를 선택하면 `tennis_class` 의 원소인 “ball”, “net”이 정상적으로 나오는 것을 확인할 수 있다.

When you run DarkLabel and select `format1(darknet yolo)`, you can confirm that the elements of `tennis_class`—“ball,” “net”—are displayed correctly.

---

## 영상을 이미지로 변환(Convert Video to Images)

![image 3](https://github.com/user-attachments/assets/fe436f7e-27d8-45ad-b634-8c36608f37f0)

DarkLabel의 “Save as Images” 기능을 사용하여 라벨링에 사용할 `video 1`, `video 2`, `video 3`을 PNG 형식의 이미지 파일로 변환했다. 이미지는 총 811개이다.

Using DarkLabel's "Save as Images" feature, convert `video 1`, `video 2`, and `video 3` into PNG image files for labeling. The total number of images is 811.

#### Train Images
- [View Train Images](https://drive.google.com/drive/folders/1In3oX1hfmI8Awwq1Pb3SyBhHQCMCGJol?usp=drive_link)

---

## 데이터 라벨링(Data Labeling)

라벨링은 `DarkLabel`을 사용하여 총 811개의 이미지에 수행하였고, 레이블은 미리 지정한 “ball”과 “net”이다.

각 label 파일은 `[classid, ncx, ncy, nw, nh]` 양식의 “txt” 파일로 저장되며, 각각 객체의 클래스 id, 바운딩 박스의 중심점의 x좌표, 바운딩 박스의 중심점의 y좌표, 바운딩 박스의 너비, 바운딩 박스의 높이를 의미한다.

이 데이터는 `yolov5` 모델을 학습시키는 데 사용할 예정이다. 또한, 모든 좌표와 크기 값이 이미지 크기에 대해 정규화되어 있어, 다양한 해상도에서도 일관된 위치 정보를 제공한다.

Labeling was performed on a total of 811 images using `DarkLabel`, with labels assigned to the predefined classes “ball” and “net.”

Each label file is saved as a “txt” file in the format `[classid, ncx, ncy, nw, nh]`, where each element represents the following: the class ID of the object, the x-coordinate of the bounding box center, the y-coordinate of the bounding box center, the width of the bounding box, and the height of the bounding box.

This dataset will be used to train the `yolov5` model. Additionally, all coordinates and size values are normalized to the image dimensions, ensuring consistent positional information across various resolutions.

![d](https://github.com/user-attachments/assets/0f0225fa-b394-496c-978e-308e60266a32)

![d2](https://github.com/user-attachments/assets/0a18b794-6885-4d68-be2b-7ea9e03f3b93)

![image 5](https://github.com/user-attachments/assets/56a8c853-2d89-4a3d-aa60-f7c537c8da3b)

#### Labels
- [View Labels](https://drive.google.com/drive/folders/1tjzO498etTYU7tAJrPSASoepTzWGXcrg?usp=drive_link)

---

## yolov5 모델 학습(Training the yolov5 model)

### yolov5 repository clone

[yolov5](https://github.com/ultralytics/yolov5)의 레포지토리를 clone 후, `requirements.txt`에 명시된 패키지들을 설치한다.

After cloning the [yolov5](https://github.com/ultralytics/yolov5) repository, install the packages listed in the `requirements.txt` file.

![clone](https://github.com/user-attachments/assets/e280029a-9df5-40bf-9599-f4166756a585)

---

### Create_npy

`imagespath`에 있는 이미지 파일들을 불러와서 일괄적으로 전처리한 후, 하나의 `.npy` 파일로 저장한다. 전처리된 이미지 파일은 추후에 모델 학습과 평가 등에 사용된다.

The image files in the `imagepath` are loaded, preprocessed in bulk, and saved into a single `.npy` file. The preprocessed image files will be used later for model training and evaluation.

![create_npy](https://github.com/user-attachments/assets/ecd2d6d5-6dc1-4600-b5d0-3aa6bfc4494e)

---

### Training Model

`data.yaml` 파일의 내용을 토대로 모델의 학습을 진행한다. 콘솔에서 진행도를 계속 확인할 수 있으며, 약 90분 정도 소요되었다.

Based on the contents of the `data.yaml` file, the model training is carried out. The progress can be continuously monitored in the console, and it took approximately 90 minutes.

![Training_Model](https://github.com/user-attachments/assets/aff443b1-cff6-4d86-acca-a0327a89adff)

---

# 결과(Results)

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

![image 9](https://github.com/user-attachments/assets/b2069d96-8c7c-4a23-9f06-7b6129f89014)

![image 10](https://github.com/user-attachments/assets/f980358b-ed77-4f7e-8892-5f488d6df382)

![image 11](https://github.com/user-attachments/assets/d9b77ea4-9a3c-476b-a467-1395d5207670)

![image 12](https://github.com/user-attachments/assets/7a1018d6-9539-4123-bce2-938aa335b02b)

#### Results
- [View Results](https://drive.google.com/drive/folders/1H2VWUAt1PbAnyjVDk_D8AM93S3zwPNJk?usp=drive_link)

---

# 탐지(Detect)

학습이 완료된 후, 학습에 사용한 이미지와 `verify 1`, `verify 2`, `verify 3`, `verify 4`, 에 대해 detect를 수행했다.

After training was completed, detection was performed on the images used for training and on `verify 1`, `verify 2`, `verify 3`, `verify 4`.

![image 13](https://github.com/user-attachments/assets/9cdca6b0-3091-49b9-bc49-d3603c224969)

---

### Image

학습에 사용된 810개 이미지 전체에 대해 detect를 진행하였으며, 200, 400, 600, 800번째 이미지의 detect 결과이다.

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
- [Verified 1](https://youtu.be/tt57ysrNfic)
- [Verified 2](https://youtu.be/fvbHZ20ShNs)
- [Verified 3](https://youtu.be/TMCXNwLt6ns)
- [Verified 4](https://youtu.be/H1BllTfTDxA)

---

# 결론(Conclusion)

ball의 `Confidence Score`는 대부분 0.8에서 0.9 사이의 신뢰할만한 수준의 높은 값을 가지지만, net은 0.3에서 0.5 사이의 상대적으로 낮은 값을 가지고 있다. 

테니스 공은 형태가 명확하고 뚜렷한 경계를 가지고 있어 모델이 이를 독립적인 객체로 탐지하기 쉽다. 하지만 네트는 길게 펼쳐진 형태에 작은 구멍이 반복적으로 존재하고, 배경이 겹쳐 보이기 때문에 모델이 경계를 명확히 구분하기 어렵다.  

특히 학습에 사용한 이미지의 net의 `Confidence Score`는 0.7에서 0.8 사이의 값을 가지는 것으로 보아, 더 다양한 모양과 각도의 데이터로 모델을 학습시키고, 적절한 후처리를 적용하면 개선된 값을 얻을 수 있을 것으로 기대한다.

The `Confidence Score` for the tennis ball is mostly between 0.8 and 0.9, indicating a reliably high value. In contrast, the net has a relatively low score between 0.3 and 0.5.

The tennis ball has a clear shape with distinct boundaries, making it easier for the model to detect it as an independent object. However, the net has a long, stretched form with small, repeating holes, and the background is visible through it, making it difficult for the model to clearly identify its edges.

In particular, the images used for training show that the net’s `Confidence Score` is between 0.7 and 0.8. This suggests that training the model with data containing more varied shapes and angles, along with appropriate post-processing, could lead to improved scores.