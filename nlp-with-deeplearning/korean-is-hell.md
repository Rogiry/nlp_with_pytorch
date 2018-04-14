# 무엇이 한국어 NLP를 더욱 어렵게 만드는가?

## 교착어 \(어순\)

| 종류 | 대표적 언어 | 특징 |
| --- | --- | --- |
| 교착어 | 한국어, 일본어, 몽골어 | 어간에 접사가 붙어 단어를 이루고 의미와 문법적 기능이 정해짐 |
| 굴절어 | 라틴어, 독일어, 러시아어 | 단어의 형태가 변함으로써 문법적 기능이 정해짐 |
| 고립어 | 영어, 중국어 | 어순에 따라 단어의 문법적 기능이 정해짐 |

한국어는 **교착어**에 속합니다. 어순이 중요시되는 영어/중국어와 달리 어근에 접사가 붙어 의미와 문법적 기능이 부여됩니다. \(굴절어의 경우에는 형태 자체가 변함으로써, 어근과 접사가 분명하게 구분되는 교착어와 다릅니다.\) 따라서, 아래와 같은 재미있는 예시의 형태도 가능합니다.

![](/assets/intro-why-korean-hell-example.png)  
\[Image from [나무위키\(교착어\)](https://namu.wiki/w/교착어)\]

위의 예처럼 접사에 따라 단어의 역할이 정의되기 때문에, 상대적으로 어순은 중요하지 않습니다. 아래는 4개의 단어가 나타날 수 있는 모든 조합을 적은 것 입니다. "간다"가 "밥을"뒤에 붙어 수식할 때를 제외하면 모두 같은 의미의 문장이 됩니다.

| 번호 | 문장 | 정상여부 |
| --- | --- | --- |
| 1. | 나는 밥을 먹으러 간다. | O |
| 2. | 간다 나는 밥을 먹으러. | O |
| 3. | 먹으러 간다 나는 밥을. | O |
| 4. | 밥을 먹으러 간다 나는. | O |
| 5. | 나는 먹으러 간다 밥을. | O |
| 6. | 나는 간다 밥을 먹으러. | O |
| 7. | 간다 밥을 먹으러 나는. | O |
| 8. | 간다 먹으러 나는 밥을. | O |
| 9. | 먹으러 나는 밥을 간다. | X |
| 10. | 먹으러 밥을 간다 나는. | X |
| 11. | 밥을 간다 나는 먹으러. | X |
| 12. | 밥을 나는 먹으러 간다. | O |
| 13. | 나는 밥을 간다 먹으러. | X |
| 14. | 간다 나는 먹으러 밥을. | O |
| 15. | 먹으러 간다 밥을 나는. | O |
| 16. | 밥을 먹으러 나는 간다. | O |

이러한 특징은 Parsing, POS Tagging 부터 Language Modeling에 이르기까지 한국어 NLP를 훨씬 어렵게 만드는 이유 중에 하나 입니다.

또한 접사가 붙어 같은 단어가 다양하게 생겨나기 때문에, 하나의 어근에서 생겨난 비슷한 의미의 단어가 정말 많이 생성됩니다. 따라서 이들을 모두 다르게 처리할 수 없기 때문에, 추가적인 segmentation을 통해서 같은 어근에서 생겨난 단어를 처리하게 됩니다. 이와 관련한 내용은 Preprocessing 챕터에서 다루도록 하겠습니다.

> 읽을거리:
>
> * [http://zomzom.tistory.com/1074](http://zomzom.tistory.com/1074)
> * [https://m.blog.naver.com/reading0365/221057575669](https://m.blog.naver.com/reading0365/221057575669)

## 띄어쓰기의 어려움

![](/assets/intro-why-korean-hell-my-bro.png) ![](/assets/intro-why-korean-hell-human-meat.png)

애초에 동양권에서는 띄어쓰기라는 것이 존재하지 않았고 근대에 들어와서 도입된 것이기 때문에, 띄어쓰기에 맞춰 발전 해 온 언어는 아닙니다. 따라서 띄어쓰기에 대한 표준이 계속 바뀌어 왔기 때문에, 사람마다 띄어쓰기를 하는 것이 다를 뿐더러, 심지어는 띄어쓰기가 아예 없더라도 해석이 가능하기도 합니다. 결국, 위에서처럼 추가적인 segmentation을 통해서 띄어쓰기를 정제(normalization) 해 주는 process가 마찬가지로 필요하게 됩니다.

## 평서문과 의문문의 차이

|언어|평서문|의문문|
|-|-|-|
|영어|I ate my lunch.|Did you have lunch?|
|한국어|점심 먹었어.|점심 먹었어?|

물론 한국어에서도 의문문을 나타낼 수 있는 접사가 있습니다 -- 니. 따라서, **점심 먹었니**라고 하면 굳이 물음표가 붙지 않더라도 의문문인 것을 알 수 있습니다. 하지만 많은 경우에 그냥 의문문과 평서문이 같은 형태의 문장을 띄는 것이 사실입니다. 따라서, 마침표나 물음표가 붙지 않을 경우에는 알 수가 없는 경우가 많습니다. 특히나, 음성인식의 결과물로 나오는 text의 경우에는 더욱더 어렵습니다.

## 주어 생략

영어는 기본적으로 특성상 명사가 굉장히 중요시 됩니다. 따라서 정말 특별한 경우를 제외하고는 주어가 생략되는 경우가 없습니다. 하지만 한국어는 동사를 중요시하기 때문에, 주어가 자주 생략됩니다. 인간은 context 정보를 잘 활용하여 생략된 정보를 메꿀 수 있지만, 컴퓨터는 할 수가 없습니다. 따라서 위의 평서문과 의문문의 예에서도 볼 수 있듯이 한국어는 주어가 생략 되었는데, 컴퓨터는 누가 점심을 먹었고 누구에게 점심을 먹었냐고 물어보는지 알 수가 없습니다. 따라서 기계번역을 비롯하여 문장의 정확한 의미를 파악하는데 상당히 어렵게 됩니다.

> 읽을거리: 
- http://www.hani.co.kr/arti/society/schooling/261322.html
- https://namu.wiki/w/%EC%A3%BC%EC%96%B4%EB%8A%94%20%EC%97%86%EB%8B%A4

## 한자 기반의 언어

|언어|단어|조합|
|-|-|-|
|영어|Concentrate|con(=together) + centr(=center) + ate(=make)|
|한국어|집중(集中)|集(모을 집) + 中(가운데 중)|

이와 같이 원래 한국어도 한자 기반의 언어이기 때문에, 한자의 조합으로 이루어진 단어들이 많습니다. 이러한 단어들은 각 글자가 의미를 지니고 있고 그 의미들이 합쳐져 하나의 단어의 뜻을 이루게 됩니다. 이것은 영어에서도 마찬가지 입니다. "water"와 같이 잉글로색슨족의 언어가 기원인 단어가 아니라면, latin 기반의 단어들은 각기 뜻을 가진 sub-word들이 합쳐져서 하나의 단어의 의미를 이루게 됩니다.

하지만 문제는 한글이 한자를 대체하면서 생겨났습니다. 한자는 **표어 문자**입니다. 문자 하나당 하나의 단어를 나타냅니다.   읽는 소리는 같을 지라도, 형태와 그 뜻은 다릅니다. 하지만 이것을 **표음 문자**인 한글이 나타내게 되면서, **정보의 손실**이 생겨버렸습니다. (사실 표음 문자가 가장 발달한 글자의 형태 입니다.) 인간은 이러한 정보의 손실로 생겨난 모호성(ambiguity)의 문제를 context를 통해서 효과적으로 해소할 수 있지만, 컴퓨터는 그렇지 못합니다. 따라서 다른 언어에 비해서 중의성이 더 가중되어 버렸습니다.

|Type|Text|
|-|-|
|원문|저는 여기 한 가지 문제점이 있다고 생각합니다.|
|형태소에 따른 segmentation|저 는 여기 한 가지 문제점 이 있 다고 생각 합니다 .|
|count based subword segmentation|▁저 ▁는 ▁여기 ▁한 ▁가지 ▁문 제 점 ▁이 ▁있 ▁다고 ▁생각 ▁합니다 ▁.|

Preprocessing 챕터에서 다루겠지만, 이렇게 마지막까지 subword level로 segmentation할 경우에, 더욱 중의성 문제가 가중되어 버립니다. 

**문제점(問題點)**이라는 단어가 **문(問, 물을 문) 제(題, 제목 제) 점(點, 점 점)**이라고 각각 segmentation 되었습니다. 하지만 결제(決濟)의 **제(濟, 건널 제)**도 있고, 제공(提供)의 **제(提, 끌 제)**도 있을 겁니다. 그런데 neural network에서 **제**라는 token은 결국 embedding vector로 변환이 될 겁니다. 따라서, embedding vector는 **제(題, 제목 제), 제(濟, 건널 제), 제(提, 끌 제)** 세가지 모두에 대해서 embedding을 하게 될 것이고 저 뜻의 중앙 방향으로 vector가 애매하게 embedding 될 것 입니다. 사실, 굳이 neural network로 가지 않더라도, traditional NLP에서도 헷갈릴 것 또한 자명한 사실 입니다.