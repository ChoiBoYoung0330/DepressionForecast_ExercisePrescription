# [건강지표를 통한 우울 지수 예측과 운동 처방]

## Business Understanding
### Business Problem
포스트코로나 시대에서는 질환 예방관리와 달리 "보편적 건강보장"을 목적으로 디지털 기기를 활용한 개인 맞춤 의료/건강/돌봄에 관심이 높아지고 있다. 주요 국가인 미국 등에서는 보편적인 건강을 바탕으로 만성질환과 "정신건강 문제"를 같이 해결하려는 노력이 강화되고 있다고 한다.

2021년 정신건강 실태조사에 따르면, 성인 4명 중 1명이 평생 한 번 이상 정신건강 문제를 경험하고 있다고 한다. 이러한 정신건강 문제는 기질적 원인과 환경적 원인인 스트레스가 있다고 한다. 2021년 대한민국 성인 중 스트레스를 '대단히 많이' 또는 '많이' 느끼는 스트레스 인지율은 27.8%로 한국인들은 객관적으로 높은 수치의 스트레스를 겪고 있음을 알 수 있다. 하지만 높은 수준의 스트레스 지수와 달리 한국의 지난 1년간 정신건강 서비스 이용률은 7.2%로, 미국 43.1%(15년), 캐나다 46.5%(14년), 호주 34.9%(09년)에 비해 현저히 낮은 수준임을 알 수 있다. 이러한 스트레스는 세계 경제에 약 1조 달러의 손실을 주는 것으로 추산되는 우울과 가장 많이 동반된다.

정신건강 치료에 있어 약물치료와 구조화된 운동 치료는 매우 효과적임을 박(2018) [1]의 연구를 통해 알 수 있고, 활동적인 움직임을 통해 스트레스 완화효과를 얻을 수 있다는 사실은 허(2019) [2] 및 최 외 (2011) [3]의 연구에서도 확인되었다. 또한 남과 김(2020) [4]의 연구를 통해 다양한 건강지표를 활용하여 스트레스 및 우울질환을 예측할 수 있음을 확인할 수 있다.

정부/민간에서 출시한 다양한 건강 앱(서울시 블루터치, 수원시 마음건강 로드맵, 삼성 더헬스, 온 핏 헬스체크업)들은 동시에 스트레스 예측과 맞춤형 운동 처방을 지원하지 않는다. 따라서 “건강지표를 통한 스트레스 지수 예측과 운동 처방”을 목표로 프로젝트를 진행하였다.

### Business Goal
1. 건강지표를 통해 객관적인 우울 예측 및 우울 지수가 높은 사람들의 특성 파악
2. 건강지표를 통해 객관적인 운동 처방 및 맞춤형 운동 목표 제안
스트레스가 과도한 사용자의 우울 지수를 줄이기 위해 구체적인 운동 처방 및 운동 목표를 제시하여 노동생산성 증진 및 국민 행복감 향상에 도움이 되고자 한다.

### Benefit
1. 노동 생산성 증진 및 스트레스 동반상병 발병률 감소
2. 공공 헬스케어 앱의 일부 서비스 기능으로 편입 가능하도록 설계
3. 기존 건강 공공데이터의 정신건강 분야로의 확장 가능성

## Data Understanding
[국민건강영양조사]

대한민국의 약 1만명을 대상으로 조사한 건강행태, 영양, 우울 점수 등의 지표들에 대한 데이터셋. 총 7,992개의 records와 815개의 attribute로 구성. 우울 예측 모델에 사용

[체력측정 및 운동처방 종합 데이터]

국민체육진흥공단에서 관리하고 있는 체력측정데이터셋. 개인별 신장, 체중 등의 신체 데이터와 악력 등의 체력측정 데이터, 그에 따른 운동처방 데이터가 포함. 총 1,589,323개의 records와 52개의 attribute로 구성. 운동 처방 모델에 사용

## Data Peparation
### 변수선택
[국민건강영양조사]
|column명|설명|column명|설명|
|:--:|:--:|:--:|:--:|
|sex|성별|HE_BMI|체질량지수|
|age|나이|body_fat|체지방률|
|HE_sbp|수축기 혈압|r_average|오른손 악력|
|HE_dbp|이완기 혈압|l_average|왼손 악력|
|HE_ht|신장|depression_scale|우울상태|
|HE_wt|몸무게|||

