# Modeling-the-Effectiveness-of-New-Anticancer-Drugs-Using-Survival-Analysis-and-Logistic-Regression

cf) 연구 과제의 특성상 연구에 진행된 데이터 정보와 분석 결과, 코드 등은 공유할 수 없습니다. 아래 내용은 분석과 모델링의 전반적인 사항과 사용한 기법과 그에 따른 결과들을 기록하고 있습니다.
 
![khd logo](https://github.com/gunlyungyou/Modeling-the-Effectiveness-of-New-Anticancer-Drugs-Using-Survival-Analysis-and-Logistic-Regression/blob/main/imgs/1.png)

해당 연구과제는 폐암의 한 종류인 비소세포폐암을 치료하기 위해 기존의 화학적 항암치료와는 다른 면역관문억제제의 국내 도입을 위한 유효성 입증 프로젝트이다.본 연구의 대상자는 2017년 ~ 2018년 면역관문억제제를 투여받은 환자들을 대상으로하며 삼성서울병원, 서울아산병원 등 전국의 20여개 상급 의료 기관의 참여를 바탕으로 DW가 구축 되었다.

분석은 아래와 같이 크게 세 가지 측면에서 진행이 되었다.

## 1.Demographic Statistic Analysis

대상자들의 인구통계학적 정보에 대한 통계 분석은 보통 아래와 같은 형식으로 진행이 된다.

![khd logo](https://github.com/gunlyungyou/Modeling-the-Effectiveness-of-New-Anticancer-Drugs-Using-Survival-Analysis-and-Logistic-Regression/blob/main/imgs/2.png)

기본적인 골자는 비교를 원하는 두 군이 각 특성에 대해서 동일한 분포를 갖고 있는지 아닌지 통계 분석을 진행하는 것이다.
A/B test의 기본 특성상 서로 다른 처치가 진행되는 집단들에 대해서는 baseline effect를 제거하기 위해서 랜덤하게 진행되기 때문에 이러한 분석이 필요 없을 수 있지만,
**Retrospective하게 진행되는 Case Study 같은 경우 서로 다른 처치를 받은 두 군이 Effect를 객관적으로 비교하기에 적당한지 혹은, 고려해야할 만한 Confounder는 없는지 확인하는 작업을 거쳐야 한다.** 만약 중대하게 각 군이 다르다는 검정 결과를 얻는다면 이에 맞는 분석법을 이용해야 하기 때문이다.

해당 연구 과제에서 살펴본 대상자들의 특성은 약 130여개로, 인구통계학적 정보 이외에도 처치에 영향을 줄 수 있는 외부 약물과 병에 대한 유무 등에 대한 통계적 동질성을 검정하였다.
연속형 변수에서는 **T-test**를, 범주형 변수에 대해서는 **Chi-squre test** 등과 같이 각 변수에 맞는 통계 분석법을 활용하여 검정을 진행하였다. 그 결과 통계적으로 유의미하게 차이가 있는 특성들이 일부 발견되었지만, 연구 교수님들의 판단과 수주 기관과의 협의를 통해 전반적인 효과 분석에 영향을 미치지 않을 것이라는 판단을 내렸다.

일부 Missing Value를 갖고 있는 특성들이 있었으나 그 수가 많지 않아 Imputation은 진행하지 않았다.

## 2.Modeling Effectiveness Using Logistic Regression

Effectiveness를 입증하기 위한 다양한 타겟 변수들을 이용하였지만, 가장 핵심 변수는 ORR(객관적 반응률)으로 반응의 유무에 따른 Binary Variable로 표현이 가능하다. 때문에, 이진 범주형 변수가 타겟 변수일 때 이용할 수 있는 가장 간단한 모델링 기법은 Logistic Regression을 이용하여 각 군에 따라 의미있는 반응을 보이고 있는지 확인해 보았다. 이러한 과정은 추후 동일한 질병의 환자들을 대상으로 해당 처치에 유의미한 영향을 미치는 요인들과 그 효과를 예측하여 환자의 반응률을 높이는데 그 목적이 있다.

모델링을 위한 변수는 앞서 통계 분석에 이용한 50개의 특성 중 Domain Knowledge가 있는 연구 교수님들께서 일부 선정을 진행해주셨다. 그 이후 Univariate Modeling 부터 시작해서 유의마한 변수들을 추려내고 Forward Feature Selection 방식을 이용하여 Multivariate Modeling을 점차 진행하였다.

![khd logo](https://github.com/gunlyungyou/Modeling-the-Effectiveness-of-New-Anticancer-Drugs-Using-Survival-Analysis-and-Logistic-Regression/blob/main/imgs/3.png)

그 결과 흡연, 이전 방사능 치료, 이전 암 부위 등의 반응에 유의미한 영향을 미치는 변수들을 찾아낼 수 있었다.

## 3.Modeling Effectiveness Using Survival Analysis

Effectiveness를 입증하기 위한 두번때 변수는 각 처치를 받은 집단의 Overall Survival(OS) 즉, 생존 기간을 확인하는 것이었다. 이를 확인하기 위해서 통계학에서 Survival Analysis를 진행할 때 가장 많이 이용하는 Kaplan Meier를 이용하였다.

![khd logo](https://github.com/gunlyungyou/Modeling-the-Effectiveness-of-New-Anticancer-Drugs-Using-Survival-Analysis-and-Logistic-Regression/blob/main/imgs/4.png)

OS도 마찬가지로 생존 or 사망의 Binary Variable로 표현이 가능하다. 하지만, 생존 자료의 특성상 Censored Data의 처리와 Time Dependent한 특성을 보이기 때문에 일반적인 이진 범주형 자료가 타겟 변수일 때 이용하는 방법들은 이용하기 어렵다. 통계학에서는 주로 이러한 상황에서 Cox Proportional Hazard Regression을 이용하여 시간의 특성과 Censored Data를 다루고 있다.

![khd logo](https://github.com/gunlyungyou/Modeling-the-Effectiveness-of-New-Anticancer-Drugs-Using-Survival-Analysis-and-Logistic-Regression/blob/main/imgs/5.png)

그 결과 나이, 암 병기 등 10가지 정도의 생존에 유의미한 영향을 미치는 특성을 찾아낼 수 있었다.
이러한 과정은 위와 마찬가지로 추후 동일한 질병의 환자들을 대상으로 해당 처치에 유의미한 영향을 미치는 요인들과 그 효과를 예측하여 환자의 생존률을 높이는데 그 목적이 있다.


## Review

과정과 핵심 내용을 최대한 간략히 기술하였지만, 실제 보고서의 내용은 126p에 이르고 엑셀로 작성한 sheet만 13개가 되는 방대한 양의 분석 보고서이다. 위에서 소개한 ORR과 OS 외에도 총 7개가 되는 타겟 변수들을 정의하고 통계 분석 후 모델링을 진행하였다. 이와 더불어 130여 개의 Deomographic 변수들을 통계 분석하는 과정을 통해서 석사 과정까지 이수하고 연구하면서 배운 통계 분석법을 총망라하여 이용하였고 Python, R, SAS, SPSS 할 것 없이 이용할 수 있는 모든 언어와 툴을 이용하여 분석 및 모델링을 진행하였다. 해당 연구 과제는 해외 Bio SCI급 저널 Publish 또한 목표로 했기에 관련된 분석법과 모델링 기법들에 대한 심도 깊은 수학적 고민과 실용성에 대한 의논 후에 적용이 이루어졌기 때문에 개인적으로도 많은 공부와 연구가 되었던 연구 과제였다. 해당 결과는 실제로 Publish에 성공하여 제2저자로 이름을 올렸다. 그리고 해당 연구 과제의 연구 선상으로 전통적인 통계학 방법론이 아닌 딥러닝 방법을 이용한 생존 분석에 대한 연구를 진행해서 학위 논문으로 작성하였다. 생존 분석은 실제로 고객 이탈과 같은 시간에 따른 반응 변수에 대한 예측에 많이 쓰이는 분야이며, 이 때의 연구 결과가 넥슨에서의 업무에도 적용하여 성과를 내는데 큰 도움이 되었다.
