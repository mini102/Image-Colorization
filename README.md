# Image-Colorization
Tensorflow's tutorial

## Reference
> Tensorflow's tutorial
> https://colab.research.google.com/github/moein-shariatnia/Deep-Learning/blob/main/Image%20Colorization%20Tutorial/Image%20Colorization%20with%20U-Net%20and%20GAN%20Tutorial.ipynb

## data set
"face images - celeba" 
jpg 이미지 파일만 대량으로 다운
> https://www.kaggle.com/jessicali9530/celeba-dataset

## Theoretical background / Algorithm 요약
RGB color space : 기본적으로 입력 받는 이미지에 들어있는 color information.
                픽셀이 얼마나 많은 Red, Green, Blue인지를 나타냄.
 
 ![image](https://user-images.githubusercontent.com/73246476/153519975-87e93efe-ed1f-40dc-a7a4-ab0634354f18.png)
 L*a*b color space : L channel은 각 픽셀의 밝기, *a channel은 각 픽셀의 녹색-빨간색 양,
 *b channel은 각 픽셀의 노란색-파란색의 양
 ![image](https://user-images.githubusercontent.com/73246476/153520014-98eabdfc-0c36-4ab0-a1c7-2788833b87d7.png)

채색을 위해 모델을 훈련시키려면 gray scale 이미지를 제공해야 하며 색상이 다양하기를 바라기 때문에, RGB color space가 아닌 L*a*b color space를 사용한다.
더 구체적으로는, L*a*b를 사용할 때 모델(gray scale 이미지)에 L 채널을 제공하고 모델이 다른 두 채널(*a, *b)을 예측하도록 하고 예측 후에 모든 채널을 연결하는 방식으로 채색된 이미지를 모델로부터 얻을 수 있다.
이 접근 방식에서는 두 가지 손실이 사용된다. 즉, 회귀 작업이 되는 L1 손실(추가 loss func)과 감독되지 않은 방식으로 문제를 해결하는 데 도움이 되는 적대적(GAN) 손실(BCELoss())이다.
L1 loss를 사용하는 이유는 색칠한 이미지가 원래의 흑백이미지를 제대로 반영하고 있다는 보장이 없기 때문이다.

![image](https://user-images.githubusercontent.com/73246476/153520038-74663a7a-ea4b-4c66-8180-29d7d14776c2.png)

![image](https://user-images.githubusercontent.com/73246476/153519435-aacfb19a-f8f0-4219-8922-c98955f3eccf.png)
![image](https://user-images.githubusercontent.com/73246476/153519460-54d63d05-2126-4f92-ace2-990526183c52.png)

- generator network와 discriminator network의 structer
![image](https://user-images.githubusercontent.com/73246476/153519548-3798a61d-63b5-46d5-925f-82f3e747da8c.png)

보통 DCGAN과의 차이점 (image colorization에 사용되는 GAN과의):

● 두 가지 loss function(L1, GANLoss) 사용 
     ∵ L1은 이미지 자체 불변성을 유지하기 위함
         GANLoss는 netG와 netD의 
         최적화를 위한 loss

● netG의 input : original image - color 
    parameter :  random noise (color 역할)

**최종적으로 discriminator에서 판별하고 generator에서 color image를 생성하는 것을 반복해 최적으로 색칠된 color image를 도출하도록 유도**

## **GAN(Deep Convolutional Generative Adversarial Networks)**
:Unsupervised Learning의 한 종류의 대표적인 모델로서, generator network와 discriminator network가 서로를 견제하며 입력 데이터의 분포와 가장 비슷한 분포를 가진 가중치 (W)를 출력하는 프로그램
## **GAN의 목적**
: generator가 최대한 진짜 같은 가짜 이미지를 생성하게 하는 것.
## **장점**
: input을 학습한 model이 직접 input의 분포를 따르는 비슷한 이미지를 ‘생성’해내고, input에 정답 레이블 등 추가적인 정보가 크게 필요하지 않아 input의 제한적인 부분이 별로 없음.
## **Result**
![image](https://user-images.githubusercontent.com/73246476/153519714-6b2407ea-d43c-46b0-b741-2a4163c510e9.png)
> first row : 입력 이미지에서 color를 제거한 model의 입력
> second row : 학습한 model이 다시 칠한 img. (= generator의 최종 출력)
> third row : training 할 때 input한 원본 color img(정답 이미지)




