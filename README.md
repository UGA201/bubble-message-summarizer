![](https://images.velog.io/images/hability24/post/669ae4eb-88a7-4846-8a4d-b9b337892bfa/image.png)

__*상세한 내용은 Colab 파일(Bubble_Summarizer.ipynb)을 참고해주세요.*__

<br>

## [ 프로젝트 목적 ]
케이팝 팬덤을 대상으로 하는 유료 소통 플랫폼 서비스가 각광받고 있다.

그 중 두드러진 성장세를 보이고 있는 1:1 채팅 서비스 플랫폼(버블, 유니버스 등)의 기능 확장을 목적으로,

팬들에게서 온 메시지를 요약해서 아티스트에게 보여주는 기능을 기획하였다.

## [ 프로젝트 기획 배경 ]
엔터사업에서 새로운 아이디어를 통해 수익을 창출하고 있는 디어유 버블 서비스가 흥미롭고, 

향후 이런 서비스를 개발해보고 싶은 마음에 프로젝트를 기획하게 되었다.

버블 앱을 직접 사용해보면서 사용자를 위한 추가적인 기능이 무엇이 있을까 고민하다 메시지 요약을 주제로 선정했다.

## [ 프로젝트 분석방법 ]
KoNLPy를 활용한 토큰화

CountVectorizer, TfidfVectorizer를 활용한 주요 토픽 추출

TextRank, lexrankr을 활용한 주요 문장 요약 및 비교 분석


![](https://images.velog.io/images/hability24/post/ee93fc46-6d71-4ad1-a247-1cc13fa1317e/image.png)

![](https://images.velog.io/images/hability24/post/e305c826-c8a3-467e-add5-5cec9dbaa276/image.png)

## [ 데이터 취득 ]
팬들이 보내는 메시지를 취득할 방법을 고민해 본 결과,

(x) 1. 서비스 운영사에 데이터 요청: 유저 동의 없이 데이터 받기란 불가능할 것으로 예상

(x) 2. 임의로 데이터 생성하여 사용: 프로젝트 주제에 맞는 충분한 데이터 생성하기 어려움

(o) 3. 트위터API 활용: 해시태그를 통해 아티스트별 팬덤 메시지 취득

유료 소통 서비스는 글자수, 답장 횟수 제한 등이 있어, 추가적인 메시지를 보내려 하는 팬들은 해당 아티스트의 특정 해시태그를 사용하여 트윗 작성한다는 특징을 반영했다.

따라서, Twitter Hash tag를 키워드로 사용하여 아티스트에게 보내는 팬들의 메시지를 수집할 수 있다.

![](https://images.velog.io/images/hability24/post/a543849b-5d4c-48cc-a099-06ab5e4f4b63/image.png)

![](https://images.velog.io/images/hability24/post/9a43c645-66b5-4bf9-ad37-564974ce7741/image.png)

![](https://images.velog.io/images/hability24/post/01783617-49e8-4294-b9f5-4b9e808f39ac/image.png)

## CountVectorizer
BoW(Bag-of-Words) 개념을 적용하여 단어들의 출현 빈도를 기반으로 문서를 벡터화한다.

해당 토큰(단어)이 문서에 얼마나 있는지를 카운트한다.

![](https://images.velog.io/images/hability24/post/cb1dec1d-cecc-4bd8-a514-998126ef615d/image.png)

## TF-IDF(Term Frequency – Inverse Document Frequency) Vectorizer
CountVectorizer는 단어 갯수를 그대로 카운트한 반면,

TF-IDF는 모든 문서에 공통적으로 들어있는 단어의 경우 문서 구별 능력이 떨어진다고 보아 가중치를 축소하는 방법이다.

![](https://images.velog.io/images/hability24/post/68072543-3b73-41fe-b1de-93c5b7072040/image.png)

결과가 주요 단어로 나오기 때문에 문맥을 이해하기 어렵다.

따라서, 단어가 아닌 문장으로 요약하는 방식이 더 적합하겠다는 판단을 했다.

## [ TextRank Summarizer ]
TextRank를 활용하면 TF-IDF를 활용하여 각 단어의 가중치를 계산한 후 그래프를 생성하여 각 문장의 랭킹을 계산함으로써 주요 문장으로 요약할 수 있다.

TextRank는 구글의 검색알고리즘인 PageRank를 텍스트에 적용한 것이며,

PageRank는 웹페이지를 인용하고 있는 다른 페이지들이 가진 페이지 랭크를 정규화한 값의 합으로 해당 웹페이지의 페이지 랭크를 구한다.

따라서 TextRank는 각 문장들을 노드로, 문장들 간 유사도를 간선의 값으로 사용하여 그래프를 만든 후 PageRank 알고리즘을 적용하여 문장의 랭킹을 계산한다.

![](https://images.velog.io/images/hability24/post/294bc99f-e4a2-4566-a294-46b84eba42ba/image.png)

## [ lexrankr ]
LexRank는 추출 기반 다중 문서 요약 방법으로,

TextRank의 그래프에 클러스터링을 적용하여 주제가 다양한 문서를 요약하는데 사용된다.

크기가 큰 클러스터부터, 각 클러스터 내에서 가장 랭킹이 높은 문장을 뽑는다.

![](https://images.velog.io/images/hability24/post/2a074ec5-c06a-4864-8154-634384e68a60/image.png)

![](https://images.velog.io/images/hability24/post/eb0b3c88-9595-4dc8-a839-1410c2c7125a/image.png)