[체력측정 및 운동처방 종합 데이터]
|column명|설명|column명|설명|
|:--:|:--:|:--:|:--:|
|CNTER_NM|센터명|MIN_BLOOD_PRESSURE|이완기 최저혈압|
|AGE_NAME|연령 그룹|MAX_BLOOD_PRESSURE|수축기 최고혈압|
|AGE|나이|L_GRIP|악력_좌|
|GRADE|체력 등급|R_GRIP|악력_우|
|GENDER|성별|FLEXIBILITY|유연성|
|HEIGHT|신장|BMI|체질량지수|
|WEIGHT|체중|RELATIVE_GRIP|상대악력|
|BODY_FAT|체지방률|PRPGRAM|운동 처방 내용|

### 이상치/결측치 제거
[국민건강영양조사]
1. mh_PHQ_S : 본 프로젝트에서 key attribute에 해당하는 값이기 때문에, 결측치가 있는 데이터는 전부 제거한다.
2. HE_sbp, HE_dbp : 혈압은 다른 데이터로 유추할 수 없기 때문에 결측치가 존재하면 전부 제거한다.
3. HE_ht, HE_wt : 두 attribute 중 하나라도 결측치가 존재하면 모든 record를 제거한다.
4. HE_BMI : HE_ht, HE_wt 값을 수식을 통해 계산한다.
   BMI = wt/ht^2
5. GS_mea_r_#, GS_mea_I_# : 3 차례에 걸친 악력 측정값이므로 평균값을 사용한다. 이를 위해 결측치를 채워넣는다.
   1) 만약 3개 attribute 값이 전부 결측치라면 데이터를 삭제한다.
   2) 3개 중 하나라도 값이 존재한다면 나머지 값을 하나의 값으로 채운다.
   3) 3개 중 값이 두 개 존재한다면 나머지 값을 두 개의 평균으로 채운다.
6. mh_PHQ_S 값의 경우, 아래와 같이 카테고리가 구분된다.

   <img width="253" alt="image" src="https://github.com/ChoiBoYoung0330/DM-TermProject/assets/65601017/0ef85d53-4a44-40c2-9f75-5bd6c779026e">
   
   각 열별로 0부터 3까지 인덱스를 할당해서, 새로운 depression_scale 열을 생성한 뒤 데이터를 전처리한다. 
7. GS_mea_r_#, GS_mea_l_# : 3번 측정한 악력에 대해서 평균값인 새로운 r_average, l_average 생성 후 사용한다.
8. BMI, age, sex를 통해 체지방률을 계산한다.
   - 성인: 1.2 * BMI + 0.23 * 나이 - 10.8 * 성별(남성=1, 여성=0) - 5.4
   - 아동: 1.5 * BMI - 0.7 * 나이 - 3.6 * 성별(남성=1, 여성=0) + 1.4

[체력측정 및 운동처방 종합 데이터]
1. raw dataset의 각 attribute별 결측치 비율을 계산하여 10%가 넘는 attribute를 전부 제거한다. (총 52개의 attribute 중 30개가 제거되었다.)
2. 남은 attribute 중 모델 생성에 불필요한 다음 6 개의 attribute를 전부 제거한다. MBER_SEQ_NO_VALUE, MESURE_SEQ_NO, MESURE_PLACE_FLAG_NM ,INPT_FLAG_NM, MESURE_DE, MESURE_DE
3. 유의미한 attribute만 남았으므로 결측치가 존재하는 모든 column을 제거한다.
4. Z-score 방법을 이용하여 Z-score가 threshold = 3 이상인 값들을 이상치로 판정하고 해당 데이터를 제거한다.
5. 다음 과정을 통해 운동 처방 데이터셋의 PROGRAM(운동 처방 프로그램) attiribute의 데이터를 transaction data 형태로 변형한다.
   - 준비 운동, 본 운동, 마무리 운동 중 본 운동만 사용할 것이므로 본 운동을 제외한 데이터를 제거한다.
   - 불필요한 기호(‘, :, [, ])를 제거하고 중복되는 데이터 값이 존재하는 것을 방지하기 위해 공백을 제거한다.
