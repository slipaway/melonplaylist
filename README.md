# 모델 설명
test 데이터셋의 구조는 크게 5가지로 나눠진다. i) 플레이리스트 제목과 태그만 있는 경우, ii) 제목만 있는 경우, iii) 곡과 태그만 있는 경우, iv) 곡만 있는 경우, v) 제목, 곡, 태그 모두 비어있는 경우. '할리스' 팀은 이에 대해 세 가지 모델을 구상하여 각 경우에 따라 모델을 달리 사용하기로 하였다.

## 1. Train2vec Model
* 모델 파일: Train2vec_Model.ipynb
* i), ii) 경우에서 곡을 예측하는데 사용됨.
* ii), iv) 경우에서 태그를 예측하는데 사용됨.

## 2. Tag2vec Model
* 모델 파일: tag2vec model.ipynb
* Word2vec 단어 임베딩 방법을 활용하여, train 데이터셋의 '태그'를 
* i), iii) 경우에서 태그를 예측하는데 사용됨.

## 3. Matrix Factorization Model (MF model)
* 모델 파일: MF_model_song_and_tag_prediction.ipynb
* iii), iv) 경우에서 곡을 예측하는데 사용됨.

# 각 모델에서 처리하지 못한 데이터, 어떻게 처리하였나?
## 4-1. Train2vec Model
1. Train2vec_>_Tag2vec_>_Matrix_Factorization_Model.ipynb
출력했음에도 태그가 10개가 되지 못함 -> Tag2vec -> MF model

2. Train2vec_>_Matrix_Factorization_>_Train2vec_Model.ipynb
둘째, Train2vec model을 사용하기 위해 test 데이터셋을 전처리하는 과정에서 제목, 곡, 태그 정보가 사라지게 되는 문제가 있었다. 즉, i), ii), iv) 경우인데도 전처리 과정을 거치면서 v)로 바뀌는 데이터가 있었던 것이다. 단어 임베딩 과정에서 학습된 단어여야지만 이 모델이 적용 가능한데, test 데이터셋의 제목, 곡, 태그가 모두 train 데이터셋에 등장하지 않아 벌어진 일로 추측된다. 이런 문제를 해결하기 위해 MF model을 추가적으로 사용했다. test 데이터셋에 주어진 태그나 곡을 바탕으로 MF model은 "Train2vec model에 적용할 수 있는 태그나 곡"을 제공할 수 있다. 이 MF model에서 제공받은 데이터셋을 가지고 Train2vec 모델로 태그 혹은 곡을 예측하려 했다.

그러나 이 방법으로 성공한 것은 오직 i) 경우 뿐이었다. MF model은 제목을 기반으로 태그나 곡을 추천할 수 없어 ii) 경우에 해당할 땐 이 방법을 사용할 수 없었다. 또, iv) 경우에는 MF model로 곡 100개를 추출하였으나 알고보니 이것들 모두 단어 임베딩 과정에서 학습하지 못한 곡들이었다. 이렇게 v)가 되어버린 몇몇 ii), iv) 경우는 나중에 다른 v) 경우와 함께 처리해주기로 했다. (5. Empty 참고)

참고로 성공한 i) 경우, 기존 test 데이터셋에 있는 태그를 MF model에 넣어 'Train2vec model에 적용할 수 있는 태그 10개'를 추출하였다. 이후 이 태그를 train2vec model에 적용하여 (기존에 하고자 했던) 곡 예측을 진행하였다.

## 4-2. Tag2vec Model

출력했음에도 비어있는 문제 -> MF model로 태그 출력하기로
tag2vec에서 결과를 출력하는데 실패한 행(row)이 있다. 태그를 뽑지 못한 이유로는 "그 행에 있는 태그 모두가 train 과정에서 학습되지 못한 태그였기 때문"인 것으로 보인다. 이 데이터의 경우, MF 모델을 사용하여 태그를 메우기로 결정하였다. MF 모델에서 태그를 출력하기 위해 tag2vec 결과물이 없는 플레이리스트 id를 뽑아내었다. (이후 결과는 MF 모델 참고)

## 5. Empty
Empty_case.ipynb

# 최종결과
final_result.ipynb
