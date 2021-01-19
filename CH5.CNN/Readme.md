# Chapter5. CNN

## 3. ResNet으로 컬러데이터셋에 적용하기
### 3.1 ResNet이란?
- conv 출력에 전의 전 계층에 쓰인 입력을 더함으로써 특징이 유실되지 않도록 한다.
- 아래그림은 ResNet의 구조이다.\
![image](https://user-images.githubusercontent.com/70633080/104994361-a3baed00-5a67-11eb-81ba-81bead3078b0.png)\
![image](https://user-images.githubusercontent.com/70633080/104994290-8a19a580-5a67-11eb-80d0-496fdd24b405.png)
### 3.2 CIFAR-10 dataset
- 32 * 32 크기의 이미지 6만개를 포함한다.
- 자동차, 새, 고양이, 사슴 등 10가지의 클래스가 존재한다.
- 컬러이미지의 픽셀값은 몇가지 채널들로 구성된다.
    - 채널 : 이미지의 색상 구성요소 ex)RGB, YCBCR
- 24bit의 컬러이미지(RGB 각각 8bit)
- 예제에서는 과적합을 방지하기위해 학습용 데이터셋에 RandomCrop과 RandomHorizontalFlip 같은 노이즈를 추가한다.
- 코드참고
### 3.3 CNN을 깊게 쌓는 방법
- 기존 신경망은 layer를 깊게 쌓았을때,학습이 진행될수록 최초 입력 이미지에 대한 정보가 소실되는 문제가 있다.
    - gradient exploding
    - vanishing gradient
- 이를 해결한 구조가 ResNet에 사용된다.
- 핵심은 네트워크를 작은 block인 Residual Block으로 나누었다는 점이다.
- Residual : block의 출력에 입력이었던 x를 더한다. 
- 입력과 출력의 관계보다 차이를 학습하는 것이 성능이 더 좋다는 가정.
- nn.BatchNorm2d : 배치정규화를 수행하는 계층
        - 학습률을 너무 높게 잡으면 기울기가 소실되거나 발산하는 증상을 예방하여 학습과정을 안정화하는 방법이다.
        - 즉, 학습 중 각 계층에 들어가는 입력을 평균과 분산으로 정규화함으로써 학습을 효율적으로 만들어준다.
        - Dropout과 같은 효과를 내는 장점이 있다.
- 예제에서 쓰이는 resnet에서 채널은 증폭해주는 모듈은 따로 shortcut module을 갖게 된다. 이는 add연산으로 입력과 출력을 연결해주는 것.