6. 우울 예측 모델에 사용되는 변수들을 우울팀과 동일하게 변경한다.
   
   |변경 전|변경 후|변경 전|변경 후|
   |:--:|:--:|:--:|:--:|
   |GENDER|sex|WEIGHT|HE_wt|
   |AGE|age|BMI|HE_BMI|
   |MAX_BLOOD_PRESSURE|HE_sbp|BODY_FAT|body_fat|
   |MIN_BLOOD_PRESSURE|HE_dbp|R_GRIP|r_average|
   |HEIGHT|HE_ht|L_GRIP|l_average|
7. 우울 예측 모델을 이용하여 새로운 depression_scale attribute를 생성 후 우울 상태 값을 예측하여 사용한다.
8. Y값인 운동 처방이 Multi Label 이기 때문에 sklearn.preprocessing 모듈의 MultiLabelBinarizer 라이브러리를 import해 인코딩을 진행한다. 예시를 들자면, 운동의 종류가 총 29개인데, 어떤 sample은 [4, 12, 20], 또 다른 샘플은 [3, 4, 23. 25]와 같은 label을 가진다. 이러한 Multi Label을 [ 010001••100] 과 같은 형태로 binarization을 진행한다.

## Modeling
[우울 예측 모델]

사용 모델 기법: sciket-learn의 [Decision Tree] 사용
모델 생성
1. x_np: ‘depression_scale’을 제외한 위의 column을 df_x_train과 df_x_test에 사용하기 위해 numpy형태로 할당한다.
   - y_np: target attribute인 ‘depression_scale’을 df_y_train과 df_y_test에 사용하기 위해 지정한다.
   - 상태에 따라 1~4로 값을 주었던 ‘depression_scale’ column을 make_class(d) 함수를 만들어 ‘a’, ‘b’, ‘c’, ‘d’의 값으로 바꾼 값을 y_np에 할당한다.
   - training set과 test setdmf 7:3의 비율로 랜덤하게 분리한다.
2. 난수를 생성할 때 같은 코드를 실행할 때마다 같은 결과를 얻을 수 있도록 난수 생성 시드를 지정하여 모형을 학습한다.
   - 학습된 decision tree 모형을 시각화. 예측할 타겟 클래스 이름은 [‘a’, ‘b’, ‘c’, ‘d’]로 지정해주고 불순도를 나타내기 위해 impurity=True
   - accuracy = 약 0.7
3. 2는 과적합 되었을 가능성이 높아 4가지의 모든 우울 척도가 leaf node에 하나 이상 포함되는 조건을 만족하며 test 정확도가 가장 높은 모델을 찾기 위해 max_depth를 변경한다.
   - max_depth=10의 경우 accuracy가 약 0.8로 가장 높은 정확도를 기록한다.
   - precision score는 모델이 예측한 Positive 클래스 중 실제로 Positive인 샘플의 비율을 나타내는 지표로 위의 모델에서는 약 0.7이 도출된다.

[운동 처방 묶음 생성 모델]

사용 모델 기법 : R arules package를 이용한 Association Rule Discovery(한 사람에게 주어진 운동 처방 데이터는 그 사람의 신체 데이터를 기반으로 처방된 값이기 때문에 연관성, 규칙이 존재할 것이고 ARD를 통해 나온 운동 처방 묶음은 그 규칙을 대변하고 있을 것이라고 판단하여 ARD를 사용)
모델 생성
1. 운동 처방 데이터셋의 PROGRAM(운동 처방 프로그램) attiribute 의 transaction 객체를 생성한다.
2. Apriori 알고리즘을 이용해서 운동 처방 transaction data의 규칙을 발견한다.
   - 지지도(support)는 0.008, 신뢰도(confidence)를 0.8로 지정하고 이 값을 넘는 규칙만을 출력해낸다.
   - 하나의 운동 처방 묶음의 개수가 6개가 적합하다고 판단하였고, support 값을 낮추더라도 묶음의 개수가 6개인 규칙은 그대로이기 때문에 지지도와 신뢰도 값이 각각 0.008, 0.8이 적절하다고 판단했다.
   - 위 과정을 통해 나온 규칙중 운동 처방 묶음의 개수가 6개인 묶음을 뽑아내어 사용한다.

