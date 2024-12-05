# NLP 일상 대화 요약 프로젝트
## Team

| ![박패캠](https://avatars.githubusercontent.com/u/156163982?v=4) | ![이패캠](https://avatars.githubusercontent.com/u/156163982?v=4) | ![최패캠](https://avatars.githubusercontent.com/u/156163982?v=4) | ![김패캠](https://avatars.githubusercontent.com/u/156163982?v=4) | ![오패캠](https://avatars.githubusercontent.com/u/156163982?v=4) |
| :--------------------------------------------------------------: | :--------------------------------------------------------------: | :--------------------------------------------------------------: | :--------------------------------------------------------------: | :--------------------------------------------------------------: |
|            [마서연](https://github.com/UpstageAILab)             |            [문기중](https://github.com/UpstageAILab)             |            [이민석](https://github.com/UpstageAILab)             |            [진주영](https://github.com/UpstageAILab)             |            [최수민](https://github.com/UpstageAILab)             |
|                            kobart MODEL TEST                             |                            DATA CLEAN, MODEL TEST                             |                            T5 MODEL TEST                             |                            EDA, DATA AUG                             |                            bart MODEL TEST                             |

## 1. Competiton Info

### Overview

- 주어진 데이터를 활용하여 일상 대화에 대한 요약을 효과적으로 생성하는 모델을 개발하는 대회

### Timeline

- 2024.11.18 10:00 ~ 2024.11.28 19:00

## 2. Data descrption

### Dataset overview

ROW
- train : 12457
- dev : 499
- test : 250
- hidden-test : 249

COLUMN
- fname : 대화 고유번호 입니다. 중복되는 번호가 없습니다.
- dialogue : 최소 2명에서 최대 7명이 등장하여 나누는 대화 내용입니다. 각각의 발화자를 구분하기 위해#Person”N”#: 을 사용하며, 발화자의 대화가 끝나면 \n 으로 구분합니다. 이 구분자를 기준으로 하여 대화에 몇 명의 사람이 등장하는지 확인해보는 부분은 EDA 에서 다루고 있습니다.
- summary : 해당 대화를 바탕으로 작성된 요약문입니다

### EDA , Data Processing

스페셜 토큰 추가
'#Person1#', '#Person2#', '#Person3#', '#PhoneNumber#', '#Address#', '#PassportNumber#', '#Person4#', '#Email#', #CardNumber#',' #DateOfBirth#', '#CarNumber#', '#Person6#', '#Person5#', '#SSN#', '#Person7#'

스페셜 토큰 오류 수정
![image](https://github.com/user-attachments/assets/233c1770-9de3-41b1-9dfc-977a700cfb05)

한글 자음 모음 수
![스크린샷 2024-11-25 165519](https://github.com/user-attachments/assets/d2159dc7-4cc6-4096-88c7-161454c0ea4f)

텍스트 토큰화된 길이 분포
토크나이저 : digit82/kobart-summarization
Train 데이터
![image](https://github.com/user-attachments/assets/bbad90c6-77fd-4554-ab79-1466fe4a8cd8)
![image](https://github.com/user-attachments/assets/caee2c73-940d-4130-8f52-0a07892542bb)

Test 데이터
![image](https://github.com/user-attachments/assets/5aa51e66-2917-4363-9e08-25ee148dad7b)
![image](https://github.com/user-attachments/assets/e7a95783-59d8-4228-b666-17e835eefd94)

### Train Data Augmentation
nltk, KorEDA 사용 
- 임의 단어 삭제 RD
- 단어 위치 변경 RS
- 임의 단어 삽입 RI
- 문장 부호 삽입 AEDA

googletrans, deepl 사용
- 역번역 BT

pseudo label
Ai hub https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=&topMenu=&aihubDataSe=data&dataSetSn=544

## 4. Modeling

### Model descrition
#### Kobart
- digit82/kobart-summarization
- gogamza/kobart-summarization
- gogamza/kobart-base-v2
- EbanLee/kobart-summary-v2
- EbanLee/kobart-summary-v3

#### T5
-pko-t5

#### LLM API
- Solar-mini
- Solar-pro

#### LLM QLora Fine-tunning
- EleutherAI/polyglot-ko-12.8b

### Modeling Process
Kobart
digit82/kobart-summarization	-	1st
gogamza/kobart-summarization
gogamza/kobart-base-v2
EbanLee/kobart-summary-v2
EbanLee/kobart-summary-v3		-	2nd
![image](https://github.com/user-attachments/assets/90ea8fa2-5218-4781-8755-afa2218c07de)

Kobart
주로 digit82/kobart-summarization 모델로 실험

변동
Num_train_epochs          	5 ~ 20
Learning_rate			1e-3 ~ 1e-5
No_repeat_ngram_size		2 ~ 15
Num_beams			2 ~ 6

고정
repetition_penalty 		1.5
lr_scheduler_type		cosine
optim				adamw_torch

![image](https://github.com/user-attachments/assets/558b3624-6f54-4a1e-9028-dbb64d3465c1)

