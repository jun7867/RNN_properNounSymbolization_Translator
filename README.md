## RNN_properNounSymbolization_Translator

## 고유명사 사전 이용 및 숫자 기호화를 통한 한영 번역기

캡스톤 프로젝트

팀원: 김명진(한동대학교), 남준영(한동대학교), 정희석(한동대학교), 최희열 (한동대학교 지도교수)

현재 이용되고 있는 번역기들은 고유명사를 인식하는데 어려움을 겪고 있다. 본 연구의 목적은 기존의 한/영 번역기에서 고유명사 번역시 일어나는 문제를 해결하고자 한다. RNN 번역 모델을 만들고 대명사와 고유명사는 토큰으로 변환하고 이 태그를 제외한 나머지 단어들을 번역한 후 마지막으로 대명사/고유명사 한/영 샘플 딕션너리에서 단어를 찾아 번역하는 구조이다.


### 고유명사 사전 기호화
기본적인 과정은 다음과 같다. 번역 모델이 번역할 때 입력으로 하나의 문장이 들어오면, 문장에 있는 고유명사를 기호화 한다. 번역 후 남아있는 기호를 입력문장에 있는 고유명사를 이용해 고유명사 딕셔너리 사전을 확인한 후 다시 원래 고유명사로 바꾼다.

### 1. 모델을 학습할 때

(1)인용 부호 ‘ ’로 고유명사 치환 인용 부호안에 들어가 있는 단어를 고유명사로 치환한다. 예를 들어 ‘겨울 왕국'과 같은 경우 일반적인 ‘Winter Kingdom’이 아닌 고유 명사 취급하여 ‘frozen’으로 번역되게끔 한다.

(2)대문자로 된 명사를 고유명사 치환 대문자로 시작하는 명사를 고유명사로 치환한다. 예를 들어 “Kim is a father” 라는 문장이 입력으로 들어오면 대문자로 된 명사를 기호화하여 입력을 “_N2 is a father”로 바꾸고, 그 후 번역 모델로 번역하여 “_N2 는 아빠이다"라는 번역 결과를 얻고, 기호를 고유명사 사전을 통해 바꾸어 “김씨는 아빠이다"라는 최종 번역문을 얻는다.

(3)입력문장 고유명사 classifier 입력문장에서 고유명사를 구별하기 위한 classifier 를 만들어 해당 문장을 번역시 고유명사 classifier 로 고유명사 부분을 찾아 따로 고유명사 dictionary 를 사용해 번역을 한다.

(4)입력문장에서 고유명사가 없을 경우 입력문장에서 고유명사가 없을 경우 모든 명사를 고유명사로 취급한 후 고유명사 사전에 포함되어 있으면 고유명사로 치환, 고유명사 사전에 포함되어 있지 않는 경우 일반 명사로 취급하여 번역한다. 이를 통해 인용 부호나 대문자로 되어있지 않은 고유명사를 찾을 수 있다.

### 2. 번역할 때

학습이 끝난 후 새로운 문장을 번역 할 때는 학습 데이터를 다룰 때와는 다른 알고리즘을 적용한다. 학습할 때와 달리 입력 문장만 주어지기 때문에 입력 문장의 고유명사를 기호화하고, 기호화한 문장을 모델을 사용해 번역한다. 나온 번역문의 기호를 다시 원래 고유명사로 치환한다.


### 학습 순서

1. Tokenize (./tokenize.perl)
2. 고유명사 기호화
3. BPE (./data_prepare.sh)
4. Train (./train.sh)

---


해당 번역기는 [AIhub](http://www.aihub.or.kr/) 에서 한국어-영어 번역 말뭉치 데이터(약 110만 문장)를 받아와 해당 데이터에서 빈도수가 높은 순으로 450개 고유명사를 고유명사 사전에 등록하여 진행했습니다.

고유명사 기호화를 사용하지 않았을때의 Bleu score: 26.55

고유명사 기호화를 사용했을 때의 Bleu score: 28.48


- 추후 Transformer model로 변경 완료.