[운동 처방 모델]

사용 모델 기법 : Sci-lit learn 모듈의 tree모듈의 DecisionTreeClassifier 라이브러리를 활용해 Decision Tree 생성
모델 생성
1. Train set, Test set split을 진행함. 비율은 7 대 3으로 했으며 모델 재현을 위해 random_state = 42로 고정한다.
2. Overfitting을 방지하기 위해 GridSearchCV를 통해 아래와 같이 parameter 조합들을 넣어주었고 best parameter를 찾는다.
   ```
   param_grid = {'max_features': ['auto', 'sqrt', 'log2'],
              'ccp_alpha': [0.1, .01, .001, .0001, 0.00001],
              'max_depth' : [5, 7, 9],
              'criterion' :['gini', 'entropy']
             }
   clf.best_params_
   {'ccp_alpha': 1e-05,
   'criterion': 'gini',
   'max_depth': 9,
   'max_features': 'auto'}
   ```

## Evaluation
[우울 예측 모델]

target attribute인 ‘depression_scale’ column의 값이 고루 분포되어 있는 형태가 아닌 특정 값에만 집중되어 있어 그 외의 값의 예측 정확도가 낮은 것으로 파악된다. 또한 사용하는 attribute에 비해 사용하는 data의 수가 적다. 불균형한 데이터에서의 평가지표로 F1 score을 함께 사용한다. accuracy 80%와 F1 score 75%로 해석해보았을 때, 모델이 전체 샘플 중 60%를 정확하게 분류하고 있다.

[운동 처방 모델]

Multi-label classification을 진행했기에 단순히 Accuracy로 evaluation을 하기보다 각 라벨별 precision을 평가하는게 더 유의미하다. 그 이유를 예를 들어 설명해보자면, 정답이   [1 7 12 24] 이고 예측한 값이 [1 7 12] 라면, ‘운동 24’ 하나를 예측하지 못했다는 이유로 accuracy 계산에서는 false prediction으로 구분이 된다. 반면, 라벨 별 precision을 분석할 경우 ‘운동 1’, ‘운동 7’, ‘운동 12’는 잘 prediction 했음이 파악 가능하다. 따라서 운동 29개의 Classification Report를 생성했고, ‘micro’, ‘macro’ 방식의 precision average는 0.53, 0.46이 나옴을 알 수 있었다.

## Interpretation
우울 예측 모델에서는 80% 이상의 정확도가 산출되었지만 F1 score을 고려해보았을 때 정확하게는 60%가 분류되었다고 할 수 있다. 운동 처방 모델에서는average precision score가 0.53을 기록했다. 더 많은 data를 수집해서 accuracy와 precision score 모두 높은 값을 산출하게 된다면 해당 모델을 현재 사업 운용중인 손목닥터9988 앱과 같은 건강 앱들의 기능 중 일부로 배포하거나 의료서비스와 연계하여 스트레스 지수가 높을 것으로 판단되는 사람들에게 운동처방을 할 수 있다. 이를 통해 스트레스를 해소, 미래의 불안/우울로 인한 사회적비용을 감소, 개인의 건강한 삶을 유도할 것으로 기대된다. 또한 기존의 스트레스 지수를 감소시키기 위한 운동의 의무성만 강조되었던 의료상식과 달리 데이터로써 구체적인 목표점을 제시함에 있어 사회에 기여할 수 있을 것으로 예측된다.


[참고문헌]

[1] 박선철. (2018). 운동의 항우울효과. 신경정신의학, 57(2), 139-144.

[2] 허재헌. (2019). 성인의 신체활동과 스트레스 인지정도: 2017년 국민건강영양조사자료를 이용한 단면연구. 스트레스 硏究, 27(4), 313-319.

[3] 최광민, 신원섭, 연평식, 조영민.(2011).산림 걷기 운동이 스트레스와 피로도에 미치는 영향.한국산림휴양학회지,15(1),61-66.

[4] 남원우 and 김병욱. (2020). 환경적 요소 선별을 통한 딥러닝 기반 우울증 분류 예측. 전기학회논문지, 69(7), 1102-1111 
