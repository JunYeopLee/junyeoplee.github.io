## Mobile Video Object Detection with Temporally-Aware Feature Maps

### 1. Abstract
  * Paper : https://arxiv.org/abs/1711.06368
  * Mobile 혹은 embedded device 환경에서 real-time video object dectection이 가능한 모델 제안
  * SSD(w Moblienet) depthwise separable convolution 적용, Bottleneck-LSTM을 통해 computation time을 현저히 줄임
  
### 2. Network architecture
  * **Overview**
    * <img src="FIGURES/MVOD/overview.PNG">
    * Video를 Input image I의 Seqeunece로 나타냄. 각 Time step마다 [SSD]()이용하여 Image object detection을 하는 것이 기본 아이디어.
    * Image object detection의 결과를 결합하여, Video object detection을 수행하려는 과정
      * 본 논문의 경우 LSTM 구조를 feture level에 집어 넣음으로써, unified architecture로 Video object detection 을 수행하도록 디자인
      * Hidden state가 2-D tensor인 convolutional LSTM 사용
      * 유사하게 Video object detection에 LSTM을 사용한 모델로 [LRCN](), [ROLO]()등이 있음.
      * 본 논문에서 제시한 모델의 경우, LSTM layer가 network의 최종단계에서 postprocessing 역할을 수행하는 LRCN, ROLO와 다르게, feature level에서 적용된다는 점에서 차별점이 있음
  
  * **Integrating Convolutional LSTMs with SSD**
    * 단순히 전체 구조 중간중간에 LSTM layer를 집어 넣게되면, 연산량이 매우 증가해 real-time detector가 될 수 없음
    * SSD의 convolution 연산을 모두 depthwise separable convolution으로 변경
    * Bottleneck LSTM 구조 제안. 
    * 실제 모델에서는 Conv13 layer + SSD feature maps(4개)뒤에만 선택적으로 LSTM layer 적용하여 실험
      
  * **Feature Refinement with LSTMs**
    * <img src="FIGURES/MVOD/refinement_LSTM.PNG">
    * 
    
  * **Efficient Bottleneck-LSTM layers**
    * <img src="FIGURES/MVOD/Bottleneck_LSTM.PNG">
    * classifier는 feature map의 각 cell마다 category score(c개의 class에 대한 확률분포)와 shape offset(cx, cy, width, heigth)을 생성
    
  * **Architecture**
    * 전체 layer 구조는 위와 같음 (Conv 13 layer 뒤에 LSTM이 있는 모델이 가장 성능이 좋았다고 함)
    
    
### 3. Results
  * **Experiments**
    * <img src="FIGURES/MVOD/default_box.PNG">
  * **Ablation Study**
    * <img src="FIGURES/SSD/res3.PNG">
