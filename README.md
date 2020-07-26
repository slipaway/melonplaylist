# 모델 설명
## 1. Train2vec Model

## 2. Tag2vec Model

## 3. Matrix Factorization Model (MF model)

# 각 모델에서 처리하지 못한 데이터, 어떻게 처리하였나?
## 4-1. Train2vec Model
1. 출력했음에도 태그가 10개가 되지 못함 -> Tag2vec -> MF model
2. MF model -> Train2vec model

## 4-2. Tag2vec Model
1. 출력했음에도 비어있는 문제 -> MF model로 태그 출력하기로
tag2vec에서 결과를 출력하는데 실패한 행(row)이 있다. 태그를 뽑지 못한 이유로는 "그 행에 있는 태그 모두가 train 과정에서 학습되지 못한 태그였기 때문"인 것으로 보인다. 이 데이터의 경우, MF 모델을 사용하여 태그를 메우기로 결정하였다. MF 모델에서 태그를 출력하기 위해 tag2vec 결과물이 없는 플레이리스트 id를 뽑아내었다. (이후 결과는 MF 모델 참고)

## 5. Empty

# 최종결과
