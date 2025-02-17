# Detect_covid19_from_CXR -- accuracy : 99.03%

## 사용 모델 : 
- Inception_Resnet_V2 사전 훈련모델의 feature_model -- 218MB -- lfs
- initial model --> tensorflow_hub
- final_model --> keras.applications

- CXR 이미지를 분류할 분류모델 --3MB

### Covid19_prediction_with_CXR.ipynb /code review
 
- 함수 하나로 구성된 코드로 예측을 수행할 CXR 이미지의 경로,
- feature_model의 경로, 분류기 모델의 경로를 차례로 파라미터로 넣어주면
- COVID19, NORMAL, PNEUMONIA 세 가지로 분류하게 된다. 정확도는 95%임을 확인하였다.

### InceptionResnet.ipynb /code review
- feature_model에 inceptionResnet_V2의 feature_model을 다운로드 받고 변수에 저장한다.
- imageDataGenerator로 파일의 전처리를 수행한 후 하나씩 feature_model로 예측을 수행한 후
리스트로 저장한다. (train/ valid set)
- 분류기 모델에 특징을 분석한 리스트를 훈련과 검증 데이터로 입력 후 모델을 훈련시킨다. -- 정확도 약 95%
- 모델을 사용하여 예측을 진행한다. 

----------------------------------------------------------------------
### HeatMap+Prediction-inception_Resnet_V2.ipynb /code review
- feature_model과 model을 지정된 경로에서 load하여 사용한다. tensorflow_hub는 사용하지 않았다.
- feature_vector에서 판단의 근거가 된 특징부분을 heatmap으로 나타내었다. CAP(Class Activation Mapping)
### Grad-CAM class activation visualization.ipynb / code review
- feature_model은 사전훈련모델 keras.applications.inception_resnet_v2에서 include_top=False로 가져왔고, classifier_model은 직접 구성하였다.
- 분류모델은 feature_model을 통해 나온 feature_vector로 학습시켰다.
- feature_vector에서 판단의 근거가 된 특징부분을 heatmap으로 나타내었다. CAP(Class Activation Mapping)
- model 예측 시각화 포함.
<br><br><br>
- 경로설정 필요.

## 사용데이터
initial_model<br>
- CXR이미지3클래스_각 약1000개 (covid19 , normal, pneumonia)<br>
- reference : kaggle datasets<br>
- https://www.kaggle.com/prashant268/chest-xray-covid19-pneumonia
- https://www.kaggle.com/tawsifurrahman/covid19-radiography-database.

final_model : CXR이미지 (covid19 : 1300, normal : 5000, pneumonia : 5000)
- reference : kaggle datasets
- https://www.kaggle.com/prashant268/chest-xray-covid19-pneumonia
- https://www.kaggle.com/tawsifurrahman/covid19-radiography-database.
- https://www.kaggle.com/praveengovi/coronahack-chest-xraydataset
## result
- initial model<br>
예측 정확도 : 96.55% <br>

- final_model <br>
accuracy : 99.03%, loss : 0.05<br>
- confusion matrix<br>
- a:covid/b:normal/c:pneumonia<br>
![screenshot_20171221-151714](https://github.com/whiteBerryJ/Detect_covid19_from_CXR/blob/master/covid_model_and_result_final/Confusion_matrix_covid_normal_pneumonia.png) <br>

- 배포 서버 : 
- 프로젝트 github address : https://github.com/Lagom92/CXR_AI


## 치명적인 오류 및 해결 
- 훈련을 잘 못 한 것인지 kaggle의 데이터가 아니면 예측 정확도가 매우 낮음. <br>
ex) 인터넷에서 저장한 다른 사진 및 그냥 핸드폰으로 찍은 사진. <br>
--> 인터넷에서 저장한 사진 normal 100장과 covid 50여 장 정도를 포함하여 학습하니 대체로 모든 상황에서 예측을 잘 수행하였다.<br>
사용한 사진들의 출처 <br>
https://radiopaedia.org/cases/normal-cxr-and-lateral?lang=us <br>
http://www.chestx-ray.com/index.php/education/normal-cxr-module-train-your-eye#!100
