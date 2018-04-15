---
title: Deep Learning 입문
category: Deep Learning
tag: Deep Learning
---

이 포스팅은 딥러닝 관련 모르는 용어나 스스로 애매하게 이해하고 있는 개념을 정리하려는 용도로 작성하였습니다.

## Computer Vision

* Encoder & Decoder & AutoEncoder

<center><a href="https://imgur.com/CHg3Fy3"><img src="https://i.imgur.com/CHg3Fy3.png" width="500px" height="400px" title="source: imgur.com" /></a></center>

Let's first understand where the motivation for such layers come from: e.g. a convolutional autoencoder. You can use a convolutional autoencoder to extract featuers of images while training the autoencoder to reconstruct the original image. (It is an unsupervised method.)

Such an autoencoder has two parts: The encoder that extracts the features from the image and the decoder that reconstructs the original image from these features. The architecture of the encoder and decoder are usually mirrored.

* end-to-end

* Paired data - Unpaired data

* transfer learning

* 모노토닉 얼라이언스: 번역시 단어가 1대1 대응한다.

* attention

* local attention

* LAB : RGB 대신 색깔을 표현하는 방법

CIE L*a*b* 색 공간에서 L* 값은 밝기를 나타낸다. L* = 0 이면 검은색이며, L* = 100 이면 흰색을 나타낸다. a*은 빨강과 초록 중 어느쪽으로 치우쳤는지를 나타낸다. a*이 음수이면 초록에 치우친 색깔이며, 양수이면 빨강/보라 쪽으로 치우친 색깔이다. b*은 노랑과 파랑을 나타낸다. b*이 음수이면 파랑이고 b*이 양수이면 노랑이다.

## NLP


- SegNet
- Check board 가 나타난다.
- end-to-end 모델: 종착역은 string?

- Semantic Segmentation
==> predefined 된 class로 multiclass Classification: 픽셀 별로 분류

vs

- Instance Segmentation
각각의 Instance로 구분 ==> 겹치는 픽셀들 보완..

## 모델 관련 용어

- Unsampling :


- Upsampling & Downsampling :
